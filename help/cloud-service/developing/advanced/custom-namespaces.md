---
title: Namespaces personalizados
description: Saiba como definir e implantar namespaces personalizados para o AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# Namespaces personalizados

Saiba como definir e implantar [namespaces](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) personalizados no AEM as a Cloud Service.

Os namespaces personalizados são a parte opcional de uma propriedade JCR que precede um `:`. O AEM usa vários namespaces, como:

+ `jcr` para propriedades do sistema JCR
+ `cq` para propriedades do AEM (anteriormente conhecido como Adobe CQ)
+ `dam` para propriedades do AEM específicas para ativos DAM
+ `dc` para as propriedades principais de Dublin

... e muitos outros.

Os namespaces podem ser usados para denotar o escopo e a intenção de uma propriedade. A criação de um namespace personalizado, geralmente o nome da sua empresa, ajuda a identificar claramente os nós ou propriedades específicos da sua implementação do AEM e contém dados específicos da sua empresa.

Os namespaces personalizados são gerenciados nos scripts [Inicialização do Repositório de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) e são implantados no AEM as a Cloud Service como configurações OSGi - e adicionados ao projeto [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) `ui.config` do seu projeto do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Recursos

+ [Documentação de Inicialização do Repositório do Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Código

O código a seguir é usado para configurar um namespace `wknd`.

### Configuração OSGi de RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Isso permite que propriedades personalizadas usando o namespace `wknd`, conforme indicado como o primeiro parâmetro após a instrução `register namespace`, sejam usadas no AEM. Para obter definições de script mais avançadas, reveja os exemplos na [documentação de Inicialização do Repositório do Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
