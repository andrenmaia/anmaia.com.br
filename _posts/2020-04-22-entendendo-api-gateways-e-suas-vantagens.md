---
layout: post
title: Entendendo API Gateways e suas vantagens
comments: true
redirect_from:
  - /entendendo-api-gateway/
excerpt: Neste artigo mostraremos uma visão geral do que é um API Gateway e quais as suas principais vantagens durante sua utilização.
---

Um API Gateway é **uma aplicação** que fica em frente de um conjunto de APIs e
age como **um ponto único de entrada** para um grupo definido de
microsserviços/serviços, criando um único canal de mapeamento de tráfego externo
para tráfego interno. 

Além de ser um serviço em frente a um conjunto de API, também podemos dizer que
uma API Gateway é um **padrão de comunicação externa para microsserviços**, com
o objetivo de: 

* receber todos os request de um tipo específico de __client__ (mobile,
web app, consumo de API);
* **determinar quais serviços são necessários**; e 
* os **combinar em uma experiência síncrona** para o usuário. 

O ponto único de entrada para serviços externos melhora a abstração e evita o
acoplamento com a infraestrutura interna, dando mais liberdade na evolução e
alterações da infraestrutura e particionamento de serviços internos.

Veja dois diagramas que representam duas arquiteturas, uma utilizando API
Gateway e outra não.

#### Sem API Gateway
{% include image.html url="/assets/api-gateway-sem.png" description="Fig. 1 - Sem API Gateway [1]" %}

#### Com API Gateway
{% include image.html url="/assets/api-gateway-com.png" description="Fig. 2 - Com API Gateway [1]" %}

Também existe a variação de utilização do padrão API Gateway com o objetivo de
servir requisições específicas para cada tipo de tecnologia de _front-end_. Essa
variação de padrão se chama é comumente chamada de _Back-end for Front-end_
(BFF).

{% include image.html url="/assets/api-gateway-bff.png" description="Fig. 3 - Back-end for Front-end [1]" %}

## Principais funcionalidades
São inúmeras as funcionalidades e utilização de um API Gateway. As suas
principais funcionalidades são:

* Principais funcionalidades
* Autenticação e Autorização;
* Aplicação de políticas de segurança (header, CSRS, outros);
* Balanceamento de carga;
* Gerenciamento de cache;
* Resolução de dependências (integração com service discovery);
* Gerenciamento de SLA;
* Políticas de retentativas, circuit breaker e QoS;
* Rate limiting e throttling;
* Log, trace, correlação;
* Transformações de Headers, query string;
* IP Whitelist;
* SSL/TLS externo, clear text interno;

## Quando API Gateway é utilizado? 
Quando se deseja criar um único ponto de entrada para _clients_ externos (ou
terceiros) para facilitar principalmente a comunicação, segurança e
monitoração.

Quando desenvolvemos uma solução utilizando a arquitetura de microsserviços,
teremos muitos pontos de entradas para aplicações de front-end, como Android e
IPhone (mobile), Web Apps (SPA ou WebForms), API para acesso de terceiros. Para
facilitar a comunicação criamos um API Gateway com um único ponto de entrada
para todos os _clients_, dessa forma tudo o que é transversal pode ser controlado
e implementado pelo API Gateway.

Quando utilizada a arquitetura de microsserviços, uma das premissas é smart
endpoint and dumb pipes, então não devemos colocar regras de negócios em API
Gateways, mas devemos colocar funcionalidades transversais como autenticação,
rate limiting, conversão de protocolos de comunicação (fora RESTFul dentro
gRPC), localização dinâmica de serviços (host+porta).

## Quais os cenários mais comuns de utilização de API Gateway?
Quanto utilizamos arquitetura de microsserviços e queremos facilitar a
comunicação a grande quantidade de microsserviços de forma uniforme e através de
um único ponto de entrada.

Geralmente quando implementamos um API Gateway perseguimos tecnologias
non-blocking IO para aumentar a vazão das requisições com menos recursos
computacionais. Implementações comuns JVM são Netty, Spring Reactor, Spring
Cloud Gateway. NodeJS é outra opção também muito utilizada.

## Por que utilizamos em microservices?
Porque microsserviços são granulares, então ao invés de fazer um _client_ externos
se comunicar com vários microsserviços criamos um único ponto de entrada para
_clients_ externos (API Gateway — um proxy reverso) para diminuir o acoplamento
aos vários microsserviços, abstrair a comunicação e facilitar a implementação de
requisito transversais, como AuthN e AuthZ para _clients_ externos (que é
diferente de comunicação interna).

## Quais os benefícios de utilizar API Gateway?
Os benefícios são vários. Veja alguns:

* Abstração de protocolos. Essa abstração facilita a comunicação externa web
  friendly enquanto internamente podemos utilizar protocolos mais eficientes.
  Externamente RESTFul API, internamente gRPC, Thrifit, REST, ou qualquer outro
  protocolo de comunicação;
* Baixo acoplamento a serviços internos. Evita o acoplamento através do API
  Gateway (o intermediário) e facilita o redesenho e particionamento de serviços
  internos sem afetar os _clients_ externos; 
* Redução de Round-trip time (RTT). O uso do API Composition pattern.
  Reduzir várias chamadas em uma única chamada para _clients_ externos. Exemplo:
{% include image.html url="/assets/api-gateway-reduce-rtt.png" description="Fig. 4 - Redução de RTT" %}
> Este é um padrão de Data Management no catálogo de padrões de microsserviços.
Ele faz parte dessa categoria porque geralmente termos que implementar esse
padrão quando utilizamos outros padrões como Database per Service. Isso
não escala bem quando os conjuntos de dados são grandes. Nesse caso é melhor
implementar CQRS.

* Dados diferentes para _clients_ diferentes. Versões desktop geralmente são mais
  elaboradas do que as versões mobile.
* Desempenho de rede. Dados diferentes para tipos de redes diferentes, redes
  telefônicas (3G, 4G) tem um desempenho menor do que redes cabeadas. O mesmo
  para WAN e LAN, e assim por diante.
* Localização de instâncias de serviços. Teremos várias instâncias de
  microsserviços se movendo pelo cluster, o API Gateway abstrai o dinamismo de
  host+porta para cada serviço se comunicando diretamente com o Service
  Discovery ou tendo endereços fixos.
* As várias chamadas para vários serviços estão na mesma rede. Como as chamas
  estão na mesma rede o desempenho é melhor do que as várias chamadas externas
  de um _client_.

## Quais as desvantagens de utilizar API Gateway?
Algumas desvantagens são:

* Aumento de complexidade. Mais um serviço (parte móvel) para ser mantida em
  produção. Isso aumenta as chances de falha.
* Aumenta o tempo de resposta por causa da hop adicional na rede. Temos mais um
  intermediário na comunicação, o que faz aumentar mais um hop de rede. Porém,
  geralmente esse hop adicional é insignificante.
* Perigo de implementação de regras de negócio. Principalmente quando utilizando
  microsserviços, devemos nos manter no princípio smart endpoint and dumb pipe.
  Pelo fato de API Gateways permitirem implementação composição de chamadas a
  serviços, é muito fácil adicionar regras no API Gateway. Então, é importante
  manter as rotas o mais simples possível e sem regras de negócio.


## Qual a diferença de API Gateway e outros?

### Zuul
O Zuul também é um API Gateway. É a implementação de API Gateway da Netflix. Na
primeira versão ele foi construído com servlet 2.5 usando uma blocking API,
nesta versão ele não dá suporte para long lived connections (como websockets).
Nas versões mais recentes ele corrigiu e evoluiu as limitações citadas. Porém, o
Spring Cloud Gateway foi construído antes da versão 2.x sair. O Spring Cloud
Gateway atende todos esses pontos, além de ser melhor integrado ao Spring
Framework. Porém, existiu um problema de performance em versões iniciais que
foram corrigidos na versão 2.x.


### Ribbon
Ribbon é um balanceador de carga _client_-side que lhe permite muito controle
sobre o comportamento de _clients_ HTTP e TCP. O Spring Feign usa o Ribbon.

### API Management
O API Management é uma solução pronta para publicar APIs para _clientes_ externos
e internos. Alguns exemplos são: Sensedia, Azure API Management, Apigee, e
outros.

### BFF
É uma variação do padrão API Gateway com o foco em implementar um gateway
específico para cada tipo de _client_ (mobile, webapp, app de terceiros).

### API Composition
O API Composition é um padrão de Data Management no catálogo de padrões de
microsserviços. Ele faz parte dessa categoria porque geralmente termos que
implementar esse padrão quando utilizamos outros padrões como Database per
Service.

### Service Mesh
É direcionado a comunicação interna entre serviços. O modo mais comum é pegar os
requisitos transversais (segurança, circuit breaker, service discovery) e
colocar isso em uma app usando o padrão Sidecar, no mesmo container da app
ou em um container próximo da app. Tudo o que passa pelo Sidecar é enviado para
um Control Plane para ajustes de configurações e observabilidade em tempo de
execução.

## Conclusão

O padrão API Gateway é principalmente utilizado para diminuir a complexidade e
evitar acoplamento em uma arquitetura que temos muitos serviços e APIs
disponíveis. Todo o trabalho de acesso a várias APIs, processo de segurança e
organização é feito através do API Gateway, facilitando a vida do que é exposto
para o mundo externo.

Embora o API Gateway seja mais um componente de software que pode aumentar a
fragilidade e reduzir a disponibilidade do sistema, sua implementação compensa
pelo fato de simplificar e canalizar uma porção de trabalhos que seriam
redundantes para todos os clientes que acessem os serviços internos.

## Referências

1. Microsoft. The API gateway pattern versus the Direct _client_-to-microservice communication. <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/direct-_client_-to-microservice-communication-versus-the-api-gateway-pattern>

