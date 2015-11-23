---
layout: post
title: Afinal, para que servem os design patterns?
comments: true
---

# Afinal, para que servem os design patterns?

Todos que começam a programar aprendem: para que serve a lógica; o que são algoritmos; o que é orientaçao a objetos e algum linguagem que permite utilizar esse paradigma. Após (e durante) esse período de aprendizado, frequentemente você se depara com algo chamado _design patterns_ ([GoF][1]) e vários outros tipode de _patterns_: arquiteturais, estruturais, entre outros milhares que existem. Mas, afinal, para que servem esses patterns?

É comum que durante o período de aprendizado você queira (desesperadamente) aprender todos os padrões possível (comigo foi assim). Porque alguém lhe disse que seria o melhor para aprender a programar; que seria bom para a sua carreira e que você se destacaria entre os outros programadores, se conhecesse os padrões. Realmente, é fundamental entender os pradrões básico ([GoF][1]), porém, porque é fundamental? Qual o verdadeiro ganho nisso? Muitos dizem que "o código fica mais bonito", que fica mais fácil "resolver problemas" utilizando os padrões. Mas isso não é bem um verdade, muitas vezes, resolver um problema com um padrão não deixa o seu código "mais bonito" (códigos tem que funcionar, não serem bonitos!), e nem sempre é mais fácil aplicar um padrão para resolver um problema. O grande ganho no uso de padrões está na **comunicação**.

## Comunicação

Suponha que você deve resolver o problema a seguir:

Toda vez que um `Pagamento` for criado ou toda vez que o seu valor for alterado, é necessário registrar essa informação em um banco de dados relacional e também em um arquivo de texto.

Exemplo do arquivo texto de registro de alteração do pagamento.

Pagamento #1 - criado
Pagamento #1 - preço alterado de 100.00 para 101.00

Dado o problema, a resposta curta, simple e objetiva para resolver esse problema seria: implemente o padrão [`Observer`][2]. Pronto! Dito isso, está tudo explicado em detalhes. Agora, você já sabe como implementar um algoritmo para resolver o problema descrito em várias linguagens diferentes. É por isso que patterns dizem respeito a comunicação. Apenas o nome de um padrão já é o suficiente para enteder toda a ideia do que e como deve ser feito.

## Conclusão

Sempre que falamos de _desing patterns_ estamos falando de comunicação. É muito mais fácil passar um instrução do tipo: implemente um [Abstract factory][3] para criar esses conjuntos de objetos necessário para fazer a compra de um produto; ou use o [Chain of responsibility][4] para restringir o acesso do usuário nas requisições desta API. É comunicação é simples e direta, com apenas o nome do padrão está implicito como fazer e o que fazer.



 [1]: http://en.wikipedia.org/wiki/Design_Patterns
 [2]: http://en.wikipedia.org/wiki/Observer_pattern
 [3]: http://en.wikipedia.org/wiki/Abstract_factory_pattern
 [4]: http://en.wikipedia.org/wiki/Chain-of-responsibility_pattern