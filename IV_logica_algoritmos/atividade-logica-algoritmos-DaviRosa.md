# Algoritmo de Controle de Acesso - Sala de Aula

## Identificação
- **Aluno:** Davi Rosa de Oliveira Souza
- **Disciplina:** Lógica e Algoritmos
- **Atividade:** Controle de Acesso - A2

---

## Pseudocódigo: Controle de Entrada de Alunos

```
ALGORITMO ControleAcessoSala
DECLARE
    listaAlunos: VETOR[1..N] DE TEXTO
    filaAlunos: VETOR[1..M] DE TEXTO
    nomeAluno: TEXTO
    entradaPermitida: LOGICO
    i: INTEIRO
FIMDECLARE

// Inicialização da lista oficial de alunos matriculados
listaAlunos[1] ← "Ana Clara Silva"
listaAlunos[2] ← "Bruno Costa Santos"
listaAlunos[3] ← "Carlos Eduardo Oliveira"
listaAlunos[4] ← "Davi Rosa de Oliveira Souza"
listaAlunos[5] ← "Emily Ferreira Rodrigues"
listaAlunos[6] ← "Felipe Augusto Lima"
listaAlunos[7] ← "Gabriela Helena Souza"
listaAlunos[8] ← "Henrique Martins Pereira"
listaAlunos[9] ← "Isabella Cristina Alves"
listaAlunos[10] ← "João Pedro Rodrigues"

// Inicialização da fila de alunos para verificação
filaAlunos[1] ← "Davi Rosa de Oliveira Souza"
filaAlunos[2] ← "Maria Eduarda Santos"
filaAlunos[3] ← "Ana Clara Silva"
filaAlunos[4] ← "Pedro Henrique Oliveira"
filaAlunos[5] ← "Carlos Eduardo Oliveira"

// Início do algoritmo de controle de acesso
ESCREVER("==========================================")
ESCREVER("  SISTEMA DE CONTROLE DE ACESSO - UDF")
ESCREVER("==========================================")
ESCREVER("")

// Laço de repetição para verificar cada aluno da fila
PARA i DE 1 ATÉ M FAÇA
    nomeAluno ← filaAlunos[i]
    
    ESCREVER("Verificando aluno: ", nomeAluno)
    
    // Verificação: Checar se o aluno está presente na lista oficial
    entradaPermitida ← FALSO
    
    // Condicional: Busca do nome na lista oficial
    PARA j DE 1 ATÉ N FAÇA
        SE listaAlunos[j] = nomeAluno ENTÃO
            entradaPermitida ← VERDADEIRO
            INTERROMPER
        FIMSE
    FIMPARA
    
    // Condicional Positiva: Se o aluno estiver na lista, entrada permitida
    SE entradaPermitida = VERDADEIRO ENTÃO
        ESCREVER("  ✓ Status: ENTRADA PERMITIDA")
        ESCREVER("  → Aluno ", nomeAluno, " autorizado(a) a entrar na sala.")
    // Condicional Negativa: Caso o nome não conste na lista, entrada negada
    SENÃO
        ESCREVER("  ✗ Status: ENTRADA NEGADA")
        ESCREVER("  → ERRO: Aluno ", nomeAluno, " não encontrado na lista oficial.")
        ESCREVER("  → Favor procurar a coordenação para regularização.")
    FIMSE
    
    ESCREVER("")
FIMPARA

// Finalização do processo
ESCREVER("==========================================")
ESCREVER("  PROCESSAMENTO CONCLUÍDO")
ESCREVER("  Total de alunos verificados: ", M)
ESCREVER("==========================================")

FIMALGORITMO
```

---

## Estruturas Lógicas Utilizadas

| Estrutura | Descrição | Aplicação no Algoritmo |
|-----------|-----------|------------------------|
| **SE-ENTÃO-SENÃO** | Condicional para tomada de decisão | Verifica se o aluno está ou não na lista |
| **PARA...FAÇA** | Laço de repetição com contador | Processa todos os alunos da fila |
| **VETOR** | Estrutura de dados | Armazena lista oficial e fila de alunos |
| **INTERROMPER** | Controle de fluxo | Interrompe busca quando encontra o aluno |

---

## Explicação do Fluxo

1. **Inicialização**: Define a lista oficial de alunos matriculados e a fila de alunos aguardando verificação.

2. **Laço de Repetição (PARA)**: O processo se repete para cada aluno na fila até que todos sejam verificados.

3. **Verificação**: Para cada aluno, o algoritmo busca seu nome na lista oficial.

4. **Condicional Positiva (SE-ENTÃO)**: Se o nome for encontrado, exibe mensagem de entrada permitida.

5. **Condicional Negativa (SENÃO)**: Se o nome não for encontrado, exibe mensagem de erro e orienta o aluno a procurar a coordenação.

6. **Finalização**: Após processar todos os alunos, exibe resumo do processamento.

---

## Conclusão

Este algoritmo demonstra a aplicação prática de estruturas lógicas de controle (condicional e repetição) para resolver um problema cotidiano de controle de acesso, atendendo aos objetivos da disciplina de Pensamento Computacional.