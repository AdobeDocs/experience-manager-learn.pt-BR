---
title: Desenvolver exportadores de modelo do Sling no AEM
description: Essa caminhada técnica percorre a configuração do AEM para uso com o Exportador do Modelo do Sling, aprimorando um Modelo do Sling existente usando a estrutura Exportador para representação como JSON, e como usar opções de Exportador e anotações Jackson para personalizar ainda mais a saída.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---


# Desenvolver exportadores de modelo do Sling

Essa caminhada técnica percorre a configuração do AEM para uso com o Exportador do Modelo do Sling, aprimorando um Modelo do Sling existente usando a estrutura Exportador para representação como JSON, e como usar opções de Exportador e anotações Jackson para personalizar ainda mais a saída.

O Sling Model Exporter foi introduzido nos Modelos do Sling v1.3.0. Este novo recurso permite que novas anotações sejam adicionadas aos Modelos do Sling que definem como o Modelo pode ser exportado como um objeto Java diferente ou, mais frequentemente, serializado em um formato diferente, como JSON.

O Apache Sling fornece um exportador JSON Jackson para cobrir o caso mais comum de exportação de Modelos Sling como objetos JSON para consumo por consumidores programáticos da Web, como outros serviços da Web e aplicativos JavaScript.

## Configuração do AEM para Exportador de Modelo do Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] é um recurso do  [!DNL Apache Sling] projeto e não está vinculado diretamente ao ciclo de lançamento do produto AEM. [!DNL Sling Model Exporter] O é compatível com o AEM 6.3 e posterior.

## O caso de uso para [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] O é perfeito para utilizar Modelos do Sling que já contêm lógica de negócios que suporta renderizações de HTML via HTL (ou antigo JSP) e expor a mesma representação de negócios que JSON para consumo por serviços Web programáticos ou aplicativos JavaScript.

## Criando um Exportador de Modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Habilitar o suporte a [!DNL Exporter] em um [!DNL Sling Model] é tão fácil quanto adicionar a anotação `@Exporter` à classe Java.

## Aplicação das opções do exportador do modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] O suporta o envio de opções por modelo Exportador para a implementação Exportador para direcionar como o  [!DNL Sling Model] é finalmente exportado. Essas opções geralmente se aplicam &quot;globalmente&quot; à maneira como o [!DNL Sling Model] é exportado, em comparação com por ponto de dados, que pode ser feito por meio de anotações em linha descritas abaixo.

[!DNL Jackson Exporter] as opções incluem:

* [Opções de recursos do Mapeador](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opções de recurso de serialização](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicar anotações [!DNL Jackson]

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

As implementações dos exportadores também podem oferecer suporte a anotações que podem ser aplicadas em linha na classe [!DNL Sling Model], que podem fornecer um nível mais refinado de controle sobre como os dados são exportados.

* [[!DNL Jackson Exporter] Anotações](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualizar o código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiais de suporte {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc de recursos](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc de recursos](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
