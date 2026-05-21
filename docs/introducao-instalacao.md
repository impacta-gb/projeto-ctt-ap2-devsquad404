# Introdução à linguagem Go

## O que é Go?

Go (ou Golang) é uma linguagem de programação criada pelo Google, com foco em simplicidade, desempenho e concorrência.  
Ela é muito usada para:
- Serviços web e APIs.
- Ferramentas de linha de comando.
- Sistemas distribuídos e backend de aplicações.

Uma das grandes vantagens do Go é que ele gera programas a partir de um único binário, o que facilita o deploy e a manutenção em produção.

## Principais características

Algumas características que tornam o Go interessante:

- **Sintaxe simples**: tem poucas palavras‑chave e uma estrutura mais limpa, o que ajuda quem está começando.
- **Concorrência nativa**: o Go tem mecanismos internos para lidar bem com múltiplas tarefas ao mesmo tempo (goroutines e channels).
- **Compilação rápida**: o código é compilado para um arquivo executável, sem precisar de máquina virtual.
- **Ferramentas de desenvolvimento embutidas**: o próprio `go` traz comandos para build, testes, formato do código, etc.

---

## Instalação do Go

Antes de escrever código em Go, você precisa instalar a linguagem no seu computador.

### Passo 1 – Acessar o site oficial

A forma mais recomendada de instalar o Go é diretamente do site oficial:

- Acesse: <https://go.dev>
- Procure a seção de download ou clique em “Download Go”.

Lá você vai encontrar versões para Windows, macOS e Linux.

### Passo 2 – Baixar e instalar

No macOS, normalmente você:

- Baixa o instalador `.pkg` para macOS.
- Abre o arquivo baixado e segue o assistente de instalação.
- O instalador adiciona o Go ao seu `PATH`, permitindo usar o comando `go` no terminal.

No Linux e Windows, o processo é semelhante, mas o arquivo de instalação é diferente (`.tar.gz` ou `.msi`).

### Passo 3 – Verificar a instalação

Depois que o Go estiver instalado, abra o terminal e execute:

```bash
go version
```

Se tudo estiver certo, o terminal mostrará algo como:

```text
go version go1.xx.x darwin/amd64
```

Ou semelhante, dependendo da sua máquina. Isso indica que o Go foi instalado corretamente.

---

## Primeiro programa em Go

Agora que você já tem o Go instalado, vamos criar um programa simples para testar tudo.

### Passo 1 – Criar um arquivo Go

1. Abra um editor de texto (Visual Studio Code, Vim, Notepad++, etc.).
2. Crie um arquivo chamado `hello.go`.
3. Cole o código abaixo:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

### Passo 2 – Entendendo o código

- `package main` indica que este é o programa principal.
- `import "fmt"` importa o pacote de formatação de texto, que permite imprimir mensagens.
- `func main()` é a função principal: o programa começa a executar por aqui.
- `fmt.Println("Hello, Go!")` imprime a mensagem na tela.

### Passo 3 – Executar o programa

No terminal, navegue até a pasta onde está o arquivo `hello.go` e rode:

```bash
go run hello.go
```

Se tudo estiver funcionando, você verá no terminal:

```text
Hello, Go!
```

Pronto! Você acaba de rodar o seu primeiro programa em Go.

---

## Dica para continuar

Agora que você instalou o Go e executou seu primeiro programa, já pode começar a explorar:
- sintaxe básica (variáveis, tipos, constantes);
- estruturas de controle (if, for, switch);
- e os outros tópicos que você vai documentar nas páginas seguintes deste site.

Esta página é justamente o primeiro passo para quem está começando com a linguagem Go.