# 32 Conceitos que todo desenvolvedor JavaScript deveria saber

## Pilha de chamadas

Simular no https://jsbin.com/

Javascript executa o código adicionando em pilha com exceção de quando é algo assincrono.

```javascript
function funcao1() {
	funcao2();
	console.log("executou a função 1");
}

function funcao2() {
	funcao3();
	console.log("executou a função 2");
}

function funcao3() {
	console.log("executou a função 3");
}

funcao1();

//saida:
//"executou a função 3"
//"executou a função 2"
//"executou a função 1"
```

## Tipos Primitivos

Todos tipos primitivos apenas valores únicos. Não são objetos.

Tipos: Boolean, String, Number, Null, Undefined, Symbol(novo), BigIn(novo).

Symbol: Representa valores únicos e imutáveis, frequentemente usados como identificadores exclusivos em objetos.

BigIn: Representa números inteiros de precisão arbitrária, permitindo a representação de números muito grandes.

O Javascript tem construtores para tipos primitivos, como "new Boolean(true)", quando utilizado ele retorna um objeto contendo todos os métodos para manipular o tipo do objeto utilizado.

```javascript
console.log(typeof true); //Boolean
console.log(typeof Boolean(true)); //Boolean
console.log(typeof new Boolean(true)); //Object
console.log(typeof new Boolean(true).valueOf()); //Boolean
console.log(typeof "Gabriel"); //string
console.log(typeof 28); //number
console.log(typeof );

//Exemplo definindo um objeto de numero em uma variavel.

let doze = new Number(12); //object
let quinze = doze + 3; //number

console.log(quinze);//15

//o objeto criado a partir de um tipo primitivo pode ser utilizado também como o seu próprio tipo primitivo que gerou o objeto.

```

## Tipo de valores e tipos de referência

Tipos primitivos são tipos de valor.

```javascript
let x = 10; //memoria.00001 = nome é x, e o valor armazenado é 10
let y = x;
x = 20;

console.log(x, y); // 20 10
```

Tipos de valores adicionado um valor diretamente a uma variável.

```javascript
let x = { valor: 10 };
let y = x;
x.valor = 20;

console.log(x);
/* [object Object] {
    valor: 20
} */
console.log(y);
/* [object Object] {
    valor: 20
} */
```

Tipos de referencia atribuem o valo diretamente, eles passam uma referência da memória, neste caso, X não é o objeto com valor 20, X é apenas um ponteiro que tem o endereço da memória que contém este objeto.

```javascript
//No exemplo acima ocorre o seguinte:
/*
local memoria.00002 = {valor: 10};
variavel x = memoria.00002;
variavel y = x = memoria.00002;
*/
```

Ao atribuir X a Y estou passando para eles o mesmo endereço de memória sendo assim ambos vão ter referência para uma memória que armazena o mesmo valor.

## Implícito, Explicito e chamada de métodos

Coerção ocorre quando o Javascript tenta "adivinhar" o que você ta tenando fazer e faz a conversão para o tipo "esperado". Com a coerção so é possível obter 3 tipos primitivos, string, number ou boolean.

```javascript
console.log("5" - 5); // 0
/* A Coerção funcionou e converteu a string para number */
console.log("5" + 5); // "55"
/* Aqui o JS compreendeu que tentamos concatenar uma string */
console.log(true + 1); // 2
/* true ou false são booleanos de 0 ou 1. True é 1, por tanto o retorno 1 + 1 foi o 2 */
console.log(true + true); // 2
/* Seguindo a lógica do exemplo anterior, neste caso também temos 1 + 1 */
console.log([] + {}); // "[object object]"
console.log([] + []); // ""
```

-   Implicito:

```javascript
console.log(+"5"); //5
console.log(5 + ""); //"5"
console.log(123 && "Hello"); //"Hello"
console.log(null || true); //"true"
```

-   Explicito:

```javascript
console.log(Number("50")); //50
console.log(String(50)); //"50"
```

-   Duck Type: Se anda como um pato e fala como um pato, so pode ser um pato. JS não é uma linguagem fortemente tipada então qualquer coisa pode ser de qualquer tipo.

```java
//exemplo de fortemente tipado em Java
 Public Integer somaNumeros(Integer a, Integer b) {
        return a + b;
}
```

```javascript
//exemplo da mesma função em js
function somaNumeros(a, b) {
	return a + b;
}
```

Neste caso o tipo não importa, se passar um inteiro vai retornar um inteiro, se passar outra coisa vai retornar mesmo que o resultado não seja o esperado.

**Coerções devem ser evitadas, não devemos deixar que o JS converta os tipos para nos.**

## == vs ===

== : Este sinal utiliza coerção por traz.
=== : Irá comparar o valor e o tipo.

```javascript
console.log(3 == "3"); //true
```

**Como o Javascript realiza essa comparação por trás**

1 - Primeiramente o JS verifica se ambos os valores são do memo tipo, se for utiliza o operador ===.

    - Se não for

2 - Verificar se ta comparando null == undefined, se for retorna True.

    - Se não for

3 - Verifica se esta comparando number com string, se for converte a string para number. (volta para o primeiro passo e irá utilizar o operador ===).

    - Se não for

4 - Verifica se esta comparando boolean == number, se for, ele converte o boolean em numero (true == 1, false == 0).

    - Se não for

5 - Verifica se esta comparando boolean com string, se for, ele converte a string para boolean.

    - Se não for

6 - Ultimo passo, verificar se é um objeto e um tipo primitivo, se for, ele converte o objeto em uma string.

Por trás, em seu nível mais baixo, o JS sempre utiliza o operador ===. É bom **evitar comparador duplo**, pois pode passar string comparando com número.

Já com o operador triplo ele irá comparar além do valor o tipo, desta forma é mais seguro e não ocorre nenhum coerção por tras.

```javascript
console.log(3 === "3"); //false
console.log(3 === 3); // true
console.log(2 === 3); // false
```

## Escopo de variáveis

O escopo de váriaveis determina basicamente onde aquela variável pode ser acessada, por exemplo, uma variavel de escopo global pode ser utilizada em qualquer parte do código, enquanto uma variável de função só poderá ser utilizada dentro daquela função.

JS tem 3 tipos de veriáveis: var, let e const

-   var: Possui escopo Global, pode ser acessada de qualquer lugar do código desde que seja declarada fora de uma função.

```javascript
var nome = "Gabriel"; //global

function teste() {
	var sobrenome = "Sobrenome";
	//Apenas pode ser acessada detro da função
}
```

-   let e const: Possuem escopo de **bloco**, ou seja, so estao disponiveis dentro de um abrir e fechar chaves, seja uma função, método, classe, um if...

```javascript
function teste() {
	var sobrenome = "Sobrenome";
	if (sobrenome === "Sobrenome") {
		const valor = 10;
	}
	console.log(valor);
}

teste();
//Retornará erro, pois o valor passado dentro do console log não existe fora do if, por mais que esteja na mesma função.
```

-   Escopo léxico: Quando temos funções aninhadas os recursos das funções declaradas mais "acima" podem ser utilizados nas funcões abaixo.

```javascript
function teste() {
	var nome = "Meu nome";
	function teste2() {
		console.log(nome);
	}
}
```

-   Escopo global: O código por padrão é um escopo global, quando trabalhamos no escopo global estamos falando diretamente do window do seu navegador, esta variável será acessível em **todo** seu código. Sempre que alterar uma variável de escopo global ela será alterada em todo o código.

-   Escopo de função: O que for criado dentro de uma função apenas será utilizado dentro dela, basicamente igual o escopo léxico.

-   Escopo de bloco: Algo declarado apenas está disponível dentro das chaves que ele foi declarado e do que estiver aninhado nestas chaves.

```javascript
function bloco() {
	var teste;
	if (true) {
		teste = "teste";
		let teste2 = "teste2";
	}
}
```
