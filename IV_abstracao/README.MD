Missão Prática: "Do mundo real ao pseudocódigo"
GovDevOps Platform — Containerização (Versão Aprofundada)
🔵 Nível 1: Descrição Detalhada (O processo bruto)
No contexto da GovDevOps Platform, a containerização não é um ato simples de "empacotar uma aplicação". Ela representa um processo completo de padronização, segurança e rastreabilidade que envolve múltiplos atores, decisões técnicas e validações obrigatórias antes que qualquer código chegue ao ambiente Kubernetes.

O processo começa muito antes do Dockerfile ser escrito:

Análise da aplicação: A equipe de infraestrutura recebe a demanda de containerizar uma aplicação. Antes de qualquer coisa, ela precisa entender o tipo da aplicação (moderna ou legada), suas dependências de sistema operacional, bibliotecas externas, variáveis de ambiente sensíveis, volumes de dados persistentes e requisitos de rede. Aplicações legadas em Delphi ou VB6, por exemplo, podem exigir Wine ou camadas de compatibilidade Windows dentro do container.

Escrita do Dockerfile otimizado: Com base na análise, o engenheiro escreve um Dockerfile seguindo boas práticas: uso de imagens base oficiais e mínimas (ex: alpine, distroless), ordenação estratégica das instruções para aproveitar o cache de layers, definição explícita de usuário não-root para execução, remoção de ferramentas de build no artefato final e declaração de healthchecks.

Multi-stage build: O build é dividido em estágios isolados. No estágio de build, todas as ferramentas de compilação, SDKs e dependências de desenvolvimento são instaladas e o artefato é gerado. No estágio de runtime, apenas o artefato compilado é copiado para uma imagem base limpa e mínima. Isso garante que a imagem final não carregue ferramentas desnecessárias que aumentam a superfície de ataque e o tamanho da imagem.

Versionamento semântico e tagging: A imagem gerada recebe tags estruturadas: a versão semântica (v2.3.1), o hash do commit Git (sha-a3f9c2) e o ambiente de destino (prod, hml, dev). Isso garante rastreabilidade total — qualquer imagem em produção pode ser rastreada até o commit exato que a originou.

Push para Harbor com políticas de retenção: A imagem é enviada ao Harbor, o registro privado do órgão. O Harbor aplica automaticamente políticas de retenção (ex: manter apenas as últimas 10 versões por projeto) e organiza as imagens por projeto/órgão/ambiente, garantindo governança sobre o que está armazenado.

Scan de vulnerabilidades em múltiplas camadas: O Harbor aciona o Trivy (ou Clair) para escanear todas as camadas da imagem — sistema operacional base, bibliotecas de runtime, dependências da aplicação e até arquivos de configuração embutidos. O scan classifica as vulnerabilidades por severidade: Critical, High, Medium, Low e Negligible. A política governamental bloqueia qualquer imagem com CVEs Critical ou High não mitigados.

Assinatura digital da imagem (Notary): Imagens aprovadas são assinadas digitalmente com Notary/Cosign, garantindo que apenas imagens verificadas e não adulteradas possam ser deployadas no cluster. O Kubernetes é configurado com admission controllers que rejeitam imagens não assinadas.

Promoção entre ambientes: A imagem aprovada e assinada é promovida sequencialmente: dev → homologação → produção. Cada promoção requer aprovação explícita no pipeline e re-execução do scan, garantindo que nenhuma imagem "pule" etapas de validação.

🔵 Nível 2: Representação Intermediária (O fluxo lógico)
Copiar
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
│
├── LEGADA (Delphi/VB6)
│ ├── Criar Wrapper Script
│ ├── Configurar Init Container
│ └── Adicionar camada de compatibilidade
│
└── MODERNA (Java/Go/Node)
└── Selecionar imagem base mínima
(alpine / distroless)
│
▼
[Escrever Dockerfile Otimizado]
├── Imagem base oficial e mínima
├── Ordenar layers para cache eficiente
├── Definir usuário não-root
├── Declarar HEALTHCHECK
└── Remover ferramentas de build
│
▼
[Executar Multi-stage Build]
│
├── ESTÁGIO 1: BUILD
│ ├── Instalar SDKs e compiladores
│ ├── Instalar dependências de dev
│ ├── Compilar código-fonte
│ └── Gerar artefato final
│
└── ESTÁGIO 2: RUNTIME
├── Partir de imagem base limpa
├── Copiar APENAS o artefato
├── Configurar variáveis de ambiente
└── Definir entrypoint seguro
│
▼
[Aplicar Tags Semânticas]
├── Tag versão: v2.3.1
├── Tag commit: sha-a3f9c2
└── Tag ambiente: prod | hml | dev
│
▼
[Push para Harbor]
├── Autenticar com credenciais do pipeline
├── Enviar imagem ao projeto correto
└── Aplicar política de retenção
│
▼
[Scan de Vulnerabilidades - Trivy/Clair]
├── Escanear camada: SO base
├── Escanear camada: Runtime libs
├── Escanear camada: App dependencies
└── Classificar CVEs por severidade
│
┌─────┴──────┐
│ │
TEM Critical SEM Critical
ou High CVE ou High CVE
│ │
▼ ▼
[Bloquear [Assinar imagem
pipeline] digitalmente
│ com Notary/Cosign]
│ │
▼ ▼
[Notificar [Marcar como
dev com APROVADA no
relatório Harbor]
de CVEs] │
│ ▼
│ [Promover entre ambientes]
│ ├── DEV ✅ (automático)
│ ├── HML ✅ (aprovação manual)
│ └── PROD ✅ (aprovação manual
│ + re-scan)
│ │
▼ ▼
[Corrigir] [Imagem disponível
[Reiniciar] para deploy no K8s]
│
FIM
