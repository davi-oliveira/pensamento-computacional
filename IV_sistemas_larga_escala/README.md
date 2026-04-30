# Projeto – Infraestrutura DevOps para Órgãos Governamentais Brasileiros

## Descrição

Este projeto foi desenvolvido como parte da disciplina Pensamento Computacional no curso de Engenharia de Software, com a Profa. Kadidja Valéria.

O objetivo é aplicar os conceitos de pensamento computacional e engenharia de software no design de uma infraestrutura DevOps de larga escala, adequada para órgãos governamentais brasileiros que enfrentam o desafio de gerenciar centenas de aplicações legadas e modernas com recursos limitados.

## Contexto do Problema

Órgãos governamentais brasileiros (federal, estadual e municipal) enfrentam desafios únicos:

- **Centenas de aplicações legadas** em tecnologias diversas (Delphi, VB6, Java 6, .NET Framework)
- **Orçamento limitado** para modernização e manutenção de infraestrutura
- **Legislação restritiva** (LGPD, TCU, políticas de segurança da informação)
- **Equipe reduzida** de infraestrutura e desenvolvimento
- **Necessidade de alta disponibilidade** para serviços essenciais à população

## Sistema Proposto

**Nome do Sistema:** GovDevOps Platform - Plataforma de Entrega Contínua para Governo

**Descrição:**

Uma plataforma de DevOps completa que permite:
- Containerização de aplicações legadas e modernas
- Orquestração via Kubernetes com gestão via Rancher
- Pipeline CI/CD automatizado com GitOps
- Economia de recursos através de consolidação de infraestrutura
- Conformidade com legislações brasileiras de segurança

## Objetivos

- Consolidar aplicações governamentais em infraestrutura containerizada
- Reduzir custos de infraestrutura em até 70% através de otimização de recursos
- Implementar entrega contínua com governança e compliance
- Garantir conformidade com políticas de segurança da informação
- Modernizar aplicações legadas gradualmente (strangler fig pattern)

## Pensamento Computacional Aplicado

### Decomposição:
- Containerização (Docker)
- Orquestração (Kubernetes)
- Gestão de Clusters (Rancher)
- Pipeline CI/CD
- GitOps
- Service Mesh

### Reconhecimento de Padrões:
- Arquitetura de microsserviços já utilizada em e-governo
- Padrões de governo digital (gov.br, SPB, e-CAC)

### Abstração:
- Modelo de camadas para infraestrutura
- Diagrama de componentes para pipeline DevOps

### Algoritmos:
- Otimização de recursos de cluster
- Auto-scaling baseado em métricas
- Blue-Green deployment

## Metodologia de Desenvolvimento

- **Metodologia:** Scrum
- **Sprints:** 2 semanas
- **Ferramentas:** GitHub Projects, Issues, Kanban

## Desafios Identificados

- Escalabilidade para milhares de usuários simultâneos.
- Segurança de dados sensíveis (princípios de Saltzer & Schroeder).
- Integração com sistemas externos (bibliotecas digitais, APIs).

## 📂 Estrutura do Repositório

```
Projeto_PensamentoComputacional_LargaEscala/
│
├── README.md              # Documentação principal
├── Design.md              # Decomposição, abstração e padrões aplicados
├── Diagrama.png           # Diagrama UML ou fluxograma
├── Desafios.md            # Lista de desafios e soluções propostas
└── src/                   # (Opcional) Protótipo ou código inicial
```

## 📅 Entrega

- **Data:** Aula de 30/04/2026
- **Local:** Repositório GitHub da disciplina
- **Commit:** "Entrega Projeto Aula – DevOps para Governo Brasileiro"

---

> **Nota:** Este projeto foi adaptado para abordar desafios reais de DevOps em órgãos governamentais brasileiros, com foco em economia de recursos e conformidade legal.