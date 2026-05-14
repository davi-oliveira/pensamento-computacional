# Erros Identificados — Projeto Pensamento Computacional

**Disciplina:** Pensamento Computacional  
**Projeto analisado:** `I_codigo_morse` — Tradutor de Código Morse em Python  
**Data da análise:** 14/05/2026  

---

## Metodologia

A análise foi conduzida revisando o código do projeto em três dimensões:

- **Erros de Sintaxe** — problemas que impedem a execução do programa.
- **Erros de Lógica** — o código executa, mas produz resultados incorretos.
- **Erros de Execução (Runtime)** — o programa quebra em situações específicas durante o uso.

---

## Erros Encontrados

### 1. Erro de Lógica — Separação de palavras no Morse

**Localização:** Função responsável por converter texto para Morse  
**Descrição:** Ao converter uma frase com múltiplas palavras, os caracteres de cada palavra eram separados por espaço, mas não havia separador entre palavras distintas. Em Morse, o separador padrão entre palavras é barra (`/`) ou espaço duplo.

**Exemplo do comportamento errado:**
```
Entrada: "SOS ME"
Saída:   "... --- ... -- ."
```

**Comportamento esperado:**
```
Saída:   "... --- ... / -- ."
```

---

### 2. Erro de Execução — Caractere não mapeado gera exceção

**Localização:** Função de tradução texto → Morse  
**Descrição:** Quando o usuário digitava um caractere especial (como `!`, `@`, `ç`, `ã`) que não está no dicionário Morse, o programa lançava um `KeyError` e encerrava abruptamente, sem mensagem amigável.

**Trecho com o problema:**
```python
for char in texto.upper():
    codigo += tabela_morse[char] + " "  # KeyError se char não existe no dicionário
```

---

### 3. Erro de Lógica — Conversão de Morse para texto ignora separador de palavras

**Localização:** Função de tradução Morse → texto  
**Descrição:** A função que converte Morse de volta para texto usava apenas o espaço simples como separador, sem tratar o `/` como separador de palavras. Isso fazia com que o `/` fosse tratado como um código Morse inválido.

**Exemplo:**
```
Entrada Morse: "... --- ... / -- ."
Saída errada:  "SOS[ERRO]/ME"
```

---

### 4. Erro de Sintaxe — Variável não inicializada

**Localização:** Início da função principal de tradução  
**Descrição:** A variável `resultado` era usada dentro de um bloco condicional sem ser inicializada antes, o que causava `UnboundLocalError` quando o fluxo não passava pelo bloco de inicialização.

**Trecho com o problema:**
```python
def traduzir(texto, modo):
    if modo == "texto_para_morse":
        resultado = ""
        # ... lógica ...
    return resultado  # NameError/UnboundLocalError se modo for inválido
```

---

### 5. Erro de Lógica — Falta de tratamento para entrada vazia

**Localização:** Entrada do usuário  
**Descrição:** O programa não validava se a entrada do usuário estava vazia. Ao pressionar Enter sem digitar nada, o programa executava a lógica com string vazia e retornava uma saída em branco sem nenhuma mensagem informativa.

---

## Resumo dos Erros

| # | Tipo | Descrição resumida | Impacto |
|---|---|---|---|
| 1 | Lógica | Falta de separador `/` entre palavras no Morse | Tradução incorreta |
| 2 | Execução | `KeyError` para caracteres não mapeados | Crash do programa |
| 3 | Lógica | Separador `/` não tratado na conversão Morse→texto | Tradução incorreta |
| 4 | Sintaxe | Variável `resultado` não inicializada | `UnboundLocalError` |
| 5 | Lógica | Sem validação de entrada vazia | UX ruim, saída em branco |
