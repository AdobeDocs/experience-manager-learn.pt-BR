---
title: Entender o Exportador do Modelo Sling em AEM
description: O Apache Sling Models 1.3.0 apresenta o Sling Model Exporter, uma maneira elegante de exportar ou serializar objetos do Sling Model em abstrações personalizadas. Este artigo justapõe o caso de uso tradicional de usar modelos Sling para preencher scripts HTL, com a utilização da estrutura do Sling Model Exporter para serializar um Modelo Sling no JSON.
version: 6.3, 6.4, 6.5
sub-product: fundação, serviços de conteúdo
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# Compreender [!DNL Sling Model Exporter]

O Apache [!DNL Sling Models] 1.3.0 apresenta [!DNL Sling Model Exporter], uma maneira elegante de exportar ou serializar [!DNL Sling Model] objetos em abstrações personalizadas. Este artigo justapõe o caso de uso tradicional de usar [!DNL Sling Models] para preencher scripts HTL, com o aproveitamento da [!DNL Sling Model Exporter] estrutura para serializar um arquivo [!DNL Sling Model] no JSON.

## Fluxo de solicitação HTTP do modelo Sling tradicional

O caso de uso tradicional para [!DNL Sling Models] é fornecer uma abstração de negócios para um recurso ou solicitação, que fornece scripts HTL (ou, anteriormente, JSPs) uma interface para acessar funções de negócios.

Padrões comuns estão sendo desenvolvidos [!DNL Sling Models] que representam AEM Componentes ou Páginas, e usando os [!DNL Sling Model] objetos para alimentar os scripts HTL com dados, com um resultado final de HTML exibido no navegador.

### Fluxo de solicitação HTTP do modelo Sling

![Fluxo de solicitação do modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Solicitação feita para um recurso em AEM.

   Exemplo: `HTTP GET /content/my-resource.html`

1. Com base no recurso de solicitação, `sling:resourceType`o Script apropriado é resolvido.

1. O Script adapta a Solicitação ou o Recurso ao desejado [!DNL Sling Model].

1. O Script usa o [!DNL Sling Model] objeto para gerar a execução HTML.

1. O HTML gerado pelo Script é retornado na Resposta HTTP.

Esse padrão tradicional funciona bem no contexto de geração de HTML, pois [!DNL Sling Model] pode ser facilmente aproveitado via HTL. Criar dados mais estruturados como JSON ou XML é um esforço muito mais tedioso, já que HTL não se presta naturalmente à definição desses formatos.

## [!DNL Sling Model Exporter] Fluxo de solicitação HTTP

O Apache [!DNL Sling Model Exporter] vem com um Exportador Jackson fornecido pela Sling que serializa automaticamente um objeto &quot;comum&quot; [!DNL Sling Model] no JSON. O Exportador Jackson, embora bastante configurável, no seu núcleo inspeciona o [!DNL Sling Model] objeto e gera JSON usando qualquer método &quot;getter&quot; como chaves JSON, e o getter return valores como os valores JSON.

A serialização direta de [!DNL Sling Models] permite que eles atendam a solicitações normais da Web com suas respostas HTML criadas usando o fluxo de [!DNL Sling Model] solicitação tradicional (consulte acima), mas também exponham execuções JSON que podem ser consumidas por serviços da Web ou aplicativos JavaScript.

![Fluxo de Solicitação HTTP do Exportador do Modelo Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este fluxo descreve o fluxo utilizando o exportador Jackson fornecido para produzir a saída JSON. O uso de exportadores personalizados segue o mesmo fluxo, mas com seu formato de saída.*

1. A Solicitação de GET HTTP é feita para um recurso em AEM com o seletor e a extensão registrados no Exportador [!DNL Sling Model]do.

   Exemplo: `HTTP GET /content/my-resource.model.json`

1. O Sling resolve o seletor `sling:resourceType`e a extensão do recurso solicitado para um Servlet Exportador Sling gerado dinamicamente, que é mapeado para o [!DNL Sling Model] com Exportador.
1. O Servlet Exportador Sling resolvido invoca o objeto [!DNL Sling Model Exporter] em relação ao [!DNL Sling Model] objeto adaptado da solicitação ou do recurso (conforme determinado pelos adaptáveis Sling Models).
1. O exportador serializa o resultado com [!DNL Sling Model] base nas anotações Opções do Exportador e Modelo de Sling específico do Exportador e retorna o resultado para o Servlet do Exportador de Sling.
1. O Servlet do Exportador Sling retorna a execução JSON do [!DNL Sling Model] na Resposta HTTP.

>[!NOTE]
>
>Enquanto o projeto Apache Sling fornece ao exportador Jackson que serializa [!DNL Sling Models] para JSON, o quadro Exporter também suporta exportadores personalizados. Por exemplo, um projeto poderia implementar um Exportador personalizado que serializa um [!DNL Sling Model] em XML.

>[!NOTE]
>
>Além de [!DNL Sling Model Exporter] serializar ** [!DNL Sling Models], também pode exportá-los como objetos Java. A exportação para outros objetos Java não desempenha uma função no fluxo de Solicitação HTTP e, portanto, não aparece no diagrama acima.

## Materiais de suporte

* [Documentação do [!DNL Sling Model Exporter] ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
