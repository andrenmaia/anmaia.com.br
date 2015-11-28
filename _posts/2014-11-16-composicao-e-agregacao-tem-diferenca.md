---
layout: post
title: Composição e agregação têm diferença?
comments: true
redirect_from:
  - /composicao-e-agregacao-tem-diferenca
---

A diferença entre composição e agregação tem relação com a existência dos objetos. Essa diferença não é tratada pelas linguagens de programação que seguem o paradigma orientado a objetos (pelo menos não as convencionais: java, c#, c++).

Toda vez que temos composição, significa que _a parte não existe sem o todo_. 

Toda vez que temos agregação, significa que _a parte pode ser compartilhada entre vários objetos_.

> Este artigo vem de uma pergunta no [pt.stackoverflow.com][2]. Além deste artigo, vale dar uma olhada nas outras respostas que surgiram por lá.


## Orientação a objetos ou UML?

O comportamento de agregação não pertence exclusivamente ao paradigma orientado a objetos. Temos o mesmo comportamento no paradigma imperativo. É o caso das estruturas em C. Veja um exemplo:

```c
struct Person
{
	int age;
	char *name;
	enum { male, female } sex;
};
```
_age, name e sex compõem o tipo Person_ 

```c
struct bintree
{
	struct bintree *left, *right;
	// some data
};
```

_bintree é composta de left e right, que são bintree_. Composição reflexiva.

Veja o conceito de composição descrito em várias [linguagens que não são orientadas a objetos na wikipedia][3].


## Tipos de associação em UML

Quando falamos de _agregação_ e _composição_ estamos falando de casos especiais de tipos de associação entre classes. Existem, em UML, associações: simples, agregação, generalização, dependência, realização (e devem existir outras que não me recordo agora).

O tipo de associação **agregação** pode ser classificada basicamente de duas forma: agregação de composição e agregação compartilhada (ou reflexiva).


### Agregações
Esse tipos de relação são chamados assim porque agregam valor para o objeto relacionado. Esse é um tipo especializado de associação que nos permite encarar a relação entre os objetos como: Todo/Parte.

Todo/Parte significa que um dos lados da associação (um classe) é chamado de **Todo** e o outro lado é chamado de **Parte**, já que a parte nos permite pensar que: A **Parte** está contida no **Todo**.


### Composição (ou agregação de composição)

Toda vez que dizemos que a relação entre duas classe é de **composição** estamos dizendo que uma dessas classe (a Parte) está contida na outra (o Todo) e a parte não vive/não existe sem o todo.

Sendo assim, toda vez que destruirmos o todo, a parte que é única e exclusiva do todo se vai junto. Por esse motivo que algum dizem que: a parte _está contida_ no todo. Quando se jogo o todo fora, a parte estava dentro e se vai junto.


### Agregação (ou agregação compartilhada)

Essa também é uma relação todo/parte, porém, nesse caso dizemos que a parte é compartilhada por outros (por isso agregação compartilhada). Isso significa que a parte de um tipo `A` está contida em um tipo `B`, quando esse tem relação de agregação entre eles, porém, essa mesma parte `A` não existe somente para **compor** `B`, essa parte pode agregar outros tipos.

### Resumindo

Estabelecemos o que são agregações de composição e compartilhada, agora que os nomes fazem sentido podemos exemplificar da seguinte forma:

* Composição (Agregação de composição)

>  É necessário que exista pelo menos um item em uma nota fiscal para que a nota fiscal exista.

_Logo_: NotaFiscal é composta de ItemNotaFiscal.

* Agregação (agregação compartilhada)

> Se eu tiver um sistema de cadastro de times, preciso cadastras várias pessoas para agregar os times, assim, cada pessoa pode agregar um time, nenhum time, ou vários times. A pessoa é independente do time, mas agrega valor a ele.

_Logo_: Time é agregado por Pessoa.

**Não há diferença na implementação e sim no comportamento**

Em ambos os tipo de relação não há diferença no momento da implementação, veja um exemplo em C#:

**Composição**
```java
class NotaFiscal: IDisposable {
	IList<ItemNotaFiscal> Itens {get;set;}
}

class ItemNotaFiscal: IDisposable { ... }
```

**Agregação**

```java
class Time {
	IList<Pessoa> Integrantes {get;set;}
}

class Pessoa {}
```

Porém, os comportamentos **semânticos** das associações devem estar presentes quanto a existência. Para composição, por exemplo, poderíamos forçar que toda vez que uma nota seja criada, uma nova lista de ItemNotaFiscal deve ser criado. E toda vez que a nota fiscal for apagada, os itens devem ser destruídos. 

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
Assim, os item serão destruídos junto com a nota fiscal.

Para agregação não seria necessário esses tratamento de criação e destruição de objetos. Já que as parte podem ser compartilhadas.


**Agregações e multiplicidade são diferentes**

Muitos acham que _toda vez que existir um agregação/composição teremos uma lista/array das partes_, porém, o que nos diz se teremos mais de um tipo associado é a **multiplicidade** e não o tipo de associação. Independente do tipo de associação, a multiplicidade pode ser: 0,1,* ou n..m. Então, toda vez que uma associação for 1 para 1, não temos listas em nenhum dos lados.

Veja um exemplo:

![Composição não é multiplicidade][4]

```java
class Carro
{
	Motor UnicoEExclusivoMotor {get;set;}
}

class Motor{}
```

Nesse caso, o `Motor` é único e exclusivo para o `Carro`, e toda vez que o `Carro` for destruído, o `Motor` será destruído também.

Esse engano ocorre porque na maioria das vezes em que um objetos está relacionado a uma coleção de outros objetos, essa associação expressa um dos tipos de agregação. Associações de agregação onde não ocorrem coleções são incomuns por conta disso.

## Conclusão

Agregação ou composição são tipo de relação entre dois objetos/tipos. Cada um dos tipos de associação está relacionado ao comportamento entre os objetos e com a existência desses de acordo com o conceito todo/parte.

Agregações não tem nada haver com multiplicidade. Embora, na maioria das vezes, objetos associados a uma coleção de outros objetos expressem, em suas relações, um comportamento de agregação.

  [1]: http://i.stack.imgur.com/ARGKS.png
  [2]: http://pt.stackoverflow.com/questions/25619/composi%C3%A7%C3%A3o-e-agrega%C3%A7%C3%A3o-quais-as-diferen%C3%A7as-e-como-usar/25628#25628
  [3]: http://en.wikipedia.org/wiki/Object_composition#Composite_types_in_C
  [4]: {{ site.url }}/assets/composicao_vs_multiplicidade.png
