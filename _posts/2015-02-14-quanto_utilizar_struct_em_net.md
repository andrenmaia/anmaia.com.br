---
layout: post
title: Quanto utilizar struct em .NET
comments: true
---

Estruturas devem ser utilizadas somente quando expressarem a unicidade; quando estiverem relacionadas a um único valor. Exemplos de estruturas são: `Money` e `DateTime`. Essas estruturas expressão um único valor, que são: um valor monetário e uma data com hora, ambos são atômicos/indivisíveis.

Basicamente, `struct`s em C# .NET são utilizadas para implementar o padrão [`Value Object`][1]. Esse padrão sugere, como ponto principal, que um `ValueObject` deve ser imutável e, uma das formas de armazenar valores imutáveis em C# .NET é utilizando `struct`s, já que o comportamento básico de uma `struct`s é armazenar o valor _by value_ e não _by reference_.

Além das recomendações sobre o padrão e o significado, existes outras recomendações importantes da `Microsoft` (veja em [Choosing Between Class and Struct][2]).

Quando fico em dúvida se devo utilizar uma classe ou uma `struct`, a recomendação da Microsoft que considero logo de início é: que o tamanho do dado armazenado não deve ultrapaçar 16 bytes, ou seja, toda estrutura deve ter **abaixo de 16 bytes como tamanha da instância**.


Considere os exemplos a seguir.

**QueryResult**

`QueryResult` representa o resultado genérico de uma consulta ao banco de dados. Os dados que devem estar contidos nessa representação são:  total de linhas retornadas na consulta e um array de objetos retornados do banco de dados. Nesse caso, `QueryResult` deve ser representado por uma classe, porque o seu tamanha certamente ultrapassará 16 bytes.

```java
class QueryResult {
    public int Lines {get;set;}
    public object[] Resuts {get;set;}
}
```


**Point**

`Point` representa um ponto em duas dimensões, composto por `x` e `y`. Nesse caso, cada valor seria um inteiro (32 bits ou 4 bytes cada), o que dá um total de 8 bytes. Além do tamanho do dado armazendo, o significado de `Point` é conciso e atômico e se encaixa perfeitamente na descrição de `Value Object`. Sendo assim `Point` certamente deveria ser implementado como uma `struct`.

```java
struct Point {
    int x;
    int y;
}

```

 [1]: http://martinfowler.com/bliki/ValueObject.html
 [2]: http://msdn.microsoft.com/en-us/library/ms229017%28v=vs.110%29.aspx

