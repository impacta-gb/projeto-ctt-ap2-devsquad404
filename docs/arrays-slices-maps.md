# Arrays, Slices e Maps em Go

Depois de aprender sobre variáveis e estruturas de controle, o próximo passo natural é entender como armazenar e manipular **vários valores de uma vez**.  
Em Go, os três tipos de coleções mais usados são:

- **Arrays**
- **Slices**
- **Maps**

Nesta página, você vai ver:

- o que é cada um;
- quando usar array, slice ou map;
- e como percorrer essas estruturas com `for range`.

---

## Arrays: coleções de tamanho fixo

Um **array** em Go é uma sequência de elementos do mesmo tipo, com **tamanho fixo** definido em tempo de compilação.  
Ou seja, depois de criar um array, você não pode aumentar ou diminuir o número de elementos.

### Como declarar e inicializar um array

Sintaxe básica:

- `var nome [tamanho]tipo`
- ou `nome := [tamanho]tipo{valores}`

Exemplo:

```go
package main

import "fmt"

func main() {
    // Array de 3 inteiros
    var notas [1]int
    notas = 8
    notas[2] = 9
    notas[3] = 7

    fmt.Println("Notas:", notas)
}
```

Você também pode inicializar direto:

```go
var temperaturas [4]float64 = [4]float64{25.5, 26.0, 24.8, 27.2, 26.9}
```

ou usar a forma mais curta:

```go
sequencia := [5]int{10, 20, 30, 40}
```

- O tamanho (`[4]`) faz parte do tipo: `int` e `[]int` são tipos diferentes.  
- Acessar um índice fora do tamanho (por exemplo, `sequencia[4]` em um `len(sequencia) == 4`) causa erro em tempo de execução.

---

## Slices: arrays dinâmicos e flexíveis

Um **slice** é a forma mais comum de “lista” em Go.  
Slices são baseados em arrays, mas são muito mais flexíveis: podem crescer e encolher conforme necessário.

### Como criar um slice

Existem três formas principais:

1. **Slice literal** (mais comum):

```go
numeros := []int{10, 20, 30, 40}
```

Note que **não se coloca tamanho**: `[]int`, não `[4]int`.

2. **Criar a partir de um array**:

```go
var array [4]int = [4]int{1, 2, 3, 4, 5}
subslice := array[1:4] // elementos de índice 1 a 3
fmt.Println("Subslice:", subslice)
```

3. **Usar `make`** para criar um slice vazio com capacidade:

```go
slice := make([]int, 0, 5) // 0 elementos, capacidade 5
```

- `len(slice)` → número de elementos atuais;
- `cap(slice)` → capacidade máxima sem realocar o array interno.

### Adicionar elementos com `append`

Para aumentar um slice, usa-se `append`:

```go
nomes := []string{"Ana", "Bruno"}

nomes = append(nomes, "Carla")
nomes = append(nomes, "Daniel")

fmt.Println("Lista de nomes:", nomes)
```

- `append` retorna um novo slice (possivelmente com um array interno redimensionado).

---

## Maps: chaves e valores

Um **map** é uma estrutura de dados associativa: você armazena **pares chave/valor**.  
É semelhante a dicionários em outras linguagens.

### Como criar um map

Duas formas principais:

1. **Usando `make`**:

```go
idades := make(map[string]int)

idades["Ana"] = 25
idades["Carlos"] = 30

fmt.Println("Idades:", idades)
```

2. **Usando map literal**:

```go
telefones := map[string]string{
    "Ana":    "9999-0000",
    "Carlos": "8888-1111",
}
fmt.Println("Telefones:", telefones)
```

- A chave (`string`, `int` e outros tipos comparáveis) identifica o valor armazenado.
- Mapas **não garantem ordem** na iteração; o Go pode mostrar os pares em ordem diferente entre execuções.

### Acessar e verificar existência de uma chave

Para acessar um valor:

```go
idade := idades["Ana"]
fmt.Println("Idade de Ana:", idade)
```

Se a chave não existir, o valor retornado é o **zero value** do tipo (no exemplo, `0` para `int`).  
Para saber se uma chave existe:

```go
if valor, existe := telefones["Ana"]; existe {
    fmt.Println("Telefone de Ana:", valor)
} else {
    fmt.Println("Ana não está no mapa.")
}
```

Assim você evita confundir “não existe” com “valor zero”.

---

## Como percorrer arrays, slices e maps

Em Go, o comando `for range` funciona para todos esses três tipos.

### Percorrer arrays e slices

```go
numeros := []int{10, 20, 30}

for indice, valor := range numeros {
    fmt.Printf("índice: %d, valor: %d\n", indice, valor)
}
```

- `indice` → posição do elemento.
- `valor` → valor do elemento.

### Percorrer maps

```go
ages := map[string]int{"Ana": 25, "Carlos": 30, "Diana": 28}

for nome, idade := range ages {
    fmt.Printf("%s tem %d anos\n", nome, idade)
}
```

- A primeira variável recebe a chave.
- A segunda, o valor.

---

## Quando usar array, slice ou map?

Algumas orientações práticas para iniciantes:

- Use **array** se você precisa de um tamanho fixo conhecido em tempo de compilação (por exemplo, dias da semana, notas de uma prova com 5 questões).  
- Use **slice** em quase todos os outros casos em que você precisa de uma lista de elementos que pode crescer ou mudar de tamanho.  
- Use **map** quando quiser “procurar por chave”: nomes de pessoas, códigos de produtos, chaves de configuração, etc.

Em código real, você verá muito mais **slices** e **maps** do que arrays puros.

---

## Conclusão e próximos passos

Nesta página você viu:

- a diferença entre **array** (tamanho fixo) e **slice** (dinâmico e flexível);
- como usar **maps** para armazenar pares chave/valor;
- e como percorrer arrays, slices e maps com `for range`.

Com isso, você já domina as principais estruturas de coleção em Go, que são usadas quase em todos os programas.

Na próxima página, você vai aprender sobre **Go Modules**, que são a forma oficial do Go para gerenciar dependências e pacotes no seu projeto, fechando a parte de “Fundamentos” da sua responsabilidade.