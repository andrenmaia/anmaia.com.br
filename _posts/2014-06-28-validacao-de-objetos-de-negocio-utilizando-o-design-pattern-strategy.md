---
layout: post
title: Validação de objetos de negócio utilizando o design pattern Strategy
comments: true
redirect_from:
  - /validacao-de-objetos-de-negocio-utilizando-o-design-pattern-strategy
---

## Introdução

Sempre que criada uma aplicação (principalmente) do tipo [_line of business_][1] é necessária verificação das regras de negócio. Porém, como essas verificações/validações podem ser feitas de modo estruturado, seguindo padrões de desenvolvimento de projetos?

Este artigo apresenta um exemplo simples e flexível baseado no [_design patterns Strategy_][4] para validações de objetos de negócio em Java.

> O código utilizado nesse artigo está disponível no [github][5].


## Modelo de domínio e código

É comum a utilização de diagramas de classe UML para exibição de modelos de domínio. A prática dos modelos de domínio são bem comum no desenvolvimento de _software_, principalmente após introduzido os conceitos de [_Domain-Driven Design_ por Eric Evan][2].

A seguir, é apresentado o modelo de domínio utilizado para o exemplo de validação de regras de negócio.

![Modelo de domínio de exemplo]({{ site.url }}/assets/pessoa_domain_model.jpg)

Para o modelo de domínio apresentado, sua representação em código Java a seguir.

```java
class Pessoa{
	String name;

	public Pessoa(String name){
		this.name = name;
	}

	public String getName(){
		return name;
	}

}

class PessoaFisica extends Pessoa {
	public PessoaFisica(String name){
		super(name);
	}
}
class PessoaJuridica extends Pessoa {
	public PessoaJuridica(String name){
		super(name);
	}
}
```

## O problema
Dado um modelo de domíno como o anterior, como é posível criar algoritmos de validação para cada entidade sem que essas validações fiquem espalhados em suas respectivas entidade?

O mais importante: como implementar essas validações sem que exista um padrão pré-estabelecido?

Se criarmos vários `if`s e os espalharmos por todas as entidades do modelo de domínio, será muito difícil saber se todas as validações necessárias foram contempladas e, além disso, será quase impossível encontrar um bug quando o projeto estiver muito extenso.


## A solução

Como solução para o problema proposto utilizaremos o _design pattern Strategy_.

O _Strategy_ nos permite selecionar algoritmos diferentes, em tempo de execução, de acordo com o tipo da entidade que precisamos validar.

A seguir, os vários algoritmos de validação para cada entidade.

```java
interface Validator<T> {
	void validate(T o) throws Exception;
}

class PessoaValidator implements Validator<Pessoa>{
	@Override
	public void validate(Pessoa o) throws Exception {
		System.out.println(o.getName() + '-' + getClass().getSimpleName());
	}
}

class PessoaFisicaValidator implements Validator<PessoaFisica>{
	@Override
	public void validate(PessoaFisica o) throws Exception {
		System.out.println(o.getName() + '-' + getClass().getSimpleName());
	}
}

class PessoaJuridicaValidator implements Validator<PessoaJuridica>{
	@Override
	public void validate(PessoaJuridica o) throws Exception {
		System.out.println(o.getName() + '-' + getClass().getSimpleName());
	}
}
```

No código de validação, utilizamos o recurso de [Generics][3] em Java, para diminuir a quantidade de código escrito para cada `Validator`.

Para cada tipo de `Pessoa` temos uma implementação específica de `Validator`. De acordo com esse código, podemos implementar facilmente várias versões diferentes de `Validator` para cada `Pessoa`. Por exemplo:

```java
class PessoaJuridicaNovaImplementacaoValidator implements Validator<PessoaJuridica>{
	@Override
	public void validate(PessoaJuridica o) throws Exception {
		if (o.getName().size() < 10)
			throw new Exception("O nome para pessoa jurídica deve conter mais que 10 caracteres");
	}
}
```

A seguir a implementação do `ValidatorStrategy`, que nos auxilia a selecionar o `Validator` correto para cada tipo de entidade `Pessoa`.

```java
class ValidatorStrategy<T>{
	Map<Class, Validator> map = null;

	public ValidatorStrategy(){

		// Cadastro dos vários tipos de validadores
		// para cara tipo de entidade.
		map = new HashMap<Class, Validator>();
		map.put(Pessoa.class, new PessoaValidator());
		map.put(PessoaFisica.class, new PessoaFisicaValidator());
		map.put(PessoaJuridica.class, new PessoaJuridicaValidator());
	}

	public Validator get(Class objectType){
		return map.get(objectType);
	}
}
```

Nesse caso, a chave para escolher qual é o algoritmo de validação correto é o tipo da entidade que precisa ser validada. Assim, é fácil obter um novo `Validator` sempre que necessário validar uma entidade.

Veja um exemplo de utilização do código proposto:


```java
public class objectValidation {

	public static void main(String[] args) throws Exception {
		List<Pessoa> pessoas = new ArrayList<Pessoa>();
		pessoas.add(new Pessoa("Uma pessoa simples"));
		pessoas.add(new PessoaFisica("João"));
		pessoas.add(new PessoaFisica("José"));
		pessoas.add(new PessoaJuridica("Maria"));

		ValidatorStrategy<Pessoa> validator = new ValidatorStrategy<Pessoa>();

		for (Pessoa p: pessoas){
			validator
				.get(p.getClass())
				.validate(p);
		}
	}
}

/*  Saídas

	Uma pessoa simples-PessoaValidator
	João-PessoaFisicaValidator
	José-PessoaFisicaValidator
	Maria-PessoaJuridicaValidator

*/
```

## Conclusões

Combinando o paradigma orientado a objetos e _design patterns_ é fácil obter um código simples, limpo, flexível e o mais importante no contexto de _software_ LOB: um código de fácil manutenção.

Essa estratégia permite fácil manutenção e extensão dos códigos de validação, sem que as informações de validação fiquem dentro da própria entidade. Além da fácil manutenção, esse código é facilmente desacoplável e pode ser injetado de várias formas diferentes.

O código de exemplo pode ser encontrado no [meu github][5].



 [1]: http://en.wikipedia.org/wiki/Line_of_Business
 [2]: http://en.wikipedia.org/wiki/Domain-driven_design
 [3]: http://docs.oracle.com/javase/tutorial/java/generics/
 [4]: http://en.wikipedia.org/wiki/Strategy_pattern
 [5]: https://github.com/andrenmaia/Java-samples/blob/master/src/objectValidation.java
