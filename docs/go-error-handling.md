````md
# Error Handling em Go

## Introdução

O tratamento de erros (*Error Handling*) em Go é uma das partes mais importantes da linguagem. Diferente de outras linguagens que utilizam `try/catch`, Go utiliza retornos explícitos de erro através do tipo `error`.

Essa abordagem torna o código mais simples, previsível e fácil de manter.

---

## O que é o tipo `error`?

Em Go, erros são representados pela interface nativa `error`.

Ela possui o seguinte método:

```go
type error interface {
    Error() string
}
````

Sempre que uma função pode falhar, normalmente ela retorna:

* O resultado esperado
* Um valor do tipo `error`

---

## Exemplo Básico

```go
package main

import (
    "fmt"
)

func dividir(a float64, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("não é possível dividir por zero")
    }

    return a / b, nil
}

func main() {
    resultado, err := dividir(10, 0)

    if err != nil {
        fmt.Println("Erro:", err)
        return
    }

    fmt.Println("Resultado:", resultado)
}
```

### Saída

```bash
Erro: não é possível dividir por zero
```

---

## Entendendo o `if err != nil`

Esse padrão é extremamente comum em Go.

```go
if err != nil {
    // tratamento do erro
}
```

Significa:

* Se existir erro → trate o erro
* Se não existir erro → continue o programa normalmente

---

## Criando Erros Personalizados

Podemos criar erros usando:

```go
fmt.Errorf()
```

ou

```go
errors.New()
```

### Exemplo

```go
package main

import (
    "errors"
    "fmt"
)

func validarIdade(idade int) error {
    if idade < 18 {
        return errors.New("idade mínima é 18 anos")
    }

    return nil
}

func main() {
    err := validarIdade(15)

    if err != nil {
        fmt.Println("Erro:", err)
        return
    }

    fmt.Println("Acesso permitido")
}
```

---

## Error Wrapping (Encapsulamento de Erros)

Go permite encapsular erros para adicionar contexto.

### Exemplo

```go
package main

import (
    "fmt"
)

func conectarBanco() error {
    return fmt.Errorf("falha ao conectar no banco")
}

func iniciarSistema() error {
    err := conectarBanco()

    if err != nil {
        return fmt.Errorf("erro ao iniciar sistema: %w", err)
    }

    return nil
}

func main() {
    err := iniciarSistema()

    if err != nil {
        fmt.Println(err)
    }
}
```

### Saída

```bash
erro ao iniciar sistema: falha ao conectar no banco
```

---

## Tratando Erros de Arquivos

Um dos usos mais comuns de tratamento de erros é leitura de arquivos.

### Exemplo

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    arquivo, err := os.Open("dados.txt")

    if err != nil {
        fmt.Println("Erro ao abrir arquivo:", err)
        return
    }

    defer arquivo.Close()

    fmt.Println("Arquivo aberto com sucesso")
}
```

---

## Uso do `panic`

O `panic` interrompe a execução do programa imediatamente.

Ele deve ser usado apenas em situações críticas.

### Exemplo

```go
package main

func main() {
    panic("erro crítico no sistema")
}
```

### Saída

```bash
panic: erro crítico no sistema
```

---

## Uso do `recover`

O `recover` permite recuperar um programa após um `panic`.

### Exemplo

```go
package main

import "fmt"

func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Erro recuperado:", r)
        }
    }()

    panic("algo deu errado")
}
```

### Saída

```bash
Erro recuperado: algo deu errado
```

---

## Boas Práticas

### Sempre trate erros

Nunca ignore erros importantes.

❌ Errado:

```go
arquivo, _ := os.Open("dados.txt")
```

✅ Correto:

```go
arquivo, err := os.Open("dados.txt")

if err != nil {
    fmt.Println(err)
}
```

---

### Adicione contexto ao erro

Evite mensagens genéricas.

❌ Ruim:

```go
return err
```

✅ Melhor:

```go
return fmt.Errorf("erro ao salvar usuário: %w", err)
```

---

### Use `panic` apenas em casos extremos

`panic` não deve ser usado para erros comuns do sistema.

Use apenas quando o programa realmente não puder continuar.

---

## Exemplo Prático Completo

```go
package main

import (
    "errors"
    "fmt"
)

func sacar(saldo float64, valor float64) (float64, error) {
    if valor <= 0 {
        return saldo, errors.New("o valor do saque deve ser maior que zero")
    }

    if valor > saldo {
        return saldo, errors.New("saldo insuficiente")
    }

    saldo -= valor

    return saldo, nil
}

func main() {
    saldo, err := sacar(100, 150)

    if err != nil {
        fmt.Println("Erro:", err)
        return
    }

    fmt.Println("Novo saldo:", saldo)
}
```

---

## Resumo

O tratamento de erros em Go é baseado em:

* Retornos explícitos
* Verificação com `if err != nil`
* Uso do tipo `error`
* Criação de erros personalizados
* Encapsulamento de erros com `%w`
* Controle de falhas críticas com `panic` e `recover`

Essa abordagem deixa o código mais transparente e facilita a manutenção do sistema.

---

## Conclusão

Error Handling é uma das características mais importantes da linguagem Go. Embora pareça simples, ele ajuda a criar aplicações mais seguras, organizadas e previsíveis.

Dominar o tratamento de erros é essencial para desenvolver sistemas profissionais em Go.

```
```
