---
title: Filtros do Dispatcher para AEM GraphQL
description: Saiba como configurar filtros do AEM Publish Dispatcher para uso com o AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Filtros do Dispatcher

O Adobe Experience Manager as a Cloud Service usa filtros AEM do Publish Dispatcher AEM AEM para garantir que somente as solicitações que devem chegar ao do sejam atendidas. Por padrão, todas as solicitações são negadas e os padrões para URLs permitidos devem ser adicionados explicitamente.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente da Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requer a configuração de filtros do Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para se alinharem aos requisitos do seu projeto.

## Configuração de filtro do Dispatcher

A configuração de filtro AEM Publish Dispatcher define os padrões de URL permitidos para alcançar o AEM AEM e deve incluir o prefixo do URL para o endpoint de consulta persistente do.

| O cliente se conecta ao | Autor do AEM | Publicação no AEM | Visualização do AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração de filtros do Dispatcher | ✘ | ✔ | ✔ |

Adicione uma regra `allow` com o padrão de URL `/graphql/execute.json/*` e verifique se a ID do arquivo (por exemplo `/0600`, é exclusiva no arquivo de farm de exemplo).
Isso permite a solicitação HTTP GET para o endpoint da consulta persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` por meio do AEM Publish.

Se estiver usando Fragmentos de experiência na sua experiência com AEM Headless, faça o mesmo para esses caminhos.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Exemplo de configuração de filtros

+ [Um exemplo do filtro Dispatcher pode ser encontrado no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
