---
layout: post
title: Afinal, para que servem os design patterns?
comments: true
---

# Afinal, para que servem os design patterns?

Todos que come�am a programar aprendem: para que serve a l�gica; o que s�o algoritmos; o que � orienta�ao a objetos e algum linguagem que permite utilizar esse paradigma. Ap�s (e durante) esse per�odo de aprendizado, frequentemente voc� se depara com algo chamado _design patterns_ ([GoF][1]) e v�rios outros tipode de _patterns_: arquiteturais, estruturais, entre outros milhares que existem. Mas, afinal, para que servem esses patterns?

� comum que durante o per�odo de aprendizado voc� queira (desesperadamente) aprender todos os padr�es poss�vel (comigo foi assim). Porque algu�m lhe disse que seria o melhor para aprender a programar; que seria bom para a sua carreira e que voc� se destacaria entre os outros programadores, se conhecesse os padr�es. Realmente, � fundamental entender os pradr�es b�sico ([GoF][1]), por�m, porque � fundamental? Qual o verdadeiro ganho nisso? Muitos dizem que "o c�digo fica mais bonito", que fica mais f�cil "resolver problemas" utilizando os padr�es. Mas isso n�o � bem um verdade, muitas vezes, resolver um problema com um padr�o n�o deixa o seu c�digo "mais bonito" (c�digos tem que funcionar, n�o serem bonitos!), e nem sempre � mais f�cil aplicar um padr�o para resolver um problema. O grande ganho no uso de padr�es est� na **comunica��o**.

## Comunica��o

Suponha que voc� deve resolver o problema a seguir:

Toda vez que um `Pagamento` for criado ou toda vez que o seu valor for alterado, � necess�rio registrar essa informa��o em um banco de dados relacional e tamb�m em um arquivo de texto.

Exemplo do arquivo texto de registro de altera��o do pagamento.

Pagamento #1 - criado
Pagamento #1 - pre�o alterado de 100.00 para 101.00

Dado o problema, a resposta curta, simple e objetiva para resolver esse problema seria: implemente o padr�o [`Observer`][2]. Pronto! Dito isso, est� tudo explicado em detalhes. Agora, voc� j� sabe como implementar um algoritmo para resolver o problema descrito em v�rias linguagens diferentes. � por isso que patterns dizem respeito a comunica��o. Apenas o nome de um padr�o j� � o suficiente para enteder toda a ideia do que e como deve ser feito.

## Conclus�o

Sempre que falamos de _desing patterns_ estamos falando de comunica��o. � muito mais f�cil passar um instru��o do tipo: implemente um [Abstract factory][3] para criar esses conjuntos de objetos necess�rio para fazer a compra de um produto; ou use o [Chain of responsibility][4] para restringir o acesso do usu�rio nas requisi��es desta API. � comunica��o � simples e direta, com apenas o nome do padr�o est� implicito como fazer e o que fazer.



 [1]: http://en.wikipedia.org/wiki/Design_Patterns
 [2]: http://en.wikipedia.org/wiki/Observer_pattern
 [3]: http://en.wikipedia.org/wiki/Abstract_factory_pattern
 [4]: http://en.wikipedia.org/wiki/Chain-of-responsibility_pattern