---
title: Namespaces personalizados
description: Saiba como definir e implantar namespaces personalizados para o AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# Namespaces personalizados

Saiba como definir e implantar aplicativos [namespaces](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) para o AEM as a Cloud Service.

Os namespaces personalizados são a parte opcional de uma propriedade JCR que precede uma `:`. O AEM usa vários namespaces, como:

+ `jcr` para propriedades do sistema JCR
+ `cq` para propriedades do AEM (antigo Adobe CQ)
+ `dam` para propriedades do AEM específicas para ativos DAM
+ `dc` para propriedades do Dublin Core

... e muitos outros.

Os namespaces podem ser usados para denotar o escopo e a intenção de uma propriedade. A criação de um namespace personalizado, geralmente o nome da sua empresa, ajuda a identificar claramente os nós ou propriedades específicos da sua implementação de AEM e contém dados específicos da sua empresa.

Os namespaces personalizados são gerenciados no [Inicialização do repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) e implanta no AEM as a Cloud Service como configurações OSGi - e adicionou ao seu [do projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) `ui.config` projeto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Recursos

+ [Documentação de inicialização do repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Código

O código a seguir é usado para configurar um `wknd` namespace.

### Configuração OSGi de RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Isso permite que propriedades personalizadas usem o `wknd` namespace, conforme indicado como o primeiro parâmetro após a variável `register namespace` instruções para utilização no AEM. Para obter definições de script mais avançadas, analise os exemplos na [Documentação de inicialização do repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
