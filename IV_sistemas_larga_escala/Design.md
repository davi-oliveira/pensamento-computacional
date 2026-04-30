# Design – Pensamento Computacional para Sistemas de Larga Escala

## Visão Geral do Sistema

Este documento detalha a aplicação dos conceitos de pensamento computacional no design da Plataforma Acadêmica Inteligente, abordando decomposição, abstração e padrões utilizados.

---

## 1. Decomposição

A decomposição foi aplicada para dividir o sistema em módulos menores e gerenciáveis:

### 1.1 Módulos Principais

| Módulo | Descrição | Responsabilidades |
|--------|-----------|-------------------|
| **Autenticação** | Gerenciamento de usuários | Login, logout, recuperação de senha, controle de acesso |
| **Gestão de Disciplinas** | Cadastro e controle de disciplinas | Matrícula, histórico, plano de aula |
| **Relatórios** | Painel da coordenação | Gráficos, estatísticas,导出 de dados |
| **Recomendação Inteligente** | Sistema de IA | Sugestões personalizadas baseadas em perfil |

### 1.2 Submódulos

- **Autenticação:**
  - Cadastro de usuários
  - Login/Logout
  - Redefinição de senha
  - Autenticação em dois fatores (2FA)

- **Gestão de Disciplinas:**
  - Matrícula em disciplinas
  - Lançamento de notas
  - Calendário acadêmico
  - Comunicados

- **Relatórios:**
  - Desempenho por aluno
  - Médias por disciplina
  - Relatórios de evasão
  - Dashboard interativo

- **Recomendação Inteligente:**
  - Análise de comportamento
  - Sugestão de disciplinas
  - Predição de desempenho
  - Recomendação de estudos

---

## 2. Abstração

### 2.1 Modelo de Classes Principal

```
┌─────────────────┐       ┌─────────────────┐
│    Usuario     │       │   Disciplina    │
├─────────────────┤       ├─────────────────┤
│ - id            │       │ - id            │
│ - nome          │       │ - codigo        │
│ - email         │       │ - nome          │
│ - tipo          │       │ - professor     │
│ - senha         │       │ - periodo       │
└────────┬────────┘       └────────┬────────┘
         │                          │
         │ 1:N                      │ N:N
         ▼                          ▼
┌─────────────────┐       ┌─────────────────┐
│   Matricula    │       │     Nota        │
├─────────────────┤       ├─────────────────┤
│ - id            │       │ - id            │
│ - usuario_id    │       │ - matricula_id  │
│ - disciplina_id │       │ - valor         │
│ - data          │       │ - data          │
│ - status        │       │ - tipo          │
└─────────────────┘       └─────────────────┘
```

### 2.2 Camadas de Abstração

1. **Camada de Apresentação (UI)**
   - Interfaces web e mobile
   - Componentes visuais
   - Interações do usuário

2. **Camada de Negócio (Business)**
   - Regras de negócio
   - Validações
   - Lógica de recomendação

3. **Camada de Dados (Data)**
   - Repositórios
   - Modelos de banco de dados
   - Queries otimizadas

---

## 3. Reconhecimento de Padrões

### 3.1 Padrões de Projeto Aplicados

| Padrão | Aplicação | Benefício |
|--------|-----------|-----------|
| **MVC** | Separação UI/Controller/Model | Manutenibilidade |
| **Repository** | Abstração de acesso a dados | Flexibilidade |
| **Factory** | Criação de objetos complexos | Baixo acoplamento |
| **Observer** | Notificações em tempo real | Atualização reativa |

### 3.2 Padrões de Interface

- **Login:** Padrão similar a sistemas bancários com validação em tempo real
- **Notas:** Estrutura inspirada em LMS (Blackboard, Moodle)
- **Dashboard:** Layout padrão de painéis analíticos

### 3.3 Padrões de Segurança

- Criptografia de senhas (bcrypt)
- Tokens JWT para autenticação
- Rate limiting em APIs
- Sanitização de inputs

---

## 4. Algoritmos Principais

### 4.1 Cálculo de Médias

```
Função calcularMedia(notas):
    soma = 0
    pesoTotal = 0
    
    Para cada nota em notas:
        soma += nota.valor * nota.peso
        pesoTotal += nota.peso
    
    Se pesoTotal > 0:
        Retornar soma / pesoTotal
    Senão:
        Retornar 0
```

### 4.2 Sistema de Recomendação

```
Função recomendarDisciplinas(aluno, historico):
    disciplinasSugeridas = []
    
    Para cada disciplina em disciplinasDisponiveis:
        pontuacao = 0
        
        // Baseado em área de interesse
        Se disciplina.area IN aluno.areasInteresse:
            pontuacao += 30
        
        // Baseado em desempenho em disciplinas relacionadas
        Se disciplina.preRequisitos IN historico.aprovadas:
            pontuacao += 40
        
        // Baseado em carga horária disponível
        Se aluno.cargaHorariaDisponivel >= disciplina.carga:
            pontuacao += 20
        
        Se pontuacao >= 50:
            adicionar(disciplinasSugeridas, disciplina)
    
    Ordenar(disciplinasSugeridas, por pontuacao decrescente)
    Retornar top 5 de disciplinasSugeridas
```

---

## 5. Decisões de Design

### 5.1 Tecnologias Selecionadas

- **Frontend:** React.js
- **Backend:** Node.js com Express
- **Banco de Dados:** PostgreSQL
- **Cache:** Redis
- **Mensageria:** RabbitMQ

### 5.2 Justificativas

| Tecnologia | Razão da Escolha |
|------------|------------------|
| React.js | Componentização, reatividade, comunidade ativa |
| Node.js | JavaScript full-stack, I/O não-bloqueante |
| PostgreSQL | Dados relacionais, ACID, escalabilidade |
| Redis | Cache de alta performance, sessões |
| RabbitMQ | Filas assíncronas, desacoplamento |

---

## 6. Considerações de Escalabilidade

### 6.1 Estratégias

- **Horizontal Scaling:** Load balancer para distribuir requisições
- **Database Sharding:** Particionamento de dados por tenant
- **Caching:** Redis para dados frequentemente acessados
- **CDN:** Entrega de assets estáticos

### 6.2 Métricas Alvo

| Métrica | Meta |
|---------|------|
| Tempo de resposta | < 200ms |
| Uptime | 99.9% |
| Usuários simultâneos | 10.000+ |
| Requisições/dia | 1M+ |

---

> **Nota:** Este documento deve ser atualizado conforme o projeto evolui.