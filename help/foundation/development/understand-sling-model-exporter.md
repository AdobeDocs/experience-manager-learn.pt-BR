---
title: Entender o exportador de modelo do Sling no AEM
description: O Apache Sling Models 1.3.0 apresenta o Exportador de modelos Sling, uma maneira elegante de exportar ou serializar objetos do Modelo Sling em abstrações personalizadas. Este artigo justapõe o caso de uso tradicional de usar Modelos Sling para preencher scripts HTL, com a utilização da estrutura do Exportador de modelo Sling para serializar um Modelo Sling em JSON.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Entender [!DNL Sling Model Exporter]

O Apache [!DNL Sling Models] 1.3.0 apresenta o [!DNL Sling Model Exporter], uma maneira elegante de exportar ou serializar objetos [!DNL Sling Model] em abstrações personalizadas. Este artigo justapõe o caso de uso tradicional do uso de [!DNL Sling Models] para preencher scripts HTL, com a utilização da estrutura [!DNL Sling Model Exporter] para serializar um [!DNL Sling Model] em JSON.

## Fluxo de solicitação HTTP do modelo Sling tradicional

O caso de uso tradicional de [!DNL Sling Models] é fornecer uma abstração de negócios para um recurso ou solicitação, que fornece scripts HTL (ou, anteriormente JSPs) uma interface para acessar funções de negócios.

Padrões comuns são o desenvolvimento de [!DNL Sling Models] que representam Componentes ou Páginas AEM e o uso de objetos [!DNL Sling Model] para alimentar scripts HTL com dados, com um resultado final de HTML que é exibido no navegador.

### Fluxo de solicitação HTTP do modelo Sling

![Fluxo De Solicitação De Modelo Do Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Solicitação feita para um recurso no AEM.

   Exemplo: `HTTP GET /content/my-resource.html`

1. Com base no recurso de solicitação `sling:resourceType`, o Script apropriado é resolvido.

1. O Script adapta a Solicitação ou Recurso ao [!DNL Sling Model] desejado.

1. O Script usa o objeto [!DNL Sling Model] para gerar a representação de HTML.

1. O HTML gerado pelo script é retornado na Resposta HTTP.

Esse padrão tradicional funciona bem no contexto da geração de HTML, pois o [!DNL Sling Model] pode ser facilmente aproveitado via HTL. Criar dados mais estruturados, como JSON ou XML, é uma tarefa muito mais tediosa, pois o HTL não se presta naturalmente à definição desses formatos.

## Fluxo de solicitação HTTP [!DNL Sling Model Exporter]

O Apache [!DNL Sling Model Exporter] vem com um Exportador Jackson fornecido pelo Sling que serializa automaticamente um objeto [!DNL Sling Model] &quot;comum&quot; em JSON. O Exportador Jackson, embora bastante configurável, em seu núcleo inspeciona o objeto [!DNL Sling Model] e gera JSON usando quaisquer métodos &quot;getter&quot; como chaves JSON, e os valores de retorno de getter como valores JSON.

A serialização direta de [!DNL Sling Models] permite que eles atendam às solicitações normais da Web com suas respostas de HTML criadas usando o fluxo de solicitações [!DNL Sling Model] tradicional (veja acima), mas também expõem representações JSON que podem ser consumidas por serviços Web ou aplicativos JavaScript.

![Fluxo de solicitação HTTP do exportador do modelo do Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este fluxo descreve o fluxo usando o Exportador Jackson fornecido para produzir a saída JSON. O uso de exportadores personalizados segue o mesmo fluxo, mas com seu formato de saída.*

1. GET A Solicitação HTTP é feita para um recurso no AEM com o seletor e a extensão registrados com o Exportador do [!DNL Sling Model].

   Exemplo: `HTTP GET /content/my-resource.model.json`

1. O Sling resolve o `sling:resourceType`, o seletor e a extensão do recurso solicitado para um Sling Exporter Servlet gerado dinamicamente, que é mapeado para o [!DNL Sling Model] com o Exportador.
1. O Sling Exporter Servlet resolvido invoca o [!DNL Sling Model Exporter] contra o objeto [!DNL Sling Model] adaptado a partir da solicitação ou do recurso (conforme determinado pelos adaptáveis dos Modelos Sling).
1. O exportador serializa o [!DNL Sling Model] com base nas opções do exportador e nas anotações do modelo Sling específicas do exportador e retorna o resultado para o Sling Exporter Servlet.
1. O Sling Exporter Servlet retorna a representação JSON de [!DNL Sling Model] na Resposta HTTP.

>[!NOTE]
>
>Embora o projeto Apache Sling forneça o Exportador Jackson que serializa [!DNL Sling Models] para JSON, a estrutura do Exportador também oferece suporte a Exportadores personalizados. Por exemplo, um projeto poderia implementar um Exportador personalizado que serializa um [!DNL Sling Model] em XML.

>[!NOTE]
>
>Não apenas [!DNL Sling Model Exporter] *serializa* [!DNL Sling Models], mas também pode exportá-los como objetos Java. A exportação para outros objetos Java não desempenha uma função no fluxo de solicitação HTTP e, portanto, não aparece no diagrama acima.

## Materiais de suporte

* [Apache [!DNL Sling Model Exporter] Documentação da estrutura](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
