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
duration: 957
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
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

[!DNL Sling Model Exporter] é um recurso do [!DNL Apache Sling] projeto e não diretamente vinculado ao ciclo de lançamento do produto AEM. [!DNL Sling Model Exporter] é compatível com o AEM 6.3 e posteriores.

## O caso de uso para [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] O é perfeito para aproveitar os Modelos do Sling que já contêm lógica de negócios que oferecem suporte a representações de HTML via HTL (ou anteriormente JSP) e expõem a mesma representação de negócios que o JSON para consumo por serviços da Web programáticos ou aplicativos JavaScript.

## Criação de um exportador de modelo do Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Ativando [!DNL Exporter] suporte em um [!DNL Sling Model] é tão fácil quanto adicionar o `@Exporter` anotação para a classe Java.

## Aplicação das opções do Exportador de modelo do Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] permite transmitir opções por modelo do Exportador para a implementação do Exportador para orientar como o [!DNL Sling Model] foi exportado. Essas opções geralmente se aplicam &quot;globalmente&quot; à forma como as [!DNL Sling Model] é exportado, versus por ponto de dados, o que pode ser feito por meio de anotações em linha descritas abaixo.

[!DNL Jackson Exporter] As opções incluem:

* [Opções de recurso do mapeador](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opções de recurso de serialização](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicando [!DNL Jackson] anotações

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

As implementações dos exportadores também podem suportar anotações que podem ser aplicadas em linha no [!DNL Sling Model] que pode fornecer um nível mais fino de controle sobre como os dados são exportados.

* [[!DNL Jackson Exporter] Anotações](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Exibir o código {#view-the-code}

[AmostraSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiais de suporte {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc de recurso](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc de recurso](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentação](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
