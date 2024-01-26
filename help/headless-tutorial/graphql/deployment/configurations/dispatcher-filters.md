---
title: Filtros do Dispatcher para o AEM GraphQL
description: Saiba como configurar filtros do Dispatcher de publicação do AEM para uso com o AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 67
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Filtros do Dispatcher

O Adobe Experience Manager as a Cloud Service usa filtros do Dispatcher de publicação do AEM para garantir que somente as solicitações que devem chegar ao AEM do cheguem ao AEM. Por padrão, todas as solicitações são negadas e os padrões para URLs permitidos devem ser adicionados explicitamente.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requer a configuração de filtros do Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para se alinharem aos requisitos do seu projeto.

## Configuração de filtro do Dispatcher

A configuração de filtro do Dispatcher de publicação do AEM define os padrões de URL permitidos para alcançar AEM e deve incluir o prefixo do URL para o endpoint da consulta persistente do AEM.

| O cliente se conecta ao | Autor do AEM | AEM Publish | Visualização do AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração de filtros do Dispatcher | ✘ | ✔ | ✔ |

Adicionar um `allow` regra com o padrão de URL `/graphql/execute.json/*`e verifique a ID do arquivo (por exemplo, `/0600`, é exclusivo no arquivo de farm de exemplo).
Isso permite a solicitação HTTP GET para o endpoint da consulta persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` para AEM Publish.

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
