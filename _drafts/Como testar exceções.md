---
layout: post
title: Como testar exceções em .net 
comments: true
---

# Como testar exceções em .net 

.. para os casos onde é necessário verificar se ocorreu uma exceção no teste, você pode usar (para facilitar a sua vida) a anotação `ExpectedExceptionAttribute`.

Veja um exemplo:


	[TestMethod]
	[ExpectedException(typeof(ApplicationException))]
	public void PluginExecution_ListOfObject_AddObjects()
	{
		//
		// Arrange
		//
		var emptyList = new List<IObject>();
		
		//
		// Act
		//
	    new CiscoParserPlugin()
			.AddObjects(emptyList);
	}


Nesse caso eu espero que esse teste, ao chegar ao final, dispare um exceção do tipo `ApplicationException`. Usando essa anotação, você economiza a verificação da exceção manual. Além de melhorar a leitura ;)