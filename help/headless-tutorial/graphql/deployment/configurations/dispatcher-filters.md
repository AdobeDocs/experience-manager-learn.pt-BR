---
title: Filtros do Dispatcher para o AEM GraphQL
description: Saiba como configurar os filtros do AEM Publish Dispatcher para usar com o AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Filtros do Dispatcher

O Adobe Experience Manager as a Cloud Service usa os filtros do AEM Publish Dispatcher para garantir que somente as solicitações que devem chegar ao AEM cheguem ao AEM. Por padrão, todas as solicitações são negadas e os padrões para URLs permitidos devem ser adicionados explicitamente.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente da Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requer a configuração de filtros do Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para se alinharem aos requisitos do seu projeto.

## Configuração de filtro do Dispatcher

A configuração do filtro AEM Publish Dispatcher define os padrões de URL permitidos para acessar o AEM e deve incluir o prefixo de URL para o endpoint da consulta persistente do AEM.

| O cliente se conecta ao | Autor do AEM | Publicação no AEM | Visualização do AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração de filtros do Dispatcher | ✘ | ✔ | ✔ |

Adicione uma regra `allow` com o padrão de URL `/graphql/execute.json/*` e verifique se a ID do arquivo (por exemplo `/0600`, é exclusiva no arquivo de farm de exemplo).
Isso permite a solicitação HTTP GET para o ponto de extremidade da consulta persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` até a Publicação do AEM.

Se estiver usando Fragmentos de experiência na sua experiência do AEM Headless, faça o mesmo para esses caminhos.

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
