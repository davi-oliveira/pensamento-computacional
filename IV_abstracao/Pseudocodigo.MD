// ============================================================
// ALGORITMO: Containerização Segura GovDevOps
// Entrada: aplicacao (objeto com metadados e código-fonte)
// Saída: imagemAprovada (pronta para deploy no Kubernetes)
// ============================================================

FUNÇÃO containerizarAplicacao(aplicacao):

    // ── ETAPA 1: Análise e classificação ──────────────────
    perfil = analisarAplicacao(aplicacao)
        → perfil.tipo          // "legada" | "moderna"
        → perfil.dependencias  // lista de libs e SO
        → perfil.segredos      // variáveis sensíveis
        → perfil.volumes       // dados persistentes

    // ── ETAPA 2: Geração do Dockerfile otimizado ──────────
    SE perfil.tipo == "legada":
        dockerfile = DockerfileFactory.criarLegado(
            wrapperScript   = gerarWrapper(aplicacao),
            initContainer   = configurarCompatibilidade(perfil),
            imagemBase      = "wine:stable-alpine"
        )
    SENÃO:
        dockerfile = DockerfileFactory.criarModerno(
            imagemBase      = selecionarImagemMinima(perfil),
            usuario         = "nonroot",
            healthcheck     = definirHealthcheck(aplicacao),
            cacheStrategy   = otimizarOrdemLayers(perfil.dependencias)
        )

    // ── ETAPA 3: Multi-stage Build ────────────────────────
    estagios = [
        Estagio("build",
            instrucoes = [
                instalarSDK(perfil.linguagem),
                instalarDependencias(perfil.dependencias),
                compilar(aplicacao.fontes)
            ],
            saida = artefato
        ),
        Estagio("runtime",
            instrucoes = [
                copiarDe("build", artefato),
                definirUsuario("nonroot"),
                definirEntrypoint(aplicacao.comando),
                injetarSegredos(perfil.segredos)
            ]
        )
    ]

    imagemBruta = executarMultiStageBuild(dockerfile, estagios)

    // ── ETAPA 4: Versionamento e tagging ──────────────────
    tags = [
        gerarTagSemantica(aplicacao.versao),        // v2.3.1
        gerarTagCommit(aplicacao.gitHash),          // sha-a3f9c2
        gerarTagAmbiente(aplicacao.ambiente)        // prod|hml|dev
    ]
    imagemTagueada = aplicarTags(imagemBruta, tags)

    // ── ETAPA 5: Push para Harbor ─────────────────────────
    Harbor.autenticar(pipeline.credenciais)
    Harbor.push(
        imagem  = imagemTagueada,
        projeto = resolverProjeto(aplicacao.orgao),
        retencao = politica(maxVersoes = 10)
    )

    // ── ETAPA 6: Scan de vulnerabilidades multicamada ─────
    resultadoScan = Harbor.scan(imagemTagueada,
        camadas = ["so-base", "runtime-libs", "app-deps"],
        engine  = Trivy
    )

    CVEsCriticos = resultadoScan.filtrar(
        severidade IN ["Critical", "High"]
    )

    SE CVEsCriticos.quantidade > 0:
        pipeline.bloquear()
        notificarDesenvolvedor(
            destinatario = aplicacao.dev,
            relatorio    = CVEsCriticos,
            acao         = "Corrigir dependências e reiniciar"
        )
        RETORNAR FALSO

    // ── ETAPA 7: Assinatura digital ───────────────────────
    assinatura = Notary.assinar(
        imagem     = imagemTagueada,
        chave      = Harbor.chavePrivada,
        politica   = "gov-br-trusted-images"
    )
    Harbor.marcarComoConfiavel(imagemTagueada, assinatura)

    // ── ETAPA 8: Promoção entre ambientes ─────────────────
    PARA CADA ambiente EM ["dev", "hml", "prod"]:

        SE ambiente != "dev":
            aprovacao = aguardarAprovacaoManual(ambiente)
            SE NOT aprovacao:
                RETORNAR FALSO

            // Re-scan obrigatório antes de produção
            SE ambiente == "prod":
                reScan = Harbor.scan(imagemTagueada)
                SE reScan.temCriticos():
                    RETORNAR FALSO

        promoverImagem(imagemTagueada, destino = ambiente)
        registrarAuditoria(
            imagem    = imagemTagueada,
            ambiente  = ambiente,
            timestamp = agora(),
            aprovador = aprovacao.responsavel
        )

    // ── RETORNO ───────────────────────────────────────────
    RETORNAR imagemTagueada {
        status       = "APROVADA",
        assinada     = VERDADEIRO,
        disponivelEm = ["dev", "hml", "prod"],
        rastreavel   = VERDADEIRO  // commit → imagem → deploy
    }

FIM FUNÇÃO
