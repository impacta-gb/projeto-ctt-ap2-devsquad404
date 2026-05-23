# Channels em Go

## O que são Channels?

Channels (canais) são estruturas da linguagem Go utilizadas para permitir a comunicação entre goroutines. Eles funcionam como “tubos” por onde os dados passam de uma goroutine para outra de forma segura e sincronizada.

Os channels são um dos principais recursos de concorrência do Go e ajudam a evitar problemas comuns relacionados ao acesso simultâneo de dados.

---

## Criando um Channel

Para criar um channel utilizamos a função `make()`.

### Sintaxe

```go
canal := make(chan tipo)
```

### Exemplo

```go
package main

import "fmt"

func main() {
	canal := make(chan string)

	go func() {
		canal <- "Olá, Channel!"
	}()

	mensagem := <-canal

	fmt.Println(mensagem)
}
```

### Explicação

* `chan string` → cria um canal que transporta strings
* `canal <- valor` → envia um valor para o canal
* `<-canal` → recebe um valor do canal
* `go func()` → executa a função em uma goroutine

---

## Comunicação entre Goroutines

Os channels são usados principalmente para compartilhar informações entre goroutines.

### Exemplo Prático

```go
package main

import (
	"fmt"
	"time"
)

func enviarMensagem(canal chan string) {
	time.Sleep(2 * time.Second)
	canal <- "Processo concluído!"
}

func main() {
	canal := make(chan string)

	go enviarMensagem(canal)

	fmt.Println("Aguardando resposta...")

	mensagem := <-canal

	fmt.Println(mensagem)
}
```

### Saída Esperada

```bash
Aguardando resposta...
Processo concluído!
```

---

## Channels Bloqueantes

Por padrão, os channels são bloqueantes.

Isso significa:

* O envio espera alguém receber
* O recebimento espera alguém enviar

### Exemplo

```go
package main

func main() {
	canal := make(chan int)

	canal <- 10
}
```

### Problema

Esse código gera:

```bash
fatal error: all goroutines are asleep - deadlock!
```

### Motivo

Não existe nenhuma goroutine recebendo o valor enviado.

---

> [!WARNING]
> Um erro de deadlock acontece quando duas partes do programa ficam esperando uma pela outra indefinidamente.

---

## Channels com Buffer

Channels com buffer permitem armazenar valores temporariamente sem bloquear imediatamente.

### Sintaxe

```go
canal := make(chan int, 3)
```

O número `3` representa a capacidade do buffer.

### Exemplo

```go
package main

import "fmt"

func main() {
	canal := make(chan int, 2)

	canal <- 10
	canal <- 20

	fmt.Println(<-canal)
	fmt.Println(<-canal)
}
```

### Saída

```bash
10
20
```

---

## Fechando Channels

Podemos fechar um channel utilizando `close()`.

### Exemplo

```go
package main

import "fmt"

func main() {
	canal := make(chan int)

	go func() {
		for i := 1; i <= 5; i++ {
			canal <- i
		}

		close(canal)
	}()

	for valor := range canal {
		fmt.Println(valor)
	}
}
```

### Explicação

* `close(canal)` fecha o canal
* `range canal` percorre os valores recebidos até o canal ser fechado

---

## Channels Direcionais

Em Go, é possível limitar um channel apenas para envio ou apenas para recebimento.

### Apenas Envio

```go
func enviar(canal chan<- string) {
	canal <- "Mensagem"
}
```

### Apenas Recebimento

```go
func receber(canal <-chan string) {
	fmt.Println(<-canal)
}
```

### Vantagem

Isso aumenta a segurança do código e evita usos incorretos do channel.

---

## Select com Channels

O `select` permite aguardar múltiplos channels ao mesmo tempo.

### Exemplo

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	canal1 := make(chan string)
	canal2 := make(chan string)

	go func() {
		time.Sleep(1 * time.Second)
		canal1 <- "Resposta do canal 1"
	}()

	go func() {
		time.Sleep(2 * time.Second)
		canal2 <- "Resposta do canal 2"
	}()

	select {
	case msg1 := <-canal1:
		fmt.Println(msg1)

	case msg2 := <-canal2:
		fmt.Println(msg2)
	}
}
```

### Explicação

O `select` executa o primeiro channel que responder.

---

## Exemplo Prático Completo

### Sistema Simples de Processamento

```go
package main

import (
	"fmt"
	"time"
)

func processarPedido(id int, canal chan string) {
	time.Sleep(2 * time.Second)

	canal <- fmt.Sprintf("Pedido %d processado", id)
}

func main() {
	canal := make(chan string)

	for i := 1; i <= 3; i++ {
		go processarPedido(i, canal)
	}

	for i := 1; i <= 3; i++ {
		fmt.Println(<-canal)
	}
}
```

### O que esse exemplo demonstra?

* Execução concorrente
* Comunicação entre goroutines
* Sincronização usando channels
* Processamento paralelo

---

## Vantagens dos Channels

* Comunicação segura entre goroutines
* Evita problemas de concorrência
* Facilita sincronização
* Código mais organizado
* Melhor controle de execução paralela

---

## Desvantagens

* Pode gerar deadlocks se usado incorretamente
* Código pode ficar complexo em sistemas grandes
* Uso excessivo pode impactar desempenho

---

## Boas Práticas

### Utilize channels para comunicação

Prefira compartilhar dados através de channels ao invés de acessar variáveis globais.

### Feche channels quando necessário

Sempre feche o canal quando ele não será mais utilizado.

### Evite deadlocks

Garanta que sempre exista envio e recebimento compatíveis.

### Use select em sistemas concorrentes

O `select` ajuda no controle de múltiplos channels simultaneamente.

---

> [!TIP]
> Em Go existe uma frase muito conhecida:
>
> **"Don't communicate by sharing memory; share memory by communicating."**
>
> Isso significa:
>
> “Não se comunique compartilhando memória; compartilhe memória se comunicando.”

---

## Conclusão

Channels são um dos recursos mais poderosos do Go para concorrência. Eles permitem que goroutines troquem informações de maneira segura, organizada e eficiente.

Com channels é possível criar aplicações concorrentes robustas, melhorar desempenho e controlar múltiplas tarefas ao mesmo tempo de forma simples e elegante.
