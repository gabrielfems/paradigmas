# Atividade 3: Derivação de um Código a partir da Gramática Formal

## Regras da Atividade (Enunciado)
**Objetivo:** Pesquisar e analisar a gramática formal de uma linguagem de programação, identificando suas principais regras de produção e utilizando-as para realizar a derivação de um trecho de código válido.

A atividade tem como objetivo relacionar os conceitos de gramáticas formais, símbolos terminais e não terminais, produções e derivação com linguagens de programação reais.

**Passos exigidos:**
1. Escolher uma linguagem de programação.
2. Pesquisar a gramática formal da linguagem.
3. Identificar a fonte da gramática.
4. Selecionar as produções necessárias.
5. Definir o código a ser gerado.
6. Realizar a derivação passo a passo.
7. Apresentar o resultado e classificar os símbolos.

---

## Resolução da Atividade

### 1. Identificação da Fonte da Gramática
- **Linguagem escolhida:** JavaScript
- **Fonte / Documentação consultada:** [ECMAScript Language Specification](https://tc39.es/ecma262/)
- **Notação utilizada:** Gramática sintática do ECMA-262, com convenções como:
  - `|` para alternativas
  - `[ ... ]opt` para elementos opcionais
  - Aspas duplas (`"..."`) para literais/terminais

### 2. Definição do Código a ser Gerado
Trecho válido em JavaScript (uma declaração `let` com atribuição):
```javascript
let soma = 1 + 2;
```

### 3. Seleção das Produções (Regras Extraídas da Spec)
Regras formais extraídas da especificação do ECMA-262 necessárias para representar essa construção:
```ebnf
LexicalDeclaration  = LetOrConst BindingList ";" .
LetOrConst          = "let" | "const" .
BindingList         = LexicalBinding { "," LexicalBinding } .
LexicalBinding      = BindingIdentifier Initializer .
BindingIdentifier   = Identifier .
Initializer         = "=" AssignmentExpression .
AssignmentExpression = AdditiveExpression .
AdditiveExpression  = AdditiveExpression "+" MultiplicativeExpression
                    | MultiplicativeExpression .
MultiplicativeExpression = PrimaryExpression .
PrimaryExpression   = Literal .
Literal             = NumericLiteral .
```

**Significado das Regras:**
- **`LexicalDeclaration`**: Produção que representa declarações com `let`/`const`, diferente de `VariableStatement` (que é exclusiva do `var`).
- **`LexicalBinding`**: Associa um identificador (`BindingIdentifier`) a um valor inicial via `Initializer`.
- **`AdditiveExpression` / `MultiplicativeExpression`**: Regras recursivas que representam a precedência de operadores aritméticos — soma/subtração dependem de produtos/divisões, que por sua vez dependem de expressões primárias.
- **`PrimaryExpression` / `Literal`**: Representam as unidades básicas de valor, como literais numéricos.

### 4. Realização da Derivação
Iniciando no símbolo `LexicalDeclaration` e aplicando sucessivamente as produções mais à esquerda (*leftmost derivation*):

```text
LexicalDeclaration
=> LetOrConst BindingList ";"
=> "let" BindingList ";"
=> "let" LexicalBinding ";"
=> "let" BindingIdentifier Initializer ";"
=> "let" Identifier Initializer ";"
=> "let" "soma" Initializer ";"
=> "let" "soma" "=" AssignmentExpression ";"
=> "let" "soma" "=" AdditiveExpression ";"
=> "let" "soma" "=" AdditiveExpression "+" MultiplicativeExpression ";"
=> "let" "soma" "=" MultiplicativeExpression "+" MultiplicativeExpression ";"
=> "let" "soma" "=" PrimaryExpression "+" MultiplicativeExpression ";"
=> "let" "soma" "=" Literal "+" MultiplicativeExpression ";"
=> "let" "soma" "=" NumericLiteral "+" MultiplicativeExpression ";"
=> "let" "soma" "=" "1" "+" MultiplicativeExpression ";"
=> "let" "soma" "=" "1" "+" PrimaryExpression ";"
=> "let" "soma" "=" "1" "+" Literal ";"
=> "let" "soma" "=" "1" "+" NumericLiteral ";"
=> "let" "soma" "=" "1" "+" "2" ";"
```

### 5. Apresentação do Resultado e Análise de Símbolos

**Código Final Gerado:**
```javascript
let soma = 1 + 2;
```

**Classificação dos Símbolos:**
- **Símbolos Não Terminais:** `LexicalDeclaration`, `LetOrConst`, `BindingList`, `LexicalBinding`, `BindingIdentifier`, `Initializer`, `AssignmentExpression`, `AdditiveExpression`, `MultiplicativeExpression`, `PrimaryExpression`, `Literal`.
- **Símbolos Terminais (Tokens/Literais):** `let`, `=`, `+`, `;`, `soma` (token de identificador resolvido pelo analisador léxico), `1`, `2` (tokens numéricos - `NumericLiteral`).

**Explicação do Processo:**
A derivação parte da categoria sintática de declaração léxica (`LexicalDeclaration`), específica para `let`/`const` no ECMA-262 — diferente do `var`, que é tratado por `VariableStatement`. A gramática exige a palavra-chave `let`, seguida de um identificador (`BindingIdentifier`) e, opcionalmente, um inicializador (`Initializer`) precedido pelo terminal `=`. O lado direito da atribuição expande-se recursivamente através de `AdditiveExpression`, que representa a precedência de operadores: soma depende de multiplicação, que depende de uma expressão primária. Ao final, todos os elementos não terminais são inteiramente substituídos por terminais válidos da linguagem JavaScript.
