# Structs e Métodos em Go

## Introdução

Em Go, as **structs** são utilizadas para agrupar diferentes tipos de dados em uma única estrutura. Elas funcionam de forma parecida com classes em outras linguagens, porém Go não possui orientação a objetos tradicional.

Já os **métodos** permitem associar funções diretamente a uma struct, tornando o código mais organizado, reutilizável e fácil de manter.

---

## O que são Structs?

Uma `struct` é um tipo personalizado que permite armazenar múltiplos campos.

### Exemplo básico

```go
package main

import "fmt"

type Usuario struct {
    Nome  string
    Idade int
}

func main() {
    usuario := Usuario{
        Nome:  "Bruno",
        Idade: 21,
    }

    fmt.Println(usuario.Nome)
    fmt.Println(usuario.Idade)
}
```

### Saída esperada

```bash
Bruno
21
```

---

## Criando Structs

A criação de uma struct utiliza a palavra-chave `type`.

### Sintaxe

```go
type NomeDaStruct struct {
    Campo tipo
}
```

### Exemplo

```go
type Produto struct {
    Nome  string
    Preco float64
}
```

---

## Inicializando Structs

Existem diferentes maneiras de criar uma struct.

### Inicialização completa

```go
produto := Produto{
    Nome:  "Notebook",
    Preco: 3500.00,
}
```

### Inicialização parcial

```go
produto := Produto{
    Nome: "Mouse",
}
```

Campos não definidos recebem valores padrão.

---

## Valores padrão (Zero Values)

Cada tipo possui um valor padrão quando não inicializado.

| Tipo   | Valor padrão |
| ------ | ------------ |
| string | `""`         |
| int    | `0`          |
| bool   | `false`      |
| float  | `0.0`        |

### Exemplo

```go
type Pessoa struct {
    Nome string
    Idade int
}

func main() {
    p := Pessoa{}

    fmt.Println(p.Nome)
    fmt.Println(p.Idade)
}
```

---

## Structs Aninhadas

Uma struct pode conter outra struct.

### Exemplo

```go
type Endereco struct {
    Cidade string
    Estado string
}

type Usuario struct {
    Nome string
    Endereco Endereco
}
```

### Utilização

```go
usuario := Usuario{
    Nome: "Carlos",
    Endereco: Endereco{
        Cidade: "São Paulo",
        Estado: "SP",
    },
}
```

---

## O que são Métodos?

Métodos são funções associadas a uma struct.

Eles permitem adicionar comportamentos ao objeto.

### Sintaxe

```go
func (variavel Struct) Metodo() {
    // código
}
```

---

## Criando Métodos

### Exemplo simples

```go
package main

import "fmt"

type Pessoa struct {
    Nome string
}

func (p Pessoa) Apresentar() {
    fmt.Println("Olá, meu nome é", p.Nome)
}

func main() {
    pessoa := Pessoa{
        Nome: "Bruno",
    }

    pessoa.Apresentar()
}
```

### Saída

```bash
Olá, meu nome é Bruno
```

---

## Receiver

O valor entre parênteses antes do nome do método é chamado de **receiver**.

```go
func (p Pessoa) Apresentar()
```

Nesse caso:

* `p` → variável de acesso
* `Pessoa` → struct associada

---

## Métodos com Retorno

Métodos também podem retornar valores.

### Exemplo

```go
package main

import "fmt"

type Produto struct {
    Nome  string
    Preco float64
}

func (p Produto) Desconto() float64 {
    return p.Preco * 0.9
}

func main() {
    produto := Produto{
        Nome:  "Teclado",
        Preco: 200,
    }

    fmt.Println(produto.Desconto())
}
```

---

## Pointer Receiver

Quando usamos ponteiros em métodos, conseguimos alterar os valores originais da struct.

### Exemplo

```go
package main

import "fmt"

type Conta struct {
    Saldo float64
}

func (c *Conta) Depositar(valor float64) {
    c.Saldo += valor
}

func main() {
    conta := Conta{
        Saldo: 100,
    }

    conta.Depositar(50)

    fmt.Println(conta.Saldo)
}
```

### Saída

```bash
150
```

---

## Value Receiver vs Pointer Receiver

| Tipo             | Característica               |
| ---------------- | ---------------------------- |
| Value Receiver   | Trabalha com cópia dos dados |
| Pointer Receiver | Altera os dados originais    |

### Recomendação

Use `Pointer Receiver` quando:

* Precisar modificar a struct
* Trabalhar com structs grandes
* Evitar cópias desnecessárias

---

## Exemplo Prático Completo

```go
package main

import "fmt"

type Carro struct {
    Marca string
    Modelo string
    Velocidade int
}

func (c *Carro) Acelerar() {
    c.Velocidade += 10
}

func (c Carro) ExibirInfo() {
    fmt.Println("Marca:", c.Marca)
    fmt.Println("Modelo:", c.Modelo)
    fmt.Println("Velocidade:", c.Velocidade)
}

func main() {
    carro := Carro{
        Marca: "Toyota",
        Modelo: "Corolla",
        Velocidade: 0,
    }

    carro.Acelerar()
    carro.Acelerar()

    carro.ExibirInfo()
}
```

### Saída

```bash
Marca: Toyota
Modelo: Corolla
Velocidade: 20
```

---

## Boas Práticas

### Utilize nomes claros

```go
type Usuario struct {}
```

Evite:

```go
type U struct {}
```

---

### Separe responsabilidades

Cada struct deve representar apenas uma entidade.

---

### Prefira métodos pequenos

Métodos menores facilitam manutenção e testes.

---

## Alertas e Observações

>  Structs em Go não possuem herança tradicional.

>  Métodos com `Pointer Receiver` podem alterar os dados originais.

>  Go favorece composição ao invés de herança.

---

## Conclusão

Structs e Métodos são fundamentais para o desenvolvimento em Go, permitindo organizar dados e comportamentos de maneira simples e eficiente.

Com eles é possível:

* Modelar entidades
* Organizar responsabilidades
* Criar códigos reutilizáveis
* Melhorar legibilidade
* Facilitar manutenção do projeto

Esses conceitos são amplamente utilizados em APIs, sistemas web, microsserviços e aplicações escaláveis desenvolvidas em Go.
