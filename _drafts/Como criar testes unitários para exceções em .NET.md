---
layout: post
title: Como criar testes unitários para exceções em .NET
comments: true
---

# Como criar testes unitários para exceções em .NET

Quanto se utiliza teste unitários é muito fácil testar o que é verdadeiro e o que é falso, ou seja, os fatos baseados no fluxo básico de uma aplicação/componente. Porém, como testar os fluxo de exceção?

Para mostrar o problema, vamos utilizar o código simples a seguir.

	class Person 
	{
		public int Idade {get;private set}
		
		public Person(int idade)
		{
			this.Idade = idade;
		}
		
		public void Check()
		{
			if (this.Idade <= 0)
				throw new UnderageException();
		}
	}
	
O teste do caso básico, quando uma pessoa é preenchida corretamente, é simples.
	
	[TestClass]
	class PersonTest
	{
		
		public void PersonValidation_AgeIsCorrectFilled_PersonInstance()
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
	
Porém, como validar se ocorreu a exceção `UnderageException`? A forma mais simples e prática para fazer a validação é utilizar a anotação `ExpectedExceptionAttribute`. Veja um exemplo:


	[TestMethod]
	[ExpectedException(typeof(UnderageException))]
	public void PersonValidation_AgeIsIncorrect_ThrowUnderageException()
	{
		//
		// Arrange
		//
		var expected = 0;

		//
		// Act
		//
	    	var person = new Person(10);
	}


Nesse caso eu espero que esse teste, ao chegar ao final, dispare uma exceção do tipo `UnderageException`. Usando essa anotação, você economiza a verificação da exceção manual (com `if`'s por exemplo). Além de melhorar a leitura do código.
