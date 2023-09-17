# 32 Conceitos que todo desenvolvedor JavaScript deveria saber

## Pilha de chamadas

Simular no https://jsbin.com/

É importante ter em mente que o Javascript, por padrão, executa o código adicionando em pilhas. Mesmo em caso de código **assincrono** o JS irá seguir uma pilha, apenas alterando a ordem para aguardar as promises.

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

Todos tipos primitivos são apenas valores único, não podem ser objetos.

Tipos: Boolean, String, Number, Null, Undefined, Symbol(novo), BigIn(novo).

Symbol: Representa valores únicos e imutáveis, frequentemente usados como identificadores exclusivos em objetos.

BigIn: Representa números inteiros de precisão arbitrária, permitindo a representação de números muito grandes.

No Javascript pode-se utilizar construtores para tipos primitivos, como "new Boolean(true)", quando utilizado ele retorna um objeto contendo todos os métodos para manipular aquele determinado tipo.

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

//O objeto criado a partir de um tipo primitivo pode ser utilizado também como o seu próprio tipo primitivo que gerou o objeto.

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

Tipos de referência não atribuem o valor diretamente, eles passam uma referência da memória, neste caso, X não é o objeto com valor 20, X é apenas um ponteiro que tem o endereço da memória que contém este objeto.

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

Coerção ocorre quando o Javascript tenta "adivinhar" o que estamos tentando fazer e faz a conversão para o tipo "esperado". Com a coerção só é possível obter 3 tipos primitivos, string, number ou boolean.

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

-   Implícito:

```javascript
console.log(+"5"); //5
console.log(5 + ""); //"5"
console.log(123 && "Hello"); //"Hello"
console.log(null || true); //"true"
```

-   Explícito:

```javascript
console.log(Number("50")); //50
console.log(String(50)); //"50"
```

-   Duck Type: "Se anda como um pato e fala como um pato, só pode ser um pato"

    Como sabemos Javascript não é uma linguagem fortemente tipada então qualquer coisa pode ser de qualquer tipo.

```java
//exemplo de uma linguagem fortemente tipada em Java
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

Neste caso o tipo não importa, se passar um inteiro vai retornar um inteiro, se passar qualquer outra coisa vai retornar mesmo que o resultado não seja o esperado.

Fuja dos erros!
**Coerções devem ser evitadas, não devemos deixar que o JS converta os tipos para nos.**

## == vs ===

-   == : O comparador duplo utiliza coerção por traz.
-   === : Já o comparador triplo irá comparar o valor e o tipo.

```javascript
console.log(3 == "3"); //true
```

**Vamos ver como o Javascript realiza esse processo por trás utilizando comparador duplo:**

1 - Primeiramente o JS verifica se ambos os valores são do mesmo tipo, se for utiliza o operador === e retorna true ou false.

    - Se não for

2 - Verificar se esta comparando null == undefined, se for retorna true, e volta para o passo 1.

    - Se não for

3 - Verifica se esta comparando number com string, se for converte a string para number, e volta para o passo 1.

    - Se não for

4 - Verifica se esta comparando boolean == number, se for, ele converte o boolean em numero (true == 1, false == 0), e volta para o passo 1.

    - Se não for

5 - Verifica se esta comparando boolean com string, se for, ele converte a string para boolean, e volta para o passo 1.

    - Se não for

6 - Ultimo passo, verificar se esta comparando um obejeto e um tipo primitivo, se for, ele converte o objeto em uma string.

Por trás, em seu nível mais baixo, o JS sempre utiliza o operador ===.
É bom **evitar o comparador duplo**, por exemplo, dependendo da situação comparar uma string com um number poderá causar problemas, mesmo que no primeito momento retorne o valor esperado.

Já com o operador triplo, como vimos antes, o Javascript irá comparar além do valor o tipo, desta forma é mais seguro e não ocorre nenhum coerção por trás.

```javascript
console.log(3 === "3"); //false
console.log(3 === 3); // true
console.log(2 === 3); // false
```

## Escopo de variáveis

O escopo de variáveis determina basicamente onde aquela variável pode ser acessada, por exemplo, uma variável de escopo global pode ser utilizada em qualquer parte do código, enquanto uma variável de função só poderá ser utilizada dentro daquela função.

JS tem 3 tipos de veriáveis: var, let e const

-   var: Possui escopo Global, pode ser acessada de qualquer lugar do código desde que seja declarada fora de uma função.

```javascript
var nome = "Gabriel"; //Global

function teste() {
	var sobrenome = "Sobrenome";
	//Apenas pode ser acessada detro da função
}
```

-   let e const: Possuem escopo de **bloco**, ou seja, só estão disponíveis dentro de um abrir e fechar chaves, seja uma função, método, classe, if...

```javascript
function teste() {
	var sobrenome = "Sobrenome";
	if (sobrenome === "Sobrenome") {
		const valor = 10;
	}
	console.log(valor);
}

teste();
//Retornará erro, pois o valor passado dentro do console.log não existe fora do if, por mais que esteja na mesma função.
```

-   Escopo léxico: Quando temos funções aninhadas(função dentro de função) os recursos das funções declaradas mais "acima" podem ser utilizados nas funcões abaixo.

```javascript
function teste() {
	var nome = "Meu nome";
	function teste2() {
		console.log(nome);
	}
}
```

-   Escopo global: O código por padrão já possui um escopo global, quando trabalhamos no escopo global estamos falando diretamente do window do seu navegador, esta variável será acessível em **todo** seu código. Sempre que alterar uma variável de escopo global ela será alterada em todo o código.

-   Escopo de função: O que for criado dentro de uma função apenas será utilizado dentro dela, basicamente igual ao escopo léxico.

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

## Expressão e Declaração

No JS uma expressão pode se comportar como uma declaração mas o inverso não ocorre.

-   Uma expressão é todo pedaço de código que retorna um valor único.

```javascript
console.log(1 + 1);

function expressao() {
	return 1 + 1;
}

//ambos os casos se tratam de expressões.
```

-   Uma declaração são pedaços de código que realizam uma ação. Por exemplo a declaração de uma variável.

```javascript
let variavel = 20;

if (true) {
	variavel = 30;
}
```

Tudo que for uma ação é considerada uma declaração.
No Javascript nunca podemos considerar uma declaração quando um valor é esperado.

## IIFE, Namespaces e módulos

-   IIFE: Immediately invoked function expression (Expressão de função imediatamente invocada).

Vamos rever como criamos uma função...

```javascript
function alerta() {
	alert("Hello World!");
}

//OU

const alerta = () => {
	alert("Hello World!");
};
```

Agora uma função IIFE...

```javascript
!(function () {
	alert("Hello World!");
})();

//O símbolo de exclamação neste caso serve para o JS saber que se trata de uma expressão.
```

Irá funcionar da mesma forma.
Contudo esta função é tratada pelo JS como uma expressão e será executada apenas uma vez e não poderá ser executada novamente a não ser que rode o codigo de novo.

Também é possível nomear uma IIFE sem problemas. O importante é lembrar do símbolo que torna ela uma expresão, ou seja, inserir a exclamação no começo.

IIFEs eram muito utilizadas para criar Namespaces.

Por exemplo:

```javascript
let dados = !(function () {
	let contador = 0;
    return {
        incrementar = function() {
            contador++
            return contador
        }
    }
}());
```

Um **"namespace"** é utilizado para organizar e isolar elementos, como variáveis, funções, classes, objetos ou outros identificadores, de modo a evitar conflitos de nomes e colisões em um sistema.
O objetivo principal de um namespace é **_evitar a poluição do espaço de nomes, o que pode levar a erros e comportamentos inesperados em um programa ou sistema._**

-   Módulos

Modulos são pedaços de códigos encapsulados que podem ser importados e expõem alguns métodos que podem ser utilizados.
