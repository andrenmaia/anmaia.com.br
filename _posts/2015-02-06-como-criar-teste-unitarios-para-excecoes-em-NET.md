---
layout: post
title: Como criar testes unitários para exceções em .NET
comments: true
redirect_from:
  - /como-criar-teste-unitarios-para-excecoes-em-net
---

Quanto se utiliza testes unitários é muito fácil testar o que é verdadeiro e o que é falso, ou seja, os fatos baseados no fluxo básico de uma aplicação/componente. Porém, como testar os fluxo de exceção?

Para mostrar o problema, vamos utilizar o código simples a seguir.

```java
class Person 
{
	public int Idade {get;private set;}
	
	public Person(int idade)
	{
		this.Idade = idade;
	}
	
	public void Check()
	{
		// Verifica se a pessoa é menor de idade.
		if (this.Idade < 18)
			throw new UnderageException();
	}
}
```
	
O teste do caso básico, quando uma pessoa é preenchida corretamente, é simples.

```java	
[TestClass]
class PersonTest
{
	public void Person_PersonWithCorrectAgeFilled_ValidPersonInstance()
	{
		//
		// Arrange
		//
		var expected = 20;

		//
		// Act
		//
		var person = new Person(20);

		//
		// Assert
		//
		Assert.AreEqual(expected, person.Idade);
	}
}
```
	
Porém, como criar um teste para validar se ocorreu a exceção `UnderageException`? A forma mais simples e prática para fazer o teste é utilizando a anotação `ExpectedExceptionAttribute`. Veja um exemplo:

```java
[TestMethod]
[ExpectedException(typeof(UnderageException))]
public void PersonValidation_AgeIsIncorrect_ThrowUnderageException()
{
	//
	// Arrange
	//
	var person = new Person(10);

	//
	// Act
	//
	person.Check();
}
```


Nesse caso eu espero que esse teste, ao chegar no final, dispare uma exceção do tipo `UnderageException`. Usando essa anotação, você economiza a verificação da exceção manual (com `if`'s por exemplo). Além de melhorar a leitura do código.
