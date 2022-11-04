---
title: Desenvolver exportadores de modelo do Sling no AEM
description: Esta caminhada técnica percorre a configuração de AEM para uso com o Exportador do Modelo Sling, aprimorando um Modelo Sling existente usando a estrutura Exportador para representação como JSON, e como usar opções de Exportador e anotações Jackson para personalizar ainda mais a saída.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# Desenvolver exportadores de modelo do Sling

Esta caminhada técnica percorre a configuração de AEM para uso com o Exportador do Modelo Sling, aprimorando um Modelo Sling existente usando a estrutura Exportador para representação como JSON, e como usar opções de Exportador e anotações Jackson para personalizar ainda mais a saída.

O Sling Model Exporter foi introduzido nos Modelos do Sling v1.3.0. Este novo recurso permite que novas anotações sejam adicionadas aos Modelos do Sling que definem como o Modelo pode ser exportado como um objeto Java diferente ou, mais frequentemente, serializado em um formato diferente, como JSON.

O Apache Sling fornece um exportador JSON Jackson para cobrir o caso mais comum de exportação de Modelos Sling como objetos JSON para consumo por consumidores programáticos da Web, como outros serviços da Web e aplicativos JavaScript.

## Configurando AEM para Exportador de Modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] é um recurso do [!DNL Apache Sling] projeto e não diretamente vinculado ao ciclo de lançamento do produto AEM. [!DNL Sling Model Exporter] é compatível com a AEM 6.3 e posteriores.

## O caso de uso para [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] O é perfeito para aproveitar Modelos do Sling que já contêm lógica de negócios que suportam representações de HTML via HTL (ou antigo JSP) e expor a mesma representação de negócios que JSON para consumo por serviços Web programáticos ou aplicativos JavaScript.

## Criando um Exportador de Modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Habilitar [!DNL Exporter] suporte em um [!DNL Sling Model] é tão fácil quanto adicionar o `@Exporter` anotação para a classe Java.

## Aplicação das opções do exportador do modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] O suporta o envio de opções por modelo do exportador para a implementação do exportador para mostrar como a variável [!DNL Sling Model] é finalmente exportado. Essas opções geralmente se aplicam &quot;globalmente&quot; à forma como a variável [!DNL Sling Model] é exportado, versus por ponto de dados, que pode ser feito por meio de anotações em linha, descritas abaixo.

[!DNL Jackson Exporter] as opções incluem:

* [Opções de recursos do Mapeador](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opções de recurso de serialização](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicação [!DNL Jackson] anotações

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

As implementações dos exportadores também podem suportar anotações que podem ser aplicadas em linha no [!DNL Sling Model] , que pode fornecer um nível mais refinado de controle sobre como os dados são exportados.

* [[!DNL Jackson Exporter] Anotações](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualizar o código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiais de apoio {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc de recursos](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc de recursos](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
