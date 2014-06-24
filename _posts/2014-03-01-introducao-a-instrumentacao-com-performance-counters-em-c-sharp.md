---
layout: post
title: Introdução a instrumentação com performance counters em C#
comments: true
---

## Introdução
Entender o programa durante o desenvolvimento é essencial para que todas as funcionalidades ocorram conforme o esperado, porém, no período do desenvolvimento a aplicação está em um ambiente controlado, a máquina do desenvolvedor. Mas quando esse ambiente não é controlado, e ocorrem gargalos durante a execução da aplicação, o que dever ser feito para corrigir o sistema?

Para identificar esses tipos de problemas, que ocorrem em tempo real, são utilizadas técnicas de instrumentação de software. Em .NET existem algumas formas de instrumentar o software em execução, uma delas é através dos contadores de performance (_performance counter_).

> O código utilizado nesse artigo está disponível no [github][6].

## Performance Counter

Os _Performance Counters_ (contadores de performance), são utilizados para medir, em tempo real, o desempenho do sistema operacional ou de uma aplicação específica, serviço, ou um _driver_. Os dados coletados auxiliam na detecção de gargalos do que está sendo monitorado.

Em todas as vesões do window, estão presentes um série de contadores de performance do sistema operacional. Contadores para CPU, discos, memória, drivers de impressora, IIS, entre outros. Todos esses contadores coletam dados em tempo real sobre informações do sistema operacional e as deixam a disposição do usuário para análise e entendimento do desempenho da máquina.

Para visualizar esses contadores de performance do seu sistema operacional, há duas formas comuns:

1. Abra o Monitor de Desempenho (perfmon.exe): na tela do monitor de desempenho é possível adicionar vários contadores para análise em tempo real, através de um gráfico de linha, barra/histograma ou até através de um relatório em texto.
[![Figura 1: tela do monitor de desempenho][1]][1]
2. No Visual Studio, existe um aba chamada _Server Explorer_ : selecione Servers Explorer &gt; [Nome do computador] &gt; Performance Counters. Nesse nível da árvore de contadores você consegue visualizar todas as categorias, contadores e instâncias dos contadores.
[![Figura 2: monitores e desempenho no Visual Studio][2]][2]

## Tipos de contadores
Os tipos de contadores que devem ser escolhidos de acordo com o objetivo da coleta. Os contadores são classificados da seguinte forma:

* Média: quando se deseja medir um valor ao longo do tempo e exibir a média das duas últimas medidas. Esse acompanha um `base counter` que dispobiliza a número de amostras envolvidas na medição;
* Diferença/Variação: subtração da última medição pela medição anterios (também chamado de _Delta_). Se a subtração for positiva, mostra a diferença, caso contrário, mostra zero;
* Instantânea: mostra a medição mais recente;
* Porcentagem: mostra o valor calculado como uma porcentagem;
* Taxa: contadores que rastreiam o número de itens ou operações por segundo.

Para um lista completa dos tipo de contadores de performance, veja a definição do enumerador de [tipos de contadores de performance][3] (System.Diagnostics.PerformanceCounterType). Todos os tipos que terminarem como "Base" são utilizados para suporte de outros contradores que não terminam com a palavra "Base", por exemplo `AverageTimer32` e `AverageBase`.


## Organização
Os contadores de performance são organizado da seguinte forma: ...Performance Counters &gt; Categoria &gt; Contador &gt; Instância. Dessa forma, caso você deseje visualizar o porcentual de espaço livre do disco `C:` de uma máquina, você deve selecionar:

1. Categoria: disco local (ou LocalDisk, caso sua máquina esteja utilizando o windows em inglês)
2. Contador: % de espaço livre (ou % Free Space)
3. Instância: `C:`

Se fosse o `% de espaço livre do disco D:` a instância seria `D:`.

## Criando contadores utilizando C#

Para a certificação [70-483: Programming in C#][4] é necessário saber criar contadores de performance e armazená-los em categorias utilizando C#. A seguir, mostrarei um trecho de código para criar um contador de performance para cada tipo de contador disponível (são 5 ao todo), junto a uma categoria para agrupá-los.

> **Importante:** para criar contadores em _runtime_, o usuário, que está executando a aplicação, deve ter permissão de administrador da máquina. Caso o usuário não seja administrador, o erro recebido será `SecurityException: Acesso ao Registro solicitado não é permitido`. Para evitar esse problema em _debug_, execute o Visual Studio como Administrador.

```java
namespace Counters
{
    class PerformanceCounterCategoriCreator
    {
       public const string NOME_CATEGORIA = "CountersCategory";

        internal void Create()
        {
            PerformanceCounterCategory.Delete(NOME_CATEGORIA);

            // 1. Verifica se a categoria já existe
            if (!PerformanceCounterCategory.Exists(NOME_CATEGORIA))
            {

                // 2. Cria a coleção de contadores e a preenche.
                var counterCollection = new CounterCreationDataCollection();

                // Contador - Média
                var media = GetNewCounterCreationData("Tempo médio por operação", PerformanceCounterType.AverageCount64);
                counterCollection.Add(media);

                var mediaBase = GetNewCounterCreationData("Tempo médio por operação BASE", PerformanceCounterType.AverageBase);
                counterCollection.Add(mediaBase);

                // Contador - Diferença/Variação
                var diferenca = GetNewCounterCreationData("Diferença/Variação", PerformanceCounterType.CounterDelta32);
                counterCollection.Add(diferenca);

                // Contador - Instantânea
                var instantanea = GetNewCounterCreationData("Número de operações executadas", PerformanceCounterType.NumberOfItems32);
                counterCollection.Add(instantanea);

                // Contador - Porcentagem
                var porcentagem = GetNewCounterCreationData("Porcentagem", PerformanceCounterType.CounterTimer);
                counterCollection.Add(porcentagem);

                // Contador - Porcentagem
                var taxa = GetNewCounterCreationData("Número de operações por seg", PerformanceCounterType.RateOfCountsPerSecond32);
                counterCollection.Add(taxa);

                // 3. Cria a categoria do performance counter.
                PerformanceCounterCategory.Create(
                    NOME_CATEGORIA,
                    "categoria para exemplo de contadores",
                    PerformanceCounterCategoryType.SingleInstance,
                    counterCollection
                );
            }
        }

        private CounterCreationData GetNewCounterCreationData(string name, PerformanceCounterType performanceCounterType)
        {
            var counter = new CounterCreationData();
            counter.CounterName = name;
            counter.CounterHelp = "Contador  - " + name;
            counter.CounterType = performanceCounterType;

            return counter;
        }
    }
}
```

O código acima, exibe como criar uma nova categoria e um contador de cada tipo disponível no performance counter. Como foi dito anteriormente, a estrutura de armazenamos é Categoria &gt; Contadores &gt; Instância. No código anterior, foram criados Categoria e Contadores.

Os passos para criação são sempre os mesmo:
1. Deve-se verificar se a categoria já existe;
2. Cria-se a coleção de contadores que deseja armazenar na categoria criada;
3. Cria-se a nova categoria com a coleção de contadores.

Após criada a categoria com o código acima, a lista de performance counters deve conter a nova categoria `CountersCategory`.

[![Figura 3: Server Explorer - nova categoria criada.][5]][5]

## Atualizando valor dos contadores

Para criar instâncias de contadores de performance utilizaremos a classe `PerformanceCounter`, informando o nome exato do contador de performance que criamos anteriormente. Para atualização podemos utilizar os métodos:

* Increment() - incrementa o valor do contador em 1;
* IncrementBy(inc) - incrementa o valor do contador para o valor informado como parâmetro (inc);
* Decrement() - decrementa o valor do contador em 1;
* DecrementBy(inc) - decrementa o valor do contador para o valor informado como parâmetro;
* RawValue - aplicar o valor exato ao contador (este é uma propriedade);

Podemos atualizar os valores dos contadores conforme o trecho de código a seguir.

```java
internal void Start()
{
    // Cria as instâncias dos contadores para atualizar seus valores.
    var instantanea = GetPerfCounter("Número de operações executadas");
    // ... outros contadores

    // Task para atualização constante dos contadores
    Task.Factory.StartNew(() =>
    {
        while (true)
        {
            var tick = GetTickForPerfCounter();

            // Se o tick for par, incrementa, caso contrário, decrementa
            if (tick % 2 == 0)
            {
                instantanea.Increment();
                // ... outros incrementos
            }
            else
            {
                instantanea.Decrement();
                // ... outros decrementos
            }

            Console.WriteLine("Atualizou o valor dos contadores.");
            System.Threading.Thread.Sleep(TimeSpan.FromSeconds(3));
        }
    });
}
```
> Alguns trechos do código acima foram suprimidos para melhor visualização. Veja o código completo no [github][6]

## Conclusão

Contador de performance é uma ferramenta importante para instrumentação de uma aplicação e idenficação de gargalos. A criação em _runtime_ é simples e rápida, beneficiando o desenvolvedor durante o desenvolvimento e após a implantação do projeto.

Para um exemplo completo, veja o [código no github][7].

## Referências
  * [Sobre performance counter][8]
  * [Tipos de contadores de performance][9]
  * [70-483: Programming in C#][10]
  * [Codigo completo no github][11]
  * [An Introduction To Performance Counters][12]


  [1]: http://anmaia.files.wordpress.com/2014/02/figura-1-tela-do-monitorador-de-desempenho.png
  [2]: http://anmaia.files.wordpress.com/2014/02/figura-2-monitorador-de-desempenho-no-visual-studio.jpg
  [3]: http://msdn.microsoft.com/en-us/library/system.diagnostics.performancecountertype(v=vs.110).aspx
  [4]: http://www.microsoft.com/learning/en-us/exam-70-483.aspx
  [5]: http://anmaia.files.wordpress.com/2014/02/figura-3-server-explorer-performance-counter-apc3b3s-criada-a-categoria.png
  [6]: https://github.com/andrenmaia/PerformanceCounterSamples
  [7]: https://github.com/andrenmaia/PerformanceCounterSamples
  [8]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa371643(v=vs.85).aspx
  [9]: http://msdn.microsoft.com/en-us/library/system.diagnostics.performancecountertype(v=vs.110).aspx
  [10]: http://www.microsoft.com/learning/en-us/exam-70-483.aspx
  [11]: https://github.com/andrenmaia/PerformanceCounterSamples
  [12]: http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters
