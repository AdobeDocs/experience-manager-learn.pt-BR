---
title: Desenvolver exportadores de modelo Sling no AEM
description: Esta apresentação técnica aborda a configuração do AEM para uso com o Exportador de modelo Sling, aprimorando um Modelo Sling existente usando a estrutura do Exportador para renderização como JSON e como usar as opções do Exportador e as anotações Jackson para personalizar ainda mais a saída.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Desenvolver exportadores de modelo do Sling

Esta apresentação técnica aborda a configuração do AEM para uso com o Exportador de modelo Sling, aprimorando um Modelo Sling existente usando a estrutura do Exportador para renderização como JSON e como usar as opções do Exportador e as anotações Jackson para personalizar ainda mais a saída.

O Exportador de modelo Sling foi introduzido no Sling Models v1.3.0. Esse novo recurso permite que novas anotações sejam adicionadas aos Modelos do Sling, que definem como o Modelo pode ser exportado como um objeto Java diferente ou, mais comumente, serializado em um formato diferente, como JSON.

O Apache Sling fornece um exportador Jackson JSON para cobrir o caso mais comum de exportação de Modelos Sling como objetos JSON para consumo por consumidores da Web programáticos, como outros serviços da Web e aplicativos JavaScript.

## Configuração do AEM para o Exportador de modelo do Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] é um recurso do projeto [!DNL Apache Sling] e não está diretamente vinculado ao ciclo de lançamento do produto AEM. [!DNL Sling Model Exporter] é compatível com AEM 6.3 e posterior.

## O caso de uso para [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

O [!DNL Sling Model Exporter] é perfeito para aproveitar os Modelos do Sling que já contêm lógica de negócios que oferecem suporte a representações de HTML via HTL (ou anteriormente JSP) e expõem a mesma representação de negócios que o JSON para consumo por serviços Web programáticos ou aplicativos JavaScript.

## Criação de um exportador de modelo do Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Habilitar o suporte a [!DNL Exporter] em um [!DNL Sling Model] é tão fácil quanto adicionar a anotação `@Exporter` à classe Java.

## Aplicação das opções do Exportador de modelo do Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

O [!DNL Sling Model Exporter] oferece suporte à transmissão de opções do Exportador por modelo para a implementação do Exportador, a fim de orientar como o [!DNL Sling Model] será exportado. Essas opções geralmente se aplicam &quot;globalmente&quot; à forma como o [!DNL Sling Model] é exportado, em vez de por ponto de dados, o que pode ser feito por meio de anotações incorporadas descritas abaixo.

[!DNL Jackson Exporter] opções incluem:

* [Opções de Recurso do Mapeador](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opções de Recurso de Serialização](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicando [!DNL Jackson] anotações

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

As implementações de exportadores também podem suportar anotações que podem ser aplicadas em linha na classe [!DNL Sling Model], o que pode fornecer um nível mais fino de controle sobre como os dados são exportados.

* [[!DNL Jackson Exporter] Anotações](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Exibir o código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiais de suporte {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc de recursos](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc de recursos](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentação](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
