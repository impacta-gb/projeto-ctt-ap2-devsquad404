````md
# Goroutines em Go

## O que são Goroutines?

As **Goroutines** são uma das funcionalidades mais poderosas da linguagem Go. Elas permitem executar funções de forma concorrente, ou seja, várias tarefas podem acontecer ao mesmo tempo sem bloquear a execução principal do programa.

Uma Goroutine é extremamente leve quando comparada às threads tradicionais de outras linguagens, consumindo menos memória e sendo gerenciada automaticamente pelo runtime do Go.

---

## Como criar uma Goroutine

Para transformar uma função em Goroutine, basta utilizar a palavra-chave `go` antes da chamada da função.

### Exemplo básico

```go
package main

import (
	"fmt"
	"time"
)

func mensagem() {
	fmt.Println("Executando Goroutine")
}

func main() {
	go mensagem()

	time.Sleep(time.Second)
	fmt.Println("Fim do programa")
}
```

### Explicação

- `go mensagem()` cria uma nova Goroutine.
- A função principal (`main`) continua executando normalmente.
- `time.Sleep()` foi utilizado para evitar que o programa finalize antes da Goroutine terminar.

---

## Concorrência em Go

Go trabalha com o conceito de concorrência, permitindo que múltiplas tarefas avancem de forma independente.

Isso é muito útil para:

- Requisições HTTP
- Processamento de arquivos
- Sistemas em tempo real
- Aplicações com múltiplos usuários
- Processamento paralelo

---

## Executando múltiplas Goroutines

Podemos executar várias Goroutines simultaneamente.

### Exemplo

```go
package main

import (
	"fmt"
	"time"
)

func tarefa(nome string) {
	for i := 1; i <= 3; i++ {
		fmt.Println(nome, "executando:", i)
		time.Sleep(time.Millisecond * 500)
	}
}

func main() {
	go tarefa("Goroutine 1")
	go tarefa("Goroutine 2")

	time.Sleep(time.Second * 3)
}
```

### Resultado esperado

```txt
Goroutine 1 executando: 1
Goroutine 2 executando: 1
Goroutine 1 executando: 2
Goroutine 2 executando: 2
...
```

A ordem pode variar, pois as Goroutines executam concorrentemente.

---

## Funções anônimas com Goroutines

Também é possível executar funções anônimas como Goroutines.

### Exemplo

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		fmt.Println("Executando função anônima")
	}()

	time.Sleep(time.Second)
}
```

---

## WaitGroup

O `WaitGroup` é utilizado para esperar que várias Goroutines terminem antes do programa continuar.

Ele pertence ao pacote `sync`.

### Exemplo com WaitGroup

```go
package main

import (
	"fmt"
	"sync"
)

func tarefa(nome string, wg *sync.WaitGroup) {
	defer wg.Done()

	fmt.Println(nome, "finalizada")
}

func main() {
	var wg sync.WaitGroup

	wg.Add(2)

	go tarefa("Tarefa 1", &wg)
	go tarefa("Tarefa 2", &wg)

	wg.Wait()

	fmt.Println("Programa encerrado")
}
```

### Explicação

- `wg.Add(2)` informa quantas Goroutines serão aguardadas.
- `wg.Done()` reduz o contador ao finalizar.
- `wg.Wait()` bloqueia o programa até todas terminarem.

---

## Vantagens das Goroutines

### Baixo consumo de memória

Goroutines são mais leves que threads tradicionais.

### Facilidade de uso

Criar concorrência em Go é simples e intuitivo.

### Melhor desempenho

Permite executar múltiplas tarefas simultaneamente.

### Escalabilidade

Muito utilizadas em servidores e aplicações distribuídas.

---

## Cuidados ao usar Goroutines

Apesar das vantagens, é importante tomar cuidado com:

- Condições de corrida (Race Conditions)
- Compartilhamento de memória
- Sincronização entre tarefas
- Finalização prematura do programa

---

## Exemplo prático

Simulando processamento de pedidos em um sistema.

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func processarPedido(id int, wg *sync.WaitGroup) {
	defer wg.Done()

	fmt.Println("Processando pedido", id)

	time.Sleep(time.Second * 2)

	fmt.Println("Pedido", id, "finalizado")
}

func main() {
	var wg sync.WaitGroup

	for i := 1; i <= 5; i++ {
		wg.Add(1)
		go processarPedido(i, &wg)
	}

	wg.Wait()

	fmt.Println("Todos os pedidos foram processados")
}
```

### O que acontece nesse exemplo?

- Cada pedido é processado em paralelo.
- O sistema consegue lidar com várias tarefas ao mesmo tempo.
- O `WaitGroup` garante que o programa espere todos os pedidos terminarem.

---

## Boas práticas

- Utilize `WaitGroup` para sincronização.
- Evite acessar variáveis compartilhadas sem controle.
- Prefira comunicação entre Goroutines usando Channels.
- Sempre planeje o encerramento das Goroutines.

---

## Conclusão

As Goroutines são um dos principais diferenciais da linguagem Go, permitindo criar aplicações rápidas, eficientes e concorrentes com poucas linhas de código.

Com elas, é possível desenvolver sistemas altamente escaláveis e preparados para múltiplas tarefas simultâneas.

---

# Alertas e Observações

> ⚠️ Atenção:
> O programa principal pode finalizar antes das Goroutines terminarem. Utilize `WaitGroup` ou outras técnicas de sincronização.

> 💡 Dica:
> Goroutines funcionam muito bem em conjunto com Channels, permitindo comunicação segura entre tarefas concorrentes.

> 🚀 Curiosidade:
> Milhares de Goroutines podem ser executadas simultaneamente devido ao baixo custo de memória.
````
