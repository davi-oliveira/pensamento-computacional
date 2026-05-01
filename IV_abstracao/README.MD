Missão Prática: "Do mundo real ao pseudocódigo"

## GovDevOps Platform — Containerização (Versão Aprofundada)

### 🔵 Nível 1: Descrição Detalhada (O processo bruto)

No contexto da GovDevOps Platform, a containerização não é apenas "empacotar uma aplicação". É um processo de padronização, segurança e rastreabilidade que envolve múltiplos atores, decisões técnicas e validações antes de qualquer código chegar ao Kubernetes.

Principais etapas e considerações:

- **Análise da aplicação**: identificar tipo (moderna ou legada), dependências do SO, bibliotecas externas, variáveis sensíveis, volumes persistentes e requisitos de rede. Aplicações legadas (Delphi/VB6) podem requerer camadas de compatibilidade.
- **Escrita do Dockerfile otimizado**: usar imagens base oficiais e mínimas (ex.: `alpine`, `distroless`), ordenar instruções para aproveitar cache, definir usuário não-root, declarar `HEALTHCHECK` e remover ferramentas de build no artefato final.
- **Multi-stage build**: usar estágios separados (build e runtime) para manter a imagem final enxuta e segura.
- **Versionamento e tagging**: aplicar tags semânticas (ex.: `v2.3.1`), hash do commit (ex.: `sha-a3f9c2`) e ambiente (`prod`, `hml`, `dev`) para rastreabilidade.
- **Push para registro (Harbor)**: enviar imagens ao projeto/organização corretos e aplicar políticas de retenção.
- **Scan de vulnerabilidades**: executar Trivy/Clair sobre camadas da imagem e bloquear imagens com CVEs Critical/High não mitigados.
- **Assinatura digital**: assinar imagens com Notary/Cosign e exigir imagens assinadas via admission controllers no cluster.
- **Promoção entre ambientes**: promover imagens `dev → hml → prod` com aprovações e re-scans a cada etapa.

#### Multi-stage build (resumo)

1. **Estágio 1: BUILD**
   - Instalar SDKs/compiladores
   - Instalar dependências de desenvolvimento
   - Compilar código e gerar artefato

2. **Estágio 2: RUNTIME**
   - Partir de imagem base limpa
   - Copiar somente o artefato final
   - Configurar variáveis de ambiente e `ENTRYPOINT`

### 🔵 Nível 2: Representação Intermediária (O fluxo lógico)

Fluxo (representação em ASCII para referência):

```text
INÍCIO
│
▼
[Receber demanda de containerização]
│
▼
[Analisar aplicação]
├── Dependências de SO
├── Bibliotecas externas
├── Variáveis de ambiente sensíveis
├── Volumes persistentes
└── Tipo: Moderna ou Legada?
	├── LEGADA (Delphi/VB6)
	│   ├── Criar wrapper script
	│   ├── Configurar init container
	│   └── Adicionar camada de compatibilidade
	└── MODERNA (Java/Go/Node)
		└── Selecionar imagem base mínima (alpine / distroless)

▼
[Escrever Dockerfile otimizado]
├── Imagem base oficial e mínima
├── Ordenar layers para cache eficiente
├── Definir usuário não-root
├── Declarar HEALTHCHECK
└── Remover ferramentas de build

▼
[Executar Multi-stage Build]
├── ESTÁGIO 1: BUILD
│   ├── Instalar SDKs e compiladores
│   ├── Instalar dependências de dev
│   ├── Compilar código-fonte
│   └── Gerar artefato final
└── ESTÁGIO 2: RUNTIME
	├── Partir de imagem base limpa
	├── Copiar APENAS o artefato
	├── Configurar variáveis de ambiente
	└── Definir entrypoint seguro

▼
[Aplicar tags semânticas]
├── Tag versão: v2.3.1
├── Tag commit: sha-a3f9c2
└── Tag ambiente: prod | hml | dev

▼
[Push para Harbor]
├── Autenticar com credenciais do pipeline
├── Enviar imagem ao projeto correto
└── Aplicar política de retenção

▼
[Scan de Vulnerabilidades - Trivy/Clair]
├── Escanear camada: SO base
├── Escanear camada: Runtime libs
├── Escanear camada: App dependencies
└── Classificar CVEs por severidade

Decisões:
- Se CVE Critical/High não mitigado → bloquear pipeline
- Se aprovado → assinar imagem e promover entre ambientes (dev → hml → prod)

FIM
```

---
