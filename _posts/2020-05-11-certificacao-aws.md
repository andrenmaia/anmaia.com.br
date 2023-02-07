---
layout: post
title: Certificação AWS
comments: true
redirect_from:
  - /certificacao-aws/
excerpt: Um guia prático sobre o que são e qual a ordem das certificações AWS obter
---

Já trabalho há vários anos com a AWS. Trabalhei bastante no passado, fiquei um
tempo sem trabalhar (uns 2 anos), e agora estou trabalhando novamente. Percebi
que muitas coisas mudaram nessa breve pausa, mas os fundamentos permanecem os
mesmos, e uma das formas mais sólidas de se aprender esses fundamentos é obtendo
uma certificação da AWS ou pelos menos seguindo o material sugerido para estudos
para obter essa certificação.

> **Certificação como guia**
>
> Na minha opinião, certificações não valem para mostrar que você sabe qualquer
> coisa, mas elas têm algo muito mais valioso: um guia completo e bem estruturado
> de como dominar alguma tecnologia. Então, sugiro obter a certificação como uma
> forma de guiar os estudos para uma determinada tecnologia, e não para mostrar que
> você sabe alguma coisa.

Uma coisa que percebi é que quando você não conhece os serviços e estruturas
básicas, você acaba gastando muito tempo para fazer qualquer coisa. Então a
minha sugestão para qualquer um que trabalhe com a AWS é: tire um tempo para
aprender pelo menos sobre permissionamento (IAM), rede (VPC), instâncias para
colocar sua aplicação em execução (EC2) e armazenamento de dados e arquivos
(S3). Eu sugiro os serviços IAM, VPC, EC2 e S3 porque quando você está começando
você precisará saber pelo menos:

1. Como os acessos na AWS funcionam para dar permissão entre os serviços
   utilizados e aplicações que você esta criando;
1. Porque as aplicações ainda não estão se comunicando da forma esperada após
   ajustar todos os permissionamentos no IAM; (É muito provável que você tenha
   um problema de rede.)
1. Como colocar qualquer aplicação para executar na AWS. Às vezes o seu problema
   pode ser onde sua aplicação está executando, que no fundo, independente do
   serviço que você esteja utilizando, será um EC2;
1. Como armazenar qualquer coisa de forma segura, que não seja em um banco de
   dado relacional, por exemplo: arquivos, html estático, imagens, arquivos para
   download.

Então, aprender o básico sobre AWS é fundamental para que você ganhe agilidade
no seu dia a dia e evite ficar empacado em pontos básicos ou pelo menos saiba o
que consultar para resolver qualquer problema com a AWS.

## As certificações AWS

{% include image.html url="/assets/post_img/aws-certifications.png" description="Fig. 1 - Certificações AWS [1]" %}

Existem três _horizontais_ que agrupam as certificações AWS: professional,
associate, foundational. Cada horizontal é sugerida pela AWS pelo seu tempo de
experiência com a nuvem.

- Professional: recomendado 2 anos de experiência;
- Associate: 1 ano de experiência
- Foundational: seis meses

Existe também uma divisão específica que é chamada de _Specialty_, para
segmentos como Machine Learning e outros, caso você esteja procurando se
especializar em alguns serviços e facilidades específicas da AWS.

Além das horizontais, também existem verticais de acordo com o foco aplicado às
tecnologias e serviços da AWS. Que são: Cloud Practitioner, Architect,
Operations, Developer. Porém, se você é um engenheiro de software, deverá
aprender todos os aspectos/verticais, porque você deverá entender exatamente
como sua aplicação está em execução em produção para conseguir sugerir melhorias
eficientes.

> Uma grande observação sobre a certificação da horizontal Foundational. Essa,
> como o seu nome sugere, vai explicar o básico sobre a nuvem, passando também por
> aspectos de precificação e outros que não diz respeito somente ao
> desenvolvimento de aplicações.

## Por onde começar?

Comece pelas certificações do nível Associate, essas são as bases para quem põe
a mão na massa. Siga a sequência:

1. Solutions Architect - Associate;
1. Developer - Associate;
1. SysOps Administrator - Associate;

A primeira vai justamente lhe mostrar todas as principais capacidades da AWS, e
focar justamente nos serviços base principais: IAM, VPC, S3, EC2. Siga para
_Developer - Associate_ que vai lhe mostrar os principais serviços utilizados
para rodar uma aplicação na AWS, como Elastic Beanstalk, SQS, SNS, DynamoDB.
Essa certificação tem muitas sobreposições com a certificação _Solutions
Architect_. Por último a _SysOps Administrator - Associate_, essa vai lhe
mostrar quais as preocupação que qualquer um tem que ter ao fazer o deploy de
uma aplicação na AWS, passando pelos processo de deployment, pipelines,
métricas e monitorações.

Isso deve ser o suficiente para você que trabalha no dia a dia com a AWS como
engenheiro de software. Caso o seu objetivo seja ir mais a fundo e conseguir
fazer bom uso de todos os serviços da AWS para criar soluções completas, então
entre no nível _Professional_.

No nível _Professional_, inicie pela _Solution Architecture - Professional_ que
vai cobrar tudo do nível _Associate_ só que em um nível mais profundo. Em
seguida passe para a _DevOps Engineer - Professional_. As duas provas do nível
_Professional_ tem muitas sobreposições, mas para quem é engenheiro de software,
deve ser mais tranquilo começar pela _Solution Architecture_.

## Conclusão

A horizontal _Foundational_ é para quem não tem experiência com AWS, ou não é da
área que coloca a mão na massa, mas utiliza AWS para apoiar um produto ou
trabalha com times que utilizam a AWS.

A horizontal _Associate_ é o suficiente para quem põem a mão na massa no dia a
dia, mas não deseja ser um especialista em todos os serviços e o que a AWS tem
a oferecer. Para esse nível de profundidade e entendimento a horizontal
_Professional_ se encaixa perfeitamente.

Só obtenha certicicações _Specialty_ se desejar trabalhar com serviços e áreas
específicas da AWS. Um exceção pode ser a certificação _Advanced Networking -
Specialty_ que pode auxiliar quem pretende atingir nível _Professional_.

Como sugestão, siga ordem de estudos:

1. Cloud Practitioner - Foundational
1. Certified Solutions Architect – Associate
1. Certified Developer – Associate
1. Certified SysOps Admin – Associate
1. Certified DevOps – Professional
1. Certified Solutions Architect – Professional

Essa deve ser a ordem mais tranquila, apresentando uma boa visão da nuvem AWS
com um incremento de dificuldade relevante a cada horizontal aplicada. Pule a
primeira se você já colocou alguma coisa em produção e pagou com o seu cartão de
crédito por isso. Pare na horizontal Associate (Certified SysOps Admin –
Associate) se o seu objetivo não é saber de tudo nos mínimos detalhes, caso
contrário, siga até o final da horizontal _Professional_.

Mais detalhes sobre as certificações AWS, veja em <https://aws.amazon.com/certification/>

## Referências

1. AWS. AWS Certification. <https://aws.amazon.com/pt/certification/>

<!--
Futuros links.
[1]: Lambada por debaixo dos panos
[2]: Orquestrando containers com ECS-->
