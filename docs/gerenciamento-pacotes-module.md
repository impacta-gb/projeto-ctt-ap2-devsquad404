# Gerenciamento de Pacotes em Go (Go Modules)

Até agora, você aprendeu a escrever código Go localmente, usar variáveis, estruturas de controle e coleções como slices e maps.  
Mas, em projetos reais, você precisa usar **códigos de outras pessoas** (bibliotecas, pacotes, frameworks) e controlar versões de dependências.  
Em Go, isso é feito com **Go Modules**.

Nesta página, você vai aprender:

- o que é um módulo em Go;
- como inicializar um módulo com `go mod init`;
- como adicionar e atualizar dependências com `go get` e `go mod tidy`;
- e o que são os arquivos `go.mod` e `go.sum`.

---

## O que é um módulo em Go?

Um **módulo (Go Module)** é um conjunto de **pacotes Go** organizados em uma pasta que contém um arquivo `go.mod` na raiz.  
Pode ser pensado como um “projeto” ou “biblioteca” completo, com metadados (versão de Go, dependências, etc.).

- Dentro de um módulo, você pode ter vários pacotes, mas o `go.mod` está sempre na pasta raiz.
- Módulos permitem controlar versões de dependências e compartilhar código com outros programas.

---

## Criando um módulo com `go mod init`

Para começar a usar Go Modules, você precisa transformar sua pasta de projeto em um módulo.

### Passo a passo

1. Navegue até a pasta do seu projeto no terminal:

```bash
cd meu-projeto
```

2. Inicialize o módulo:

```bash
go mod init github.com/seuusuario/meu-projeto
```

- `go mod init <caminho-do-modulo>` cria o arquivo `go.mod`.
- O caminho geralmente é algo parecido com `github.com/usuario/repositorio` (ou outro domínio).

Exemplo de `go.mod` gerado:

```text
module github.com/seuusuario/meu-projeto

go 1.24
```

- `module` define o **nome do módulo** (onde seu código pode ser importado).
- `go 1.24` indica a versão mínima do Go usada no projeto.

---

## Arquivos `go.mod` e `go.sum`

Depois de rodar `go mod init`, você verá dois arquivos principais:

### `go.mod`

- Declara o nome do módulo.
- Lista as dependências externas (outros módulos que seu projeto usa).
- Especifica a versão mínima do Go.

Exemplo de trecho de `go.mod` com dependência:

```text
module github.com/seuusuario/meu-projeto

go 1.24

require (
    github.com/foo/bar v1.2.0
)
```

Esse arquivo é o “manifesto” do seu módulo.

### `go.sum`

- Contém hashes de dependências baixadas, para garantir que o código não foi alterado.
- Ele é mantido automaticamente pelos comandos `go mod`; normalmente você não edita ele à mão.

Se esse arquivo estiver ausente, o Go pode rebaixar ou rebaixar dependências; por isso, ele é importante no controle de versões.

---

## Adicionando dependências com `go get`

Quando você importa um pacote externo no seu código Go, o Go percebe que você precisa de uma dependência e a lista no `go.mod` (ou você pode adicionar manualmente com `go get`).

### Exemplo de uso de `go get`

Digamos que você queira usar um pacote chamado `rsc.io/quote` em um exemplo de código:

1. Importe no código Go:

```go
package main

import (
    "fmt"

    "rsc.io/quote"
)

func main() {
    fmt.Println(quote.Hello())
}
```

2. No terminal, no diretório raiz do módulo:

```bash
go mod tidy
```

O comando `go mod tidy` faz o seguinte:

- Baixa automaticamente dependências necessárias.
- Adiciona possíveis dependências ausentes em `go.mod`.
- Remove dependências que não são mais usadas.
- Atualiza `go.sum` com os hashes das versões.

Você também pode usar explicitamente:

```bash
go get rsc.io/quote
```

para adicionar ou atualizar esse pacote.

---

## Comandos úteis do Go Modules

Alguns comandos que você vai usar com frequência em projetos com Go Modules:

### `go mod init <module path>`

Cria o arquivo `go.mod` e inicia o módulo na pasta atual.

```bash
go mod init github.com/seuusuario/meu-projeto
```

### `go mod tidy`

Limpõe o `go.mod` e `go.sum`, removendo dependências não usadas e adicionando as que faltam.

```bash
go mod tidy
```

Este é o comando mais usado no dia a dia.

### `go mod download`

Baixa para o cache local todas as versões de dependências listadas no `go.mod`, sem compilar o código.

```bash
go mod download
```

Muito útil em CI/CD ou quando quer garantir que as dependências estão baixadas antes de compilar.

### `go get <package>[@version]`

Adiciona ou atualiza uma dependência.

```bash
go get github.com/gin-gonic/gin@v1.10.0
```

Se não passar versão, ele usa a versão mais recente compatível.

---

## Organização de pacotes dentro de um módulo

Um módulo pode conter vários pacotes em subpastas. Por exemplo:

```text
meu-projeto/
  ├── go.mod
  ├── main.go
  ├── utils/
  │   └── stringutil.go
  └── handlers/
      └── api.go
```

- `main.go` pertence ao pacote `main`.
- `stringutil.go` pertence ao pacote `utils`.
- `api.go` pertence ao pacote `handlers`.

Importar um pacote interno:

```go
package main

import (
    "meu-projeto/utils"
    "meu-projeto/handlers"
)
```

- Como o módulo é `github.com/seuusuario/meu-projeto`, o caminho interno é `meu-projeto/...` dentro do código.

---

## Boas práticas com Go Modules

Algumas orientações rápidas para quem está começando:

- Sempre inicie o módulo com `go mod init` antes de começar a usar pacotes externos.
- Sempre rode `go mod tidy` depois de mudanças grandes no código, para deixar `go.mod` e `go.sum` em ordem.
- Use URLs de repositórios públicos como caminho do módulo (ex.: `github.com/usuario/projeto`), facilitando que outras pessoas consumam seu código.
- Nunca “apague à mão” dependências do `go.mod`; use `go mod tidy` para isso.

---

## Conclusão e conexão com o resto da documentação

Nesta página, você aprendeu:

- o que é o conceito de **módulo** em Go;
- como inicializar um módulo com `go mod init`;
- quais são os arquivos `go.mod` e `go.sum` e o que eles fazem;
- e como usar `go get` e `go mod tidy` para gerenciar dependências.

