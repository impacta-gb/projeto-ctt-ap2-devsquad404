# Estruturas de controle em Go (if, for, switch)

As estruturas de controle definem o fluxo de execução do seu programa: o que executar, em que ordem e quantas vezes.  
Em Go, existem poucas estruturas de controle, mas elas são poderosas e costumam ser suficientes para a maioria dos cenários.

Nesta página, você vai aprender:

- o uso de `if` e blocos `else`;
- como usar o laço `for` (incluindo `for range`);
- como trabalhar com `switch` para múltiplas opções.

---

## if e else: tomada de decisão simples

A estrutura `if` é usada para executar um bloco de código **somente se uma condição for verdadeira**. Em Go, o `if` sempre exige chaves `{}` ao redor do bloco, mesmo que o bloco tenha só uma linha.

### Exemplo básico de if

```go
package main

import "fmt"

func main() {
    idade := 18

    if idade >= 18 {
        fmt.Println("Você é maior de idade.")
    }
}
```

Explicando:

- `idade >= 18` é a condição;
- o bloco entre chaves só é executado se essa condição for `true`.

### if + else

Para executar um código quando a condição é falsa, usa-se `else`:

```go
if idade >= 18 {
    fmt.Println("Você é maior de idade.")
} else {
    fmt.Println("Você é menor de idade.")
}
```

Note que o `else` também precisa de chaves em Go.

### if, else if e vários casos

Você pode encadear múltiplas condições com `else if`:

```go
nota := 75

if nota >= 90 {
    fmt.Println("Aprovado com nota A")
} else if nota >= 80 {
    fmt.Println("Aprovado com nota B")
} else if nota >= 70 {
    fmt.Println("Aprovado com nota C")
} else {
    fmt.Println("Reprovado")
}
```

Dessa forma, o programa testa condições em ordem e executa o bloco da primeira condição que for verdadeira.

---

## for: o único laço em Go

Em Go, **o único** laço de repetição é `for`. Ele é flexível o suficiente para se comportar como `while` e `for` de outras linguagens.

### for tradicional (com inicialização, condição e pós‑incremento)

Sintaxe:

```go
for inicializacao; condicao; incremento {
    // código a repetir
}
```

Exemplo:

```go
for i := 0; i < 5; i++ {
    fmt.Println("Contagem:", i)
}
```

Esse código imprime os números de 0 a 4.

### for como while

Em Go, você também pode usar `for` sem a parte de inicialização e incremento, deixando apenas a condição:

```go
contador := 0

for contador < 3 {
    fmt.Println("Repetindo:", contador)
    contador++
}
```

Nesse caso, o `for` funciona como um `while` de outras linguagens.

### for com range

O `for range` é muito usado para percorrer **slices**, **arrays** e **mapas**:

```go
numeros := []int{10, 20, 30}

for indice, valor := range numeros {
    fmt.Printf("índice: %d, valor: %d\n", indice, valor)
}
```

- `range` percorre a coleção e devolve o índice e o valor de cada elemento.
- `indice` e `valor` são variáveis locais criadas a cada iteração.

Essa estrutura é muito comum na idiomática de Go, e você verá muito código assim em projetos reais.

---

## switch: múltiplas opções de forma clara

O `switch` é usado para executar blocos de código distintos com base no valor de uma expressão.  
Em Go, o `switch` não “cai” automaticamente para o próximo caso; para isso, é preciso usar `fallthrough`, o que torna o comportamento mais explícito.

### Exemplo de switch com um valor

```go
dia := "segunda"

switch dia {
case "segunda":
    fmt.Println("Início da semana, vamos lá!")
case "sexta":
    fmt.Println("Ufa, fim de semana chegando!")
default:
    fmt.Println("Mais um dia de trabalho.")
}
```

- Cada `case` define um valor que a variável `dia` pode ter;
- `default` é opcional, mas recomendado, para cobrir valores inesperados.

### Switch com múltiplos valores em um case

Você também pode agrupar valores em um mesmo `case`:

```go
nota := "B+"

switch nota {
case "A+", "A", "A-":
    fmt.Println("Excelente")
case "B+", "B", "B-":
    fmt.Println("Bom")
case "C", "D", "F":
    fmt.Println("Precisa melhorar")
default:
    fmt.Println("Nota desconhecida")
}
```

Isso torna o código mais limpo do que vários `if` encadeados.

### switch sem expressão (switch com condições)

Go permite um tipo especial de `switch` que avalia condições booleanas em vez de um valor:

```go
nota := 75

switch {
case nota >= 90:
    fmt.Println("A")
case nota >= 80:
    fmt.Println("B")
case nota >= 70:
    fmt.Println("C")
default:
    fmt.Println("Reprovado")
}
```

Nesse caso, os `case`s são testes de condição, e a primeira condição verdadeira define o bloco executado.

---

## break e continue

Dentro de loops, `for` e `for range`, você pode usar:

- `break`: interrompe completamente o laço;
- `continue`: pula o restante do bloco atual e vai para a próxima iteração.

Exemplo com `break`:

```go
for i := 1; i <= 10; i++ {
    if i == 5 {
        fmt.Println("Chegamos no 5, vamos parar.")
        break
    }
    fmt.Println("Contando:", i)
}
```

Exemplo com `continue`:

```go
for i := 1; i <= 5; i++ {
    if i == 3 {
        continue
    }
    fmt.Println("Contando:", i)
}
```

Saída:

```text
Contando: 1
Contando: 2
Contando: 4
Contando: 5
```

O número 3 é pulado por causa do `continue`.

---

## Como escolher qual estrutura usar?

Regras práticas para iniciantes:

- Use `if` e `else` quando tiver uma ou poucas decisões simples (`if x > 0 { ... }`).
- Use `switch` quando tiver vários valores distintos de uma mesma variável (`if status == "ativo" || status == "inativo"` vira `switch status { ... }`).
- Use `for` quando precisar repetir algo um número conhecido de vezes ou percorrer uma coleção.
- Use `for range` sempre que quiser percorrer slices, arrays ou maps de forma idiomática.

---

## Conclusão e próximos passos

Nesta página você viu:

- como usar `if`, `else` e `else if` para tomar decisões;
- como trabalhar com `for` em seus diferentes estilos, incluindo `for range`;
- como simplificar múltiplas comparações com `switch`;
- e como interromper ou pular partes do laço com `break` e `continue`.

Com isso, você já domina o fluxo básico de um programa em Go.  
Na próxima página, você vai aprender sobre **arrays, slices e maps**, que são formas de armazenar e manipular múltiplos valores de uma vez. Isso fecha a parte de fundamentos que você é responsável por documentar.