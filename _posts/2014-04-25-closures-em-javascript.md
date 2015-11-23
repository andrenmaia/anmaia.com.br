---
layout: post
title: Closures em Javascript
comments: true
---

_Closure_ é uma recurso presente nas linguagens do paradigma funcional. Uma linguagem que tem uma base muito forte nesse paradigma (e é a linguagem mais utiliza atualmente), é a Javascript.

Para explicar de uma forma bem rápida e prática o que é _closure_, veja a (minha) definição:

> _Closure_ é permitir que o escopo de uma função mais interna, acesse o escopo de uma função mais externa.

Para entender, veja o exemplo mais clássico de _closures_:
{% highlight javascript %}
function somador(numero) {
	// escopo mais externo.

	return function(paraSomar){
		// escopo mais interno.
		return numero + paraSomar;
	};
}

var soma5 = somador(5);
console.log(soma5(3));

// output
// > 8
{% endhighlight %}
Por causa do _closure_ podemos acessar a variável `numero` , que pertence a função somador (a função mais externa), dentro da função mais interna.
