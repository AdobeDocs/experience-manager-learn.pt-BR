---
title: Entender o Exportador do Modelo Sling em AEM
description: O Apache Sling Models 1.3.0 apresenta o Sling Model Exporter, uma maneira elegante de exportar ou serializar objetos do Sling Model em abstrações personalizadas. Este artigo justapõe o caso de uso tradicional de usar Modelos do Sling para preencher scripts HTL, com o aproveitamento da estrutura do Exportador do Modelo do Sling para serializar um Modelo do Sling em JSON.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# Entender [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduz [!DNL Sling Model Exporter], uma maneira elegante de exportar ou serializar [!DNL Sling Model] objetos em abstrações personalizadas. Este artigo justapõe o caso de uso tradicional de [!DNL Sling Models] para preencher scripts HTL, com o uso da variável [!DNL Sling Model Exporter] estrutura para serializar um [!DNL Sling Model] no JSON.

## Fluxo de Solicitação HTTP do Modelo de Sling Tradicional

O caso de uso tradicional para [!DNL Sling Models] O é fornecer uma abstração de negócios para um recurso ou solicitação, que fornece scripts HTL (ou JSPs anteriores) e uma interface para acessar funções de negócios.

Padrões comuns em desenvolvimento [!DNL Sling Models] que representam AEM componentes ou páginas e usam o [!DNL Sling Model] objetos para alimentar os scripts HTL com dados, com um resultado final do HTML exibido no navegador.

### Fluxo de Solicitação HTTP do Modelo Sling

![Fluxo de solicitação do modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] A solicitação é feita para um recurso no AEM.

   Exemplo: `HTTP GET /content/my-resource.html`

1. Com base no `sling:resourceType`, o Script apropriado é resolvido.

1. O Script adapta a Solicitação ou o Recurso ao [!DNL Sling Model].

1. O script usa a variável [!DNL Sling Model] para gerar a representação de HTML.

1. O HTML gerado pelo Script é retornado na Resposta HTTP.

Esse padrão tradicional funciona bem no contexto da geração de HTML como o [!DNL Sling Model] pode ser facilmente aproveitado via HTL. Criar dados mais estruturados, como JSON ou XML, é um esforço muito mais entediante, já que HTL não se presta naturalmente à definição desses formatos.

## [!DNL Sling Model Exporter] Fluxo de solicitação HTTP

Apache [!DNL Sling Model Exporter] vem com um exportador de Jackson fornecido pelo Sling que serializa automaticamente um &quot;normal&quot; [!DNL Sling Model] em JSON. O Exportador Jackson, embora bastante configurável, no seu núcleo inspeciona o [!DNL Sling Model] e gera JSON usando qualquer método &quot;getter&quot; como chaves JSON e o getter retorna valores como os valores JSON.

A serialização direta de [!DNL Sling Models] permite que eles atendam às solicitações normais da Web com as respostas do HTML criadas usando o [!DNL Sling Model] fluxo de solicitação (veja acima), mas também exponha representações JSON que podem ser consumidas por serviços da Web ou aplicativos JavaScript.

![Fluxo de Solicitação HTTP do Exportador do Modelo Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este fluxo descreve o fluxo utilizando o exportador de Jackson fornecido para produzir a saída JSON. A utilização de exportadores personalizados segue o mesmo fluxo, mas com o respectivo formato de saída.*

1. HTTP GET Request é feito para um recurso no AEM com o seletor e a extensão registrada no [!DNL Sling Model]Exportador.

   Exemplo: `HTTP GET /content/my-resource.model.json`

1. O Sling resolve o `sling:resourceType`, seletor e extensão para um Servlet Exportador Sling gerado dinamicamente, que é mapeado para o [!DNL Sling Model] com Exportador.
1. O Servlet do Exportador Sling resolvido chama o [!DNL Sling Model Exporter] contra [!DNL Sling Model] objeto adaptado a partir da solicitação ou do recurso (conforme determinado pelos adaptáveis dos Modelos do Sling).
1. O exportador serializa o [!DNL Sling Model] com base nas anotações das opções do exportador e do modelo Sling específico do exportador e retorna o resultado ao Servlet do exportador Sling.
1. O Servlet do Exportador Sling retorna a representação JSON do [!DNL Sling Model] na Resposta HTTP.

>[!NOTE]
>
>Enquanto o projeto Apache Sling fornece o Exportador Jackson que serializa [!DNL Sling Models] para JSON, o quadro Exportador também oferece suporte a exportadores personalizados. Por exemplo, um projeto pode implementar um Exportador personalizado que serializa um [!DNL Sling Model] em XML.

>[!NOTE]
>
>Não apenas [!DNL Sling Model Exporter] *serializar* [!DNL Sling Models], também pode exportá-los como objetos Java. A exportação para outros objetos Java não desempenha uma função no fluxo da Solicitação HTTP e, portanto, não aparece no diagrama acima.

## Materiais de apoio

* [Apache [!DNL Sling Model Exporter] Documentação da estrutura](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
