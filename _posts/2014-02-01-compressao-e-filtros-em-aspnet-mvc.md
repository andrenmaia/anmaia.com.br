---
layout: post
title: Compressão e filtros em ASP.NET MVC
comments: true
---


## Introdução

Web sites com um alto tráfego de informação ocasionam em grande utilização de largura de banda e tempo de _download_ alto. Para reduzir o tráfego de informação na rede e garantir melhor performance (tempo de resposta), em aplicações ASP.NET MVC, pode-se comprimir os dados antes que esses sejam entregues.

Existem algumas formas de comprimir os dados no servidor antes do envio ao cliente. Uma dessas formas é criar um filtro para controles, que comprima os dados ao executar uma ação.

A seguir, será apresentado como implementar o filtro para compressão do _response_ a cada vez que uma ação do controle for invocada.

> **Código:** se você prefere aprender pelo código, veja o [repositório no github](https://github.com/andrenmaia/MVCCompressResponseFilter) com um exemplo do filtro implementado.

## O filtro de compressão
O objetivo é diminuir o tamanho das informações que serão entregues a cada cliente. Para isso, serão utilizados os filtro em ASP.NET MVC (para saber mais sobre filtro veja [Filtering in ASP.NET MVC](http://msdn.microsoft.com/en-us/library/gg416513(VS.98).aspx), e para um exemplo prático veja [ASP.NET MVC 4 Custom Action Filters](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-custom-action-filters))

> **Filtros:** A implementação de filtros para aplicações ASP.NET MVC é simples e prática, permitindo grande flexibilidade na manipulação de requisições (Requests) e respostas (Response).

Em ASP.NET MVC, existem alguns tipos de filtros. Filtros para autorização, exceção, ações, resultados. Como o objetivo é comprimir as respostas para cada cliente, foi utilizado o filtro de ações (ActionFilterAttribute).

Uma das formas de utilização dos filtros é através de _anotações customizadas_ em c# ([Creating Custom Attributes in C#](http://msdn.microsoft.com/en-us/library/sw480ze8.aspx)), então, para a implementação de um novo filtro de ação, basta herdar a classe base `ActionFilterAttribute`. Veja a implementação do filtro `Compress` a seguir:

```java
public class CompressAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext filterContext)
    {
        // O código do filtro vai aqui.
    }
}
```
> **Importante:** O filtro que utilizamos foi utilizado é o filtro de ação do _namespace_ `System.Web.Mvc` e não o filtro do _namespace_ `System.Web.Http.Filters`.

A classe acima implementa um filtro de ação com o método `OnActionExecuting(ActionExecutingContext)` sobrescrito, para capturar o momento em que a ação está sendo executada em um controle.

### OnActionExecuting
O método `OnActionExecuting` é chamado pelo framework ASP.NET MVC antes da ação em um controle terminar. Existem outros momentos que podem ser capturados pelo filtro de ação. Esses momentos são: `OnActionExecuted`, `OnResultExecuted` e `OnResultExecuting`, além do próprio `OnActionExecuting` para mais detalhes veja [Filtering in ASP.NET MVC](http://msdn.microsoft.com/en-us/library/gg416513(VS.98).aspx).

## Comprimir para quem?
Antes de comprimir os dados que serão entregues ao cliente é necessário verifica se o cliente aceita receber os dados comprimidos, e qual tipo de compressão é aceita.

O protocolo HTTP 1.1 envia, em cada requisição, informações sobre qual tipo de compressão é aceita como _response_. Veja um exemplo de requisição HTTP:

```http
    GET / HTTP/1.1
    Host: localhost:17711
    User-Agent: ...
    Accept-Encoding: gzip,deflate,sdch
```
Então, para saber qual tipo de compressão é aceita pelo cliente, devemos verifica se o parâmetro `Accept-Encoding` foi enviado, e para saber qual o tipo de compressão pode ser utilizado, deve-se ver quais os tipos informados por esse parâmetro (`Accept-Encoding`). Nesse caso são aceitos os tipos `gzip`, `deflate` e `sdch`.

O código a seguir implementa o filtro com a compressão do _response_:

```java
public override void OnActionExecuting(ActionExecutingContext filterContext)
{
    // Obtém o request do cliente e o Accept-Encoding, para verifica se
    // o cliente aceita compressão.
    HttpRequestBase request = filterContext.HttpContext.Request;
    string acceptEncoding = request.Headers["Accept-Encoding"];

    // Caso o cliente aceite compressão:
    // - verifica qual tipo de compressão;
    // - comprime o response.
    if (!String.IsNullOrEmpty(acceptEncoding))
    {
        acceptEncoding = acceptEncoding.ToUpperInvariant();
        HttpResponseBase response = filterContext.HttpContext.Response;

        // Comprime o response utilizando os algoritmos de compressão
        // comuns do .NET (caso o cliente aceite esses tipos de compressão).
        if (acceptEncoding.Contains("GZIP"))
        {
            response.AppendHeader("Content-encoding", "gzip");
            response.Filter = new GZipStream(response.Filter, CompressionMode.Compress);
        }
        else if (acceptEncoding.Contains("DEFLATE"))
        {
            response.AppendHeader("Content-encoding", "deflate");
            response.Filter = new DeflateStream(response.Filter, CompressionMode.Compress);
        }
    }
}
```
O código acima mostra a verificação do parâmetro enviado no _request_ e qual o tipo de compressão é aceita pelo cliente e, por fim, comprime os dados.

### Controle + Filtro: ligando tudo
Agora vem a parte mais fácil e simples. Após o filtro implementado, basta aplicá-lo no controle que deve comprimir o _response_, veja o exemplo a seguir:

```java
[Compress]
public class HomeController: Controller
{
    // As suas ações devem estar aqui...
}
```

> **Lembre-se:** o filtro herda a classe `ActionFilterAttribute`. Isso significa que **para cada ação executada, o filtro será executado**. No exemplo acima, qualquer ação executada passa pelo filtro de compressão. Isso significa que todas as ações executadas nesse controle (`HomeController`), o _response_ será comprimido.

É possível implementar o filtro à apenas algumas ações específicas, veja o exemplo:

```java
public class HomeController : Controller
{
    [Compress]
    public ActionResult Compressed()
    {
        return View();
    }

    public ActionResult NotCompressed()
    {
        return View();
    }
}
```
Nesse exemplo, o resultado é comprimido apenas para o ação `Compressed()`. A ação `NotCompressed()` não comprimirá a resposta, ou seja, o método `OnActionExecuting` do filtro não será executado.

## Conclusão
Este artigo demonstra um exemplo de como implementar um filtro de ação em ASP.NET MVC para comprimir as requisições feitas pelos clientes ao servidor. Além da implementação do filtro, esse artigo apresenta também, como aplicá-lo a todas as ações de um controle e a ações específicas de um controle.

## Referências

* Um exemplo dessa implementação está disponível no [github](https://github.com/andrenmaia/MVCCompressResponseFilter).
* [Filtering in ASP.NET MVC](http://msdn.microsoft.com/en-us/library/gg416513(VS.98).aspx)
* [ASP.NET MVC 4 Custom Action Filters](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-custom-action-filters)
* [Creating Custom Attributes in C#](http://msdn.microsoft.com/en-us/library/sw480ze8.aspx)
* [GZip](http://msdn.microsoft.com/en-us/library/system.io.compression.gzipstream(v=vs.110).aspx)
* [Deflate](http://msdn.microsoft.com/en-us/library/system.io.compression.deflatestream(v=vs.110).aspx)
