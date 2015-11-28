---
layout: post
title: Testes pra quê?!
comments: true
redirect_from:
    - /testes-pra-que
img: /img/testes-pra-que.jpeg
---

Muitas vezes escuto a frase "testes pra quê?!" no ambiente de desenvolvimento de software. Eu também já disse a mesma frase vez ou outra e hoje tenho outra opinião sobre os testes. Explico o porquê:

Testes não são apenas testes. Os teste são a comprovação/validação de que o seu trabalho (o código) foi feito de forma adequada, de forma que ninguém poderá questionar sua verdade. E também, que você pode dormir em paz ;)

A forma mais fácil de se escrever um testes é encontrar os **fatos** incontestáveis à respeito do código que será executado. Para cada fato, um teste. Verificando todos os fatos envolvidos no seu código, o seu código estará seguro.

Veja um exemplo para testar os fatos de um suposto serviço chamado `CalculartorService` que permite apenas a soma de dois inteiros:

```java
// Teste de exemplo
public class CalculartorServiceTest{

	CalculartorService target = new CalculartorService();
	
	@Test
	public void sumTwoPositivesValues(){
		assertEquals(3, target.sum(1, 2));
	}

	@Test
	public void sumTwoNegativesValues(){
		assertEquals(-3, target.sum(-1, -2));
	}

	@Test
	public void sumTwoNegativesAndPositivesValues(){
		assertEquals(-1, target.sum(1, -2));
		assertEquals(1, target.sum(-1, 2));
	}

	@Test( expected = OverflowExceptions.class )
	public void treatOverflowExceptions(){
		target.sum(Integer.MAX_VALUE, Integer.MAX_VALUE);
	}

	@Test( expected = UnderflowExceptions.class )
	public void treatUnderflowExceptions(){
		target.sum(Integer.MAX_VALUE, Integer.MAX_VALUE);
	}
}

```

Esses simples testes expõem todos os fatos envolvidos na operação de soma de dois inteiros. Basicamente, os teste nos diz que é permitido: 

1. Somar dois números inteiros positivos;
1. Somar dois números inteiros negativos;
1. Somar dois números inteiros negativos e positivos;
1. Tratar de forma segura os casos de _Overflow_ e _Underflow_ na soma de inteiros.


Aí está a garantia necessária que mostra que o que foi feito está funcionando conforme o esperado.

Então não se engane, se você ainda usa a frase "testes pra quê?!", significa que você está fazendo o seu trabalho incompleto, e todos sabem o que acontece com trabalhos incompletos...
