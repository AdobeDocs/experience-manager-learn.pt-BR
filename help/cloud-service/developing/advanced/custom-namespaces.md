---
title: Namespaces personalizados
description: Saiba como definir e implantar namespaces personalizados para AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 79804ac1ec18f8ac4815bd5ee6f309ef85110cb9
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 4%

---

# Namespaces personalizados

Saiba como definir e implantar personalizados [namespaces](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) AEM as a Cloud Service.

Os namespaces personalizados são a parte opcional de uma propriedade JCR que precede uma `:`. AEM usa vários namespaces, como:

+ `jcr` para propriedades do sistema JCR
+ `cq` para propriedades AEM (anteriormente conhecidas como Adobe CQ)
+ `dam` para AEM propriedades específicas de ativos DAM
+ `dc` para propriedades Dublin Core

... e muitos outros.

Os namespaces podem ser usados para indicar o escopo e a intenção de uma propriedade. A criação de um namespace personalizado, geralmente o nome da sua empresa, ajuda a identificar claramente os nós ou propriedades específicos da sua implementação de AEM e a conter dados específicos da sua empresa.

Os namespaces personalizados são gerenciados em [Inicialização do Repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) scripts e implantações em AEM as a Cloud Service como configurações OSGi - e adicionadas a [AEM do projeto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) `ui.config` projeto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319/?quality=12&learn=on)

## Recursos

+ [Documentação de Inicialização do Repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Código

O código a seguir é usado para configurar um `wknd` namespace.

### Configuração OSGi do RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Isso permite que propriedades personalizadas usem o `wknd` namespace, como indicado como o primeiro parâmetro após a variável `register namespace` para ser usada em AEM. Para obter definições de script mais avançadas, revise os exemplos na seção [Documentação de Inicialização do Repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
