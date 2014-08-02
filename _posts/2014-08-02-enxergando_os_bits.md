---
layout: post
title: Enxergando os bits
comments: true
---


As linguagens de programação atuais nos distanciam cada vez mais do baixo nível. Daí vem a pergunta: E se eu quiser enxergar os bits de um tipo primitivo, o que eu devo fazer?

Por exemplo, em C# e Java, um `integer` tem 32 bits, um `short` tem 16 bits, e assim por diante. Para conseguir visualizar aquilo que os professores desenham na lousa (representação de tipo de dado na memória - blocos de 0's e 1's), eu criei em C# um código rápido e simples que exibe os bits de alguns tipos primitivos. Batizei o projeto de [_ShowMeTheBits_][1].

Tudo fica muito simples quando se trabalha com C#, inclusive para visualizar o dado em baixo nível. Para chegar a informação (ao bit), utilizei apenas a classe `BitConverter`. Essa classe tem o método estático `GetBytes(...)` que permite, como parâmetro, qualquer tipo primitivo.

Então funcionada assim:

> Passo o tipo primitivo para o `GetBytes` em `BitConverter` e obtenho um `byte[]`.

Veja um exemplo:

```java
var bytes = BitConverter.GetBytes(8);
```
Nesse exemplo, o `GetBytes` retorna um array de bytes de um `int` de valor 8. Separado em blocos de 4 bits ficaria assim: `0000 0000 0000 0000 0000 0000 0000 1000`. A cada 2 blocos de 4 bits, ou seja, a cada 8 bits, temos 1 byte, totalizando 4 bytes ou 32 bits.


A partir do `byte[]` é só trabalhar a `string` para exibir em blocos de 4 bits.

A seguir um método que fiz para formatar o `byte[]` em blocos de 4 bits.

```java
private static void printBits(byte[] bytes)
{
    var str = new List<string>();
    var bits = new BitArray(bytes);

    for (int i = 0; i < bits.Length; i++)
    {
        var bit = bits[i];
        if (bit)
            str.Add("1");
        else
            str.Add("0");

        // separator
        if ((i+1) % 4 == 0)
            str.Add(" ");
    }

    // print the bits list in string.
    Console.WriteLine(string.Join("", string.Join("", str).Reverse()));
}
```



Para obter o código do projeto veja no [GitHub][1].

  [1]: https://github.com/andrenmaia/ShowMeTheBits
