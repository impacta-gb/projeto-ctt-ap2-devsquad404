# Testes Automatizados em Go

## Introdução

Os testes automatizados são fundamentais no desenvolvimento de software moderno. Em Go, o suporte a testes já vem integrado na própria linguagem através do pacote `testing`, permitindo validar funcionalidades, identificar erros rapidamente e garantir maior confiabilidade no código.

Com testes automatizados, é possível verificar se funções e comportamentos do sistema continuam funcionando corretamente mesmo após alterações no projeto.

---

## O que são Testes Automatizados?

Testes automatizados são códigos criados para validar automaticamente o funcionamento de outras partes do sistema.

Eles ajudam a:

* Garantir que o código funcione corretamente
* Detectar erros antes da entrega
* Facilitar manutenção do sistema
* Evitar bugs após alterações
* Melhorar a qualidade do software

Em Go, os testes geralmente ficam em arquivos com o sufixo:

```go
*_test.go
```

Exemplo:

```go
calculadora_test.go
```

---

# Estrutura Básica de um Teste

O Go utiliza o pacote `testing` para criação de testes.

Exemplo simples:

```go
package main

import "testing"

func Soma(a int, b int) int {
    return a + b
}

func TestSoma(t *testing.T) {
    resultado := Soma(2, 3)

    if resultado != 5 {
        t.Errorf("Esperado 5, mas recebeu %d", resultado)
    }
}
```

---

## Explicando o Código

### Função de teste

Toda função de teste deve:

* Começar com `Test`
* Receber `*testing.T`

Exemplo:

```go
func TestSoma(t *testing.T)
```

---

### Verificação de resultado

O teste compara o valor esperado com o valor recebido.

```go
if resultado != 5
```

Se houver diferença:

```go
t.Errorf()
```

é utilizado para informar erro.

---

# Executando Testes

Para executar todos os testes do projeto:

```bash
go test
```

Para mostrar detalhes:

```bash
go test -v
```

---

## Exemplo de saída

```bash
=== RUN   TestSoma
--- PASS: TestSoma (0.00s)
PASS
```

---

# Testando Múltiplos Casos

Uma boa prática é utilizar tabelas de testes.

Exemplo:

```go
package main

import "testing"

func Soma(a int, b int) int {
    return a + b
}

func TestSoma(t *testing.T) {

    testes := []struct {
        nome     string
        a        int
        b        int
        esperado int
    }{
        {"Soma simples", 2, 3, 5},
        {"Números negativos", -1, -1, -2},
        {"Com zero", 5, 0, 5},
    }

    for _, teste := range testes {

        resultado := Soma(teste.a, teste.b)

        if resultado != teste.esperado {
            t.Errorf(
                "%s: esperado %d, recebeu %d",
                teste.nome,
                teste.esperado,
                resultado,
            )
        }
    }
}
```

---

## Vantagens dos Testes em Tabela

* Código mais organizado
* Facilita adicionar novos cenários
* Evita repetição
* Torna os testes mais legíveis

---

# Testes de Erro

Também é importante validar situações de erro.

Exemplo:

```go
package main

import (
    "errors"
    "testing"
)

func Dividir(a float64, b float64) (float64, error) {

    if b == 0 {
        return 0, errors.New("divisão por zero")
    }

    return a / b, nil
}

func TestDividir(t *testing.T) {

    _, err := Dividir(10, 0)

    if err == nil {
        t.Errorf("Esperava um erro de divisão por zero")
    }
}
```

---

# Cobertura de Testes

A cobertura mostra quanto do código foi testado.

Executar:

```bash
go test -cover
```

Exemplo:

```bash
coverage: 85.7% of statements
```

Quanto maior a cobertura, maior a confiança no sistema.

---

# Benchmark em Go

O Go também permite medir desempenho de funções.

Exemplo:

```go
package main

import "testing"

func Soma(a int, b int) int {
    return a + b
}

func BenchmarkSoma(b *testing.B) {

    for i := 0; i < b.N; i++ {
        Soma(10, 20)
    }
}
```

Executar benchmark:

```bash
go test -bench=.
```

---

# Organização de Testes

Boa prática de estrutura:

```text
projeto/
│
├── main.go
├── calculadora.go
├── calculadora_test.go
```

---

# Boas Práticas

## 1. Criar testes pequenos

Cada teste deve validar apenas uma funcionalidade.

---

## 2. Utilizar nomes claros

Exemplo:

```go
TestUsuarioValido
```

---

## 3. Evitar dependência entre testes

Os testes devem funcionar individualmente.

---

## 4. Testar casos de erro

Não testar apenas cenários positivos.

---

## 5. Automatizar sempre

Executar testes frequentemente durante o desenvolvimento.

---

# Exemplo Prático Completo

## Arquivo principal

```go
package main

func Multiplicar(a int, b int) int {
    return a * b
}
```

---

## Arquivo de teste

```go
package main

import "testing"

func TestMultiplicar(t *testing.T) {

    resultado := Multiplicar(4, 5)

    esperado := 20

    if resultado != esperado {
        t.Errorf(
            "Esperado %d, recebeu %d",
            esperado,
            resultado,
        )
    }
}
```

---

# Comandos Importantes

| Comando            | Função               |
| ------------------ | -------------------- |
| `go test`          | Executa testes       |
| `go test -v`       | Executa com detalhes |
| `go test -cover`   | Mostra cobertura     |
| `go test -bench=.` | Executa benchmark    |

---

# Alertas e Observações

> [!IMPORTANT]
> Testes automatizados ajudam a evitar bugs durante manutenção do sistema.

---

> [!TIP]
> Sempre escreva testes para funções críticas do projeto.

---

> [!WARNING]
> Cobertura alta não significa ausência total de erros.

---

# Conclusão

Os testes automatizados em Go são simples de criar e extremamente importantes para garantir qualidade e estabilidade no desenvolvimento de software.

Com o pacote `testing`, é possível validar comportamentos, detectar erros rapidamente e manter o código mais seguro e confiável ao longo do tempo.

Além disso, Go oferece ferramentas integradas para benchmark e cobertura de testes, tornando o processo de validação ainda mais eficiente.
