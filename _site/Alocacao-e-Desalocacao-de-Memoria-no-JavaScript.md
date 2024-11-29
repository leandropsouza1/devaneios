# Entendendo a Alocação e Desalocação de Memória no JavaScript

Olá, pessoal!

Sou novo no JavaScript e estava tentando entender os tipos de dados e como funciona o gerenciamento de memória "por debaixo do capô". O JavaScript por ser uma linguagem de alto nível, muitos dos detalhes técnicos, como o gerenciamento de memória, são abstraídos do desenvolvedor. No entanto, acredito que compreender o mínimo de como funciona a alocação e a desalocação de memória é essencial para escrevermos um código mais eficiente e evitar problemas, como vazamentos de memória.

Fui atrás de materiais para esclarecer melhor esse tema, mas tive dificuldade em encontrar **em um unico lugar** algo que explicasse esse processo de forma mais aprofundada. Por isso, resolvi escrever este artigo para ajudar quem possa ter as mesmas dúvidas que eu!

Neste texto, vou abordar como o motor JavaScript gerencia variáveis, tipos de dados e as áreas de memória (*stack e heap*), além de explicar o funcionamento do *Garbage Collector* (coleta de lixo).


## Tipos de Dados em JavaScript: Imutáveis e Mutáveis

Os tipos de dados no JavaScript podem ser divididos em duas categorias principais:

### 1. Tipos Primitivos (Imutáveis)

Os tipos primitivos são valores simples e imutáveis. Isso significa que, uma vez criados, eles não podem ser alterados diretamente. Quando você modifica um valor primitivo, na verdade, você está descartando o valor original e alocando um novo valor na memória.

Veja o exemplo abaixo:

```
let str = "Hello";
str = str + " World"; // Um novo valor "Hello World" é criado, e "Hello" é descartado.
```

Aqui, o valor original "Hello" não é modificado; em vez disso, um novo valor é alocado na memória.

**Tipos Primitivos no JavaScript**

- Number
- String
- Boolean
- null
- undefined
- Symbol
- BigInt


### 2. Tipos Não Primitivos (Mutáveis)

Os tipos **não** primitivos são os Objects, Arrays e Functions. Eles são mutáveis, o que significa que seus **valores internos** podem ser alterados sem criar uma nova referência.

Exemplo:

```
const obj = { name: "Alice" };
obj.name = "Bob"; // Alteração do valor interno é permitida
```
No exemplo acima, a referencia do objeto na memória _stack_ permanece a mesa, mas seu conteúdo interno pode ser modificado.

**Tipos Mutáveis no JavaScript**

- Objects ({})
- Arrays ([])
- Functions

## Alocação de Memória: Stack e Heap

O motor do JavaScript utiliza duas áreas principais de memória para alocar variáveis: *stack* e *heap*.

### 1. Stack

A memória *stack* é usada para **armazenar valores primitivos e referências** a objetos no *heap*.

É uma estrutura de dados linear e limitada em tamanho, ideal para armazenamento temporário de variáveis locais e chamadas de funções.

Operações no *stack* são extremamente rápidas devido ao seu modo de funcionamento: [LIFO (*Last In, First Out*)](https://pt.wikipedia.org/wiki/LIFO).

Exemplo:

```
let a = 10; // Alocado diretamente no stack
let b = a;  // Nova cópia do valor é criada no stack
```
Neste exemplo, a e b armazenam cópias independentes do valor 10.

### 2. Heap

A memória *heap* é usada para armazenar Objects, Arrays e Functions.

Ao contrário do *stack*, o *heap* é uma área de memória não estruturada e dinâmica, o que o torna ideal para armazenar dados mais complexos e de tamanho variável.

Os objetos armazenados no *heap* são acessados por referências que por sua vez são armazenadas no *stack*.

Exemplo:

```
const obj = { name: "Alice" }; // O objeto está no heap
```

Aqui, a variável obj está alocada na *Heap* e na *stack* contém uma **referência** do endereço de acesso ao local do objeto na *heap*.

### Tabela de referência

Fiz uma tabelinha para facilitar o entendimento de qual é a mutabilidade e o local de alocação dos tipos de dados no JavaScript:

| Tipo | Mutabilidade | Local de Alocação| 
| ---  | --- | --- |
| Number | Imutavel | Stack |
| String | Imutavel | Heap (Por ser de tamanho variável) |
| Boolean | Imutavel | Stack |
| null/undefined | Imutavel | Stack |
| Object | Mutável | Heap (Referência na stack) |
| Array | Mutável | Heap (Referência na stack) |
| Function| Mutável | Heap (Referência na stack) |

## Desalocação de Memória e Garbage Collector

Uma das vantagens do JavaScript é que ele gerencia a memória automaticamente. A desalocação de memória é realizada por meio de um processo conhecido como *Garbage Collection* (Coleta de Lixo) que por sua vez, utiliza um algoritmo chmado [*Mark-and-Sweep*](https://www.geeksforgeeks.org/mark-and-sweep-garbage-collection-algorithm/) para saber quando uma alocação de memoria pode ser liberada. Sendo bem simplista, o processo é mais ou menos assim:

**1. Marcar:**

O motor identifica todas as variáveis que ainda são acessíveis a partir das "raízes" (como o escopo global e a pilha de execução).

**2. Varredura:**

Objetos que não estão marcados como acessíveis são considerados "inacessíveis" e têm sua memória liberada.

**Exemplo de Objeto Inacessível:**

```
let obj = { name: "Alice" };
obj = null; // O objeto original se torna inacessível
```

O *Garbage Collection* detectará que o obj { name: "Alice" } não é mais acessível e liberará a memória associada a ele.

## Problemas Comuns Relacionados à Memória

Apesar de ter todo o processo de gerenciamento de memória automatizado, ainda sim é possível encontrar problemas de memória no JavaScript.

**- Referências circulares**

```
let obj1 = {};
let obj2 = {};
obj1.ref = obj2;
obj2.ref = obj1;
```

Esse tipo de estrutura pode impedir o *Garbage Collector* de liberar memória.

**- Listeners não removidos**

```
window.addEventListener("resize", someFunction);
```
Listeners que não são removidos continuam ocupando memória.

## Conclusão

Tentei simplificar ao máximo o assunto que é um pouco complexo, mas espero que vocês tenham conseguido ter pelo menos uma noção de como o JavaScript aloca e desaloca memória e terem percebido o quanto esse conhecimento é fundamental para podermos escrever códigos mais eficientes e evitar problemas de desempenho. Saber distinguir entre *stack e heap*, e entre valores primitivos e mutáveis, ajuda a prever como as variáveis são tratadas internamente.

Embora o *Garbage Collector* automatize a limpeza de memória, boas práticas, como evitar referências desnecessárias e liberar recursos explicitamente, são essenciais para evitar problemas como vazamentos de memória em aplicações reais.

Se você está desenvolvendo aplicações complexas ou críticas, explorar ferramentas como o Chrome DevTools para análise de memória pode ser uma grande vantagem.


Fontes:

[MDM Data Structures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Data_structures)

[MDM Grammar and Types](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Grammar_and_types)

[MDM Primitive](https://developer.mozilla.org/pt-BR/docs/Glossary/Primitive)
