---
layout: post
title: Por que a interface em .Net se chama IEnumerable?
comments: true
---

Recentemente, refletindo sobre como dar o nome para objetos/classes, parei para examinar os nomes dados aos objetos no framework .NET. Um nome que sempre achei estranho era `IEnumerable`. Por que `IEnumerable`? Por que não `IIterable` ou coisas do tipo, já que essa interface permite que, para quem a implementa, expor um enumerador que suporta uma iteração simples sobre uma coleção não gerérica, conforme a definição na [MSDN][1]:

> Exposes an enumerator, which supports a simple iteration over a non-generic collection.

Após ler a referência da [MSDN][1], tentei entender o que realmente significa _enumerar_, ou _o que é alguma coisa enumerável_. Então tudo ficou claro e percebi o quão brilhante e científico foi a escolha do nome `IEnumerable`.

## O que é algo enumerável

Alguma coisa enumerável significa alguma coisa que se possa __contar__. Suponha que essa coisa seja um conjunto. Um conjunto enumerável é um conjunto com infinitos elementos, porém contável. A estes conjuntos é dados o nome de [conjuntos infinitos contáveis][2], e essa é uma definição matemática.

Em poucas e simples (e também superficiais) palavras, toda vez que dizemos que um conjunto (ou uma coleção) é enumerável, significa que é possível contar seus elementos um-a-um, mesmo que esta coleção tenha um tamanho infinito. Parece estanho contar infinitamente, mas é matematicamente provável, pergunta para o [Georg Cantor][3].

Para deixar as coisas mais claras, suponha que você foi encarregado de criar uma classe que representa uma coleção ou um cojunto de objetos e esta coleção é ilimita (estamos despresando o tamanho da sua memória RAM aqui). Essa classe que você irá deve permitir contar cada elemento presenta na coleção, porque para iterarmos sobre os elementos é necessário contarmos cada um deles e sabermos em que posição cada um está.

Considerando as definições matématicas, a forma mais sensata e simples é criar uma classe que represente conjuntos infinitos contáveis, e considerando também o que já foi dito anteriormente, todo conjunto infinito contável também é chamado de enumerável. Eis a grande sacada do nome `IEnumerable`.

> **NOTA:** não encontrei em nenhum lugar a real explicação para o nome `IEnumerable`, mas após ler sobre conjuntos enumeráveis, fiz, quase que imediatamente, a ligação com o nome da interface utilizada no framework .NET.

  [1]: http://msdn.microsoft.com/en-us/library/system.collections.ienumerable(v=vs.110).aspx
  [2]: http://pt.wikipedia.org/wiki/Conjunto_cont%C3%A1vel
  [3]: http://pt.wikipedia.org/wiki/Georg_Cantor
