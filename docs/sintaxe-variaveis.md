# Sintaxe básica e variáveis em Go

Depois de instalar o Go e rodar seu primeiro programa, o próximo passo é entender a sintaxe básica da linguagem e como declarar variáveis.  
Esta página apresenta a estrutura geral de um programa Go, a forma de declarar variáveis e constantes, e os tipos de dados mais comuns.

---

## Estrutura geral de um programa em Go

Um programa simples em Go normalmente segue esta estrutura:

```go
package main

import "fmt"

func main() {
    fmt.Println("Olá, Go!")
}
```

Explicando cada parte:

- `package main`: indica que este arquivo faz parte do pacote principal do programa. Pacotes `main` geram executáveis.
- `import "fmt"`: importa o pacote `fmt`, que fornece funções para formatar e imprimir valores, como `Println`.
- `func main() { ... }`: define a função principal. Todo programa executável em Go começa pela função `main`.

---

## Declaração de variáveis

Em Go, variáveis são usadas para guardar valores na memória, como números, textos ou valores lógicos.  
Existem duas formas muito usadas para declarar variáveis: usando `var` com tipo explícito e usando a sintaxe curta `:=` com inferência de tipo.

### Usando `var` com tipo explícito

A forma mais tradicional é:

```go
var idade int
var nome string
```

Aqui:

- `var` indica que você está declarando uma variável.
- `idade` e `nome` são os nomes das variáveis.
- `int` e `string` são os tipos.

Você pode também inicializar a variável já com um valor:

```go
var idade int = 25
var nome string = "Ana"
```

### Usando inferência de tipo com `:=`

Dentro de funções, Go permite uma forma mais curta de declarar variáveis, deixando que o compilador descubra o tipo automaticamente:

```go
idade := 25
nome := "Ana"
altura := 1.68
ativo := true
```

- O operador `:=` declara e atribui ao mesmo tempo.
- O tipo é inferido a partir do valor: `25` é `int`, `"Ana"` é `string`, `1.68` é `float64`, `true` é `bool`.

Essa sintaxe é muito comum em código idiomático Go porque torna o código mais enxuto.

---

## Valores padrão (zero values)

Quando você declara uma variável com `var` e não atribui um valor inicial, Go não deixa a variável “sem valor”.  
Em vez disso, cada tipo tem um **valor padrão** (chamado de *zero value*).

Exemplo:

```go
var numero int
var texto string
var ativo bool
```

Os valores iniciais serão:

- `numero` → `0`
- `texto` → `""` (string vazia)
- `ativo` → `false`

Isso evita problemas com “lixo de memória” e torna o comportamento mais previsível.

---

## Tipos básicos em Go

Go é uma linguagem com tipos estáticos, ou seja, cada variável tem um tipo bem definido em tempo de compilação.  
Alguns tipos básicos mais usados são:

### Números inteiros

Tipos para números sem parte decimal, como:

- `int` (tamanho depende da arquitetura, comum em exemplos)
- `int8`, `int16`, `int32`, `int64`

Exemplo:

```go
var contador int = 10
```

### Números de ponto flutuante

Para valores com casas decimais:

- `float32`
- `float64` (mais comum)

```go
var temperatura float64 = 26.5
```

### Strings

Representam textos (sequência de caracteres):

```go
var mensagem string = "Estudando Go"
```

Strings em Go são imutáveis: ao modificar uma string, o Go cria um novo valor internamente.

### Booleanos

Guardam apenas `true` ou `false`:

```go
var logado bool = true
```

Valores booleanos são muito usados em estruturas de controle, como `if` e `for`.

---

## Constantes

Constantes são valores que não mudam durante a execução do programa. Elas são declaradas com a palavra-chave `const`.

```go
const pi float64 = 3.14159
const mensagem = "Bem-vindo ao sistema"
```

Características importantes:

- Uma constante precisa ser conhecida em tempo de compilação (não pode depender de cálculo em tempo de execução).
- Usar constantes ajuda a tornar o código mais legível e evita “números mágicos” espalhados pelo código.

Você também pode agrupar constantes:

```go
const (
    DiaSemana = 7
    MesAno    = 12
)
```

---

## Boas práticas de nomes em Go

Algumas convenções comuns de nomes em Go:

- Use **camelCase** para variáveis e funções locais: `idadeUsuario`, `totalPedidos`.
- Use nomes curtos e claros. Go costuma preferir nomes mais simples (como `n`, `err`) quando o contexto já é óbvio.
- A primeira letra maiúscula ou minúscula importa:
  - Nomes que começam com maiúscula são exportados (visíveis em outros pacotes).
  - Nomes que começam com minúscula são privados ao pacote atual.

Exemplos:

```go
var idadeUsuario int    // visível só no pacote
var IdadeUsuario int    // seria exportada, se estivesse em um pacote
```

---

## Conclusão e próximos passos

Nesta página você viu:

- como é a estrutura básica de um programa em Go;
- como declarar variáveis com `var` e com `:=`;
- o que são zero values e alguns tipos básicos da linguagem;
- como e por que usar constantes.

A partir daqui, o próximo passo natural é aprender como controlar o fluxo do seu programa com **estruturas de controle** (`if`, `for`, `switch`), que será o tema da próxima página desta documentação.