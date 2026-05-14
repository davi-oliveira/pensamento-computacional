# Avaliação da Solução Final

**Disciplina:** Pensamento Computacional  
**Projeto:** `I_codigo_morse` — Tradutor de Código Morse  
**Data:** 14/05/2026  

---

## 1. Clareza do Código

### Pontos positivos

- O uso de **dicionários** (`TABELA_MORSE` e `TABELA_INVERSA`) torna a estrutura de dados legível e fácil de atualizar — qualquer membro do grupo consegue identificar rapidamente os mapeamentos de caracteres.
- As funções foram divididas com responsabilidade única: `texto_para_morse()` e `morse_para_texto()` fazem exatamente o que o nome indica, sem efeitos colaterais.
- O dicionário inverso é gerado automaticamente com `{v: k for k, v in TABELA_MORSE.items()}`, evitando duplicação de dados e possíveis inconsistências.
- Os comentários nas correções (marcados com `# CORREÇÃO N`) facilitam a rastreabilidade das mudanças realizadas.

### Pontos a melhorar

- O menu interativo no `main()` poderia ser extraído para uma função separada, deixando o `main()` mais enxuto.
- As mensagens de aviso (`⚠️`) poderiam utilizar um sistema de logging em vez de `print()` simples, o que facilitaria desativar logs em produção.

**Nota de clareza: 8/10**

---

## 2. Eficiência

### Análise de complexidade

| Operação | Complexidade | Observação |
|---|---|---|
| Texto → Morse | O(n) | Percorre cada caractere uma única vez |
| Morse → Texto | O(n) | Percorre cada código uma única vez |
| Busca no dicionário | O(1) | Python usa hash tables internamente |

- O algoritmo é **linear** em relação ao tamanho da entrada, o que é ótimo para esse tipo de problema.
- A geração do dicionário inverso é feita **uma única vez** na inicialização do módulo, não repetindo o custo a cada chamada de função.
- O uso de `lista.append()` seguido de `' '.join()` é mais eficiente que concatenação de strings (`+=`) em Python, pois evita a criação de múltiplos objetos intermediários na memória.

### Comparação: antes vs. depois das correções

| Aspecto | Antes | Depois |
|---|---|---|
| Tratamento de erros | Crash com `KeyError` | Aviso e continuidade |
| Memória | Concatenação de strings | Lista + join (mais eficiente) |
| Separação de palavras | Incorreta | Correta com `/` |

**Nota de eficiência: 8.5/10**

---

## 3. Escalabilidade

### O que funciona bem em escala

- O uso de **dicionário** para mapeamento permite adicionar novos caracteres (como letras acentuadas ou símbolos adicionais) com uma única linha de código, sem alterar nenhuma lógica.
- As funções são **puras** (sem estado global modificado), o que facilita seu uso em ambientes concorrentes ou em testes automatizados.

### Limitações atuais

- O projeto é um **script de linha de comando** (CLI). Para escalar a uma aplicação web ou API, seria necessário encapsular as funções em um serviço (ex.: Flask ou FastAPI), o que a estrutura modular atual já facilita.
- Não há persistência de dados — o histórico de traduções não é salvo. Para uma versão escalável, seria interessante integrar um banco de dados simples (SQLite) ou exportar para arquivo.
- Não há suporte a diferentes variantes do Código Morse (ex.: Morse japonês Wabun, Morse russo). Uma arquitetura mais escalável usaria a tabela como parâmetro configurável da função.

### Proposta de evolução

```
Versão atual:   Script Python CLI
Versão 2.0:     API REST (FastAPI) com endpoint /traduzir
Versão 3.0:     Interface Web + suporte a áudio (pontos e traços sonoros)
Versão 4.0:     Suporte a múltiplos alfabetos Morse (internacionalização)
```

**Nota de escalabilidade: 7/10**

---

## 4. Reflexão Final do Grupo

A atividade de tratamento de erros foi fundamental para reforçar que **código que funciona no caso ideal não é suficiente** — um software robusto precisa lidar com entradas inesperadas, estados inválidos e fluxos alternativos.

Os pilares do Pensamento Computacional estiveram presentes em todo o processo:

- **Decomposição:** dividir o problema de tradução em etapas menores (separar palavras, depois caracteres).
- **Reconhecimento de padrões:** identificar que todos os erros tinham em comum a ausência de validação de entrada.
- **Abstração:** usar dicionários para representar a tabela Morse sem se preocupar com detalhes de busca.
- **Algoritmos:** estruturar o fluxo de correção de forma sistemática e reproduzível.

A versão corrigida do projeto está mais robusta, legível e preparada para evoluções futuras.

---

> **Grupo:** Davi Rosa de Oliveira Souza, Leonardo de Castro Zeraik, Yuri Garcia Chacon, Ricardo Martins Ribeiro
