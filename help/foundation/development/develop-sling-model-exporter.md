---
title: Desenvolver exportadores de modelos Sling em AEM
description: Esse técnico percorre a configuração de AEM para uso com o Sling Model Exporter, aprimorando um Sling Model existente usando a estrutura Exporter para renderização como JSON, e como usar opções Exporter e anotações Jackson para personalizar ainda mais a saída.
version: 6.3, 6.4, 6.5
sub-product: fundação, serviços de conteúdo
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# Desenvolver Exportadores de Modelos Sling

Esse técnico percorre a configuração de AEM para uso com o Sling Model Exporter, aprimorando um Sling Model existente usando a estrutura Exporter para renderização como JSON, e como usar opções Exporter e anotações Jackson para personalizar ainda mais a saída.

O Sling Model Exporter foi introduzido nos Modelos Sling v1.3.0. Esse novo recurso permite que novas anotações sejam adicionadas aos Modelos Sling que definem como o Modelo pode ser exportado como um objeto Java diferente, ou, mais frequentemente, serializado em um formato diferente, como JSON.

Apache Sling fornece um exportador Jackson JSON para cobrir o caso mais comum de exportação de Sling Models como objetos JSON para consumo por consumidores programáticos da Web, como outros serviços da Web e aplicativos JavaScript.

## Configuração do AEM para Exportador de Modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] é um recurso do  [!DNL Apache Sling] projeto e não está vinculado diretamente ao ciclo de lançamento do produto AEM. [!DNL Sling Model Exporter] é compatível com AEM 6.3 e posterior.

## O caso de uso para [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] é perfeito para aproveitar modelos Sling que já contêm lógica de negócios que suportam execuções HTML via HTL (ou antigo JSP) e expõe a mesma representação de negócios que JSON para consumo por serviços Web programáticos ou aplicativos JavaScript.

## Criando um Exportador de Modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Habilitar o suporte a [!DNL Exporter] em um [!DNL Sling Model] é tão fácil quanto adicionar a anotação `@Exporter` à classe Java.

## Aplicando opções do Exportador do Modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] O oferece suporte para o envio de opções por modelo do Exportador para a implementação do Exportador, a fim de determinar como o exportador  [!DNL Sling Model] é finalmente exportado. Essas opções geralmente se aplicam &quot;globalmente&quot; à maneira como [!DNL Sling Model] é exportado, em comparação com cada ponto de dados que pode ser feito por meio de anotações em linha descritas abaixo.

[!DNL Jackson Exporter] as opções incluem:

* [Opções de recurso do mapeador](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opções de recurso de serialização](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicar anotações [!DNL Jackson]

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

As implementações de exportadores também podem suportar anotações que podem ser aplicadas em linha na classe [!DNL Sling Model], que podem fornecer um nível mais fino de controle sobre como os dados são exportados.

* [[!DNL Jackson Exporter] Anotações](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualização do código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiais de suporte {#supporting-materials}

* [[!DNL Jackson Mapper] Recurso Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Recurso Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
