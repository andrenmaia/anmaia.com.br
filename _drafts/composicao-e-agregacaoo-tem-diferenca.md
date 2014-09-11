# Composi��o e agrega��o: Tem diferen�a?

A diferen�a entre composi��o e agrega��o tem rela��o com a exist�ncia dos objetos. Essa diferen�a n�o � tratada pelas linguagens de programa��o que seguem o paradigma orientado a objetos (pelo menos n�o as convencionais: java, c#, c++).

Toda vez que temos composi��o, significa que _a parte n�o existe sem o todo_. 
Toda vez que temos agrega��o, significa que _a parte pode ser compartilhada entre v�rios objetos_.

> Este artigo vem de uma pergunta no [pt.stackoverflow.com][2]. Al�m deste artigo, vale dar uma olhada nas outras respostas que surgiram por l�.

## Orienta��o a objetos ou UML?
O comportamento de agrega��o n�o pertence exclusivamente ao paradigma orientado a objetos. Temos o mesmo comportamento no paradigma imperativo. � o caso das estruturas em C. Veja um exemplo:

```c
struct Person
{
	int age;
	char *name;
	enum { male, female } sex;
};
```
_age, name e sex comp�em o tipo Person_ 

```c
struct bintree
{
	struct bintree *left, *right;
	// some data
};
```

_bintree � composta de left e right, que s�o bintree_. Composi��o reflexiva.

Veja o conceito de composi��o descrito em v�rias [linguagens que n�o s�o orientadas a objetos na wikipedia][3].

## Tipos de associa��o em UML
Quando falamos de _agrega��o_ e _composi��o_ estamos falando de casos especiais de tipos de associa��o entre classes. Existem, em UML, associa��es: simples, agrega��o, generaliza��o, depend�ncia, realiza��o (e devem existir outra que n�o me recordo agora).

O tipo de associa��o **agrega��o** pode ser classificada basicamente de duas forma: agrega��o de composi��o e agrega��o compartilhada (ou reflexiva).

### Agrega��es
Esse tipos de rela��o s�o chamados assim porque agregam valor para o objeto relacionado. Esse � um tipo especializado de associa��o que permite nos permite encara a rela��o entre os objetos como: Todo/Parte.

Todo/Parte significa que um dos lados da associa��o (um classe) � chamado de Todo e o outro lado � chamado de Parte, j� que a parte nos permite pensage que: A **Parte** est� contidade no **Todo**

### Composi��o (ou agrega��o de composi��o)
Toda vez que dizemos que a rela��o entre duas classe � de **composi��o** estamos dizendo que uma dessas classe (a Parte) est� contida na outra (o Todo) e a parte n�o vive/n�o existe sem o todo.

Sendo assim, toda vez que distruirmos o todo, a parte que � �nica e exclusiva do todo se vai junto. Por esse motivo que algum disse que: a parte _est� contida_ no todo. Quando se jogo o todo fora, a parte estava dentro e se vai junto.

### Agrega��o (ou agrega��o compartilhada)
Essa tamb�m � uma rela��o todo/parte, por�m, nesse caso dizemos que a parte � compartilhada por outros (por isso agrega��o compartilhada). Isso significa que a parte de um tipo `A` est� contida em um tipo `B`, quando esse tem rela��o de agrega��o entre eles, por�m, essa mesma parte `A` n�o existe somente para **compor** `B`, essa parte pode agregar outros tipos.

### Concluindo a ideia *<< **Trocar esse t�tulo**

*>> **Este trecho est� errado**
Estabelecemos o que s�o agrega��es de agrega��o compartilhada, agora que os nomes fazem sentido podemos exemplificar da seguinte forma:
<<*

* Composi��o (Agrega��o de composi��o)
>  � necess�rio que exista pelo menos um item em uma nota fiscal para que a nota fiscal exista.

_Logo_: NotaFiscal � composta de ItemNotaFiscal.

* Agrega��o (agrega��o compartilhada)
> Se eu tiver um sistema de cadastro de times, preciso cadastras v�rias pessoas para agregar os times, assim, cada pessoa pode agregar um time, nenhum time, ou v�rios times. A pessoa � independente do time, mas agrega valor a ele.

_Logo_: Time � agregado por Pessoa.



#### N�o h� diferen�a na implementa��o e sim no comportamento
Em ambos os tipo de rela��o n�o h� diferen�a no momento da implementa��o, veja um exemplo em C#:

**Composi��o**
```java
class NotaFiscal: IDisposable {
	IList<ItemNotaFiscal> Itens {get;set;}
}

class ItemNotaFiscal: IDisposable { ... }
```

**Agrega��o**

```java
class Time {
	IList<Pessoa> Integrantes {get;set;}
}

class Pessoa {}
```

Por�m, os comportamentos **sem�nticos** das associa��es devem estar presentes quanto a exist�ncia. Para composi��o, por exemplo, poder�amos for�ar que toda vez que uma nota seja criada, uma nova lista de ItemNotaFiscal deve ser criado. E toda vez que a nota fiscal for apagada, os itens devem ser destru�dos. 

```java
class NotaFiscal {
	IList<ItemNotaFiscal> Itens {get;set;}
	NotaFiscal(){
		// Cria lista nova
		Itens = new List<ItemNotaFiscal>();
	}
	
	void Dispose() {
		foreach(var item in Itens){
			item.Dispose();
		}
	}
}
```
Assim, os item ser�o destru�dos junto com a nota fiscal.

Para agrega��o n�o seria necess�rio esses tratamento de cria��o e destrui��o de objetos. J� que as parte podem ser compartilhadas.


### Agrega��es e multiplicidade s�o diferentes
Muitos acham que _toda vez que existir um agrega��o/composi��o teremos uma lista/array das partes_, por�m, o que nos diz se teremos mais de um tipo associado � a **multiplicidade** e n�o o tipo de associa��o. Independente do tipo de associa��o, a multiplicidade pode ser: 0,1,* ou n..m. Ent�o, toda vez que uma associa��o for 1 para 1, n�o temos listas em nenhum dos lados.

Veja um exemplo:

[![inserir a descri��o da imagem aqui][4]][4]

```java
class Carro
{
	Motor UnicoEExclusivoMoto {get;set;}
}

class Motor{}
```

Nesse caso, o `Motor` � �nico e exclusivo para o `Carro`. E toda vez que o `Carro` for destru�do, o `Motor` ser� destru�do tamb�m.

Esse engano ocorre porque, na maioria das vezes em que um objetos est� relacionado a uma cole��o de outros objetos, essa associa��o expresse um dos tipos de agrega��o. Associa��es de agrega��o onde n�o ocorrem cole��es s�o incomum por conta disso.

## Conclus�o
Agrega��o ou composi��o s�o tipo de rela��o entre dois objetos/tipos. Cada um dos tipos de associa��o est� relacionado ao comportamento entre os objetos e com a exist�ncia desses de acordo com o conceito todo/parte.

Agrega��es n�o nada haver com multiplicidade. Embora, na maioria das vezes, objetos associados a uma cole��o de outros objetos expressem, em suas rela��es, um comportamento de agrega��o.

  [1]: http://i.stack.imgur.com/ARGKS.png
  [2]: http://pt.stackoverflow.com/questions/25619/composi%C3%A7%C3%A3o-e-agrega%C3%A7%C3%A3o-quais-as-diferen%C3%A7as-e-como-usar/25628#25628
  [3]: http://en.wikipedia.org/wiki/Object_composition#Composite_types_in_C
  [4]: {{ site.url }}/assets/composicao_vs_multiplicidade.png