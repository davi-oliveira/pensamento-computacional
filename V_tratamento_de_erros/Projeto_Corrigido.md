# Projeto Corrigido — Tradutor de Código Morse

**Disciplina:** Pensamento Computacional  
**Projeto:** `I_codigo_morse`  
**Data:** 14/05/2026  

---

## Código Corrigido

Abaixo está a versão corrigida do tradutor de Código Morse em Python, com todas as correções aplicadas e comentadas.

```python
# ============================================================
# Tradutor de Código Morse — Versão Corrigida
# Disciplina: Pensamento Computacional
# Grupo: Davi, Leonardo, Yuri, Ricardo
# ============================================================

# Dicionário texto → Morse
TABELA_MORSE = {
    'A': '.-',    'B': '-...',  'C': '-.-.',  'D': '-..',
    'E': '.',     'F': '..-.',  'G': '--.',   'H': '....',
    'I': '..',    'J': '.---',  'K': '-.-',   'L': '.-..',
    'M': '--',    'N': '-.',    'O': '---',   'P': '.--.',
    'Q': '--.-',  'R': '.-.',   'S': '...',   'T': '-',
    'U': '..-',   'V': '...-',  'W': '.--',   'X': '-..-',
    'Y': '-.--',  'Z': '--..',
    '0': '-----', '1': '.----', '2': '..---', '3': '...--',
    '4': '....-', '5': '.....', '6': '-....', '7': '--...',
    '8': '---..', '9': '----.',
    '.': '.-.-.-', ',': '--..--', '?': '..--..', ' ': '/'
}

# Dicionário inverso: Morse → texto (gerado automaticamente)
TABELA_INVERSA = {v: k for k, v in TABELA_MORSE.items()}


def texto_para_morse(texto: str) -> str:
    """
    Converte uma string de texto para Código Morse.
    Palavras são separadas por ' / '.
    Caracteres não mapeados são ignorados com aviso.
    """
    # CORREÇÃO 4: resultado inicializado antes de qualquer condicional
    resultado = []

    # CORREÇÃO 5: validação de entrada vazia
    if not texto.strip():
        return "⚠️  Entrada vazia. Por favor, digite um texto."

    for char in texto.upper():
        if char in TABELA_MORSE:
            resultado.append(TABELA_MORSE[char])
        elif char == ' ':
            # CORREÇÃO 1: espaço entre palavras vira separador '/'
            resultado.append('/')
        else:
            # CORREÇÃO 2: caractere não mapeado é ignorado com aviso, sem crash
            print(f"⚠️  Caractere '{char}' não reconhecido e será ignorado.")

    return ' '.join(resultado)


def morse_para_texto(morse: str) -> str:
    """
    Converte uma string de Código Morse para texto.
    O separador '/' indica espaço entre palavras.
    """
    # CORREÇÃO 4: resultado inicializado antes de qualquer condicional
    resultado = []

    # CORREÇÃO 5: validação de entrada vazia
    if not morse.strip():
        return "⚠️  Entrada vazia. Por favor, digite um código Morse."

    # CORREÇÃO 3: separar por ' / ' para identificar palavras corretamente
    palavras = morse.strip().split(' / ')

    for palavra in palavras:
        letras = palavra.split()
        palavra_texto = ''
        for codigo in letras:
            if codigo in TABELA_INVERSA:
                palavra_texto += TABELA_INVERSA[codigo]
            else:
                # CORREÇÃO 2: código não mapeado não causa crash
                print(f"⚠️  Código Morse '{codigo}' não reconhecido e será ignorado.")
        resultado.append(palavra_texto)

    return ' '.join(resultado)


def main():
    print("=" * 40)
    print("   Tradutor de Código Morse")
    print("=" * 40)

    while True:
        print("\nEscolha uma opção:")
        print("1 — Texto para Morse")
        print("2 — Morse para Texto")
        print("0 — Sair")

        opcao = input("\nOpção: ").strip()

        if opcao == '1':
            texto = input("Digite o texto: ")
            print("\nResultado:", texto_para_morse(texto))

        elif opcao == '2':
            morse = input("Digite o código Morse (use '/' para separar palavras): ")
            print("\nResultado:", morse_para_texto(morse))

        elif opcao == '0':
            print("Encerrando o programa. Até logo!")
            break

        else:
            print("⚠️  Opção inválida. Tente novamente.")


if __name__ == "__main__":
    main()
```

---

## Justificativas das Correções

### Correção 1 — Separador de palavras no Morse

**Problema:** Palavras eram concatenadas sem separador, tornando o Morse ilegível e impossível de traduzir de volta.  
**Solução:** O caractere de espaço `' '` foi mapeado no dicionário como `'/'`, seguindo o padrão internacional do Código Morse onde `/` separa palavras.  
**Justificativa técnica:** O padrão ITU-R M.1677 define que palavras em Morse são separadas por 7 unidades de silêncio; na representação textual, usa-se `/`.

---

### Correção 2 — Tratamento de caracteres não mapeados

**Problema:** Um `KeyError` encerrava o programa ao encontrar caracteres especiais.  
**Solução:** Substituído o acesso direto ao dicionário (`tabela[char]`) por uma verificação com `if char in TABELA_MORSE`, exibindo um aviso e continuando a execução.  
**Justificativa técnica:** Aplicação do princípio de **tratamento de exceções defensivo** — o programa deve lidar graciosamente com entradas inesperadas sem encerrar abruptamente.

---

### Correção 3 — Conversão Morse → texto com separador de palavras

**Problema:** O `/` era tratado como código Morse inválido ao fazer a conversão inversa.  
**Solução:** A string Morse é primeiro dividida por `' / '` para identificar palavras, e depois cada palavra tem seus caracteres separados por espaço simples.  
**Justificativa técnica:** Separar o problema em etapas (split por palavras, depois por caracteres) aplica o pilar de **decomposição** do Pensamento Computacional.

---

### Correção 4 — Inicialização da variável `resultado`

**Problema:** `UnboundLocalError` quando o fluxo não passava pelo bloco que inicializava a variável.  
**Solução:** A variável `resultado` foi inicializada como lista vazia `[]` antes de qualquer estrutura condicional.  
**Justificativa técnica:** Boa prática de programação defensiva — variáveis devem ter um estado inicial definido antes de serem utilizadas.

---

### Correção 5 — Validação de entrada vazia

**Problema:** Entrada vazia gerava saída em branco sem feedback ao usuário.  
**Solução:** Adicionada verificação `if not texto.strip()` no início de cada função, retornando uma mensagem informativa.  
**Justificativa técnica:** Validação de entrada é um dos primeiros passos de robustez em qualquer sistema interativo, melhorando a **usabilidade e experiência do usuário**.
