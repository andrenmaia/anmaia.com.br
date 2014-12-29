---
layout: post
title: Quanto utilizar struct em .net 
comments: true
---

# Quanto utilizar struct em .net 

`RuleResults` não deveria ser uma `struct`. Explico por que:

Estruturas devem ser utilizadas somente quando expressarem a unicidade; quando estiverem relacionadas a um único valor. Exemplos de estruturas são: Money e DateTime. Essas estruturas expressão um único valor, que são: um valor monetário e um data com hora, ambos são atômicos/indivisíveis. 

Basicamente, `struct`s em C# são utilizadas para implementar o padrão [`Value Object`][1]. Esse padrão sugere, como ponto principal, que um `ValueObject` deve ser imutável e esse é outro motivo para utilizarmos `struct`s, porque `struct`s em C# são _by value_ e não _by reference_.

Além das recomendações sobre o padrão e o significado, existe outra recomendação importante da `Microsoft` (veja em [Choosing Between Class and Struct][2]). 

As recomendações da Microsoft, que são várias, a que utilizo primeiramente para eliminar/aceitar a utilização de uma `struct` é a recomendação dos 16 bytes. Essa recomendação, para mim, é como a regras do _a craseado_, a primeira coisa que verificou é se a palavra a seguir é feminina, senão esqueço a crase (até hoje não sei se isso está certo, mas tudo bem, vamos prosseguir). A regra/recomendação da MSDN é que toda estrutura deve ter **abaixo de 16 bytes como tamanha da instância**. A `struct` `RuleResults` tem, aproximadamente 20 bytes (5 propriedade de Int32 = (32-bit * 5) / 8 = 20 bytes).

Por esses motivos e outros, sugiro que troque a `RuleResults` para uma classe ao invés de uma `struct`.


 [1]: http://martinfowler.com/bliki/ValueObject.html
 [2]: http://msdn.microsoft.com/en-us/library/ms229017%28v=vs.110%29.aspx

