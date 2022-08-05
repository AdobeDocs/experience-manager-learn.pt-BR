---
title: Filtros do Dispatcher para AEM GraphQL
description: Saiba como configurar filtros do Dispatcher de publicação do AEM para uso com AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---


# Filtros do Dispatcher

O Adobe Experience Manager as a Cloud Service usa filtros de Dispatcher de publicação do AEM para garantir que apenas as solicitações que devem AEM alcance o AEM. Por padrão, todas as solicitações são negadas e os padrões para URLs permitidos devem ser explicitamente adicionados.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente da Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Exige configuração de filtros do Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para alinhar-se às necessidades do seu projeto.

## Configuração de filtro do Dispatcher

A configuração de filtro Publicar Dispatcher do AEM define os padrões de URL permitidos para alcançar o AEM e deve incluir o prefixo de URL do ponto de extremidade de consulta persistente do AEM.

| O cliente se conecta a | Autor do AEM | AEM Publish | Visualização de AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Exige configuração de filtros do Dispatcher | ✘ | ✔ | ✔ |

Adicione um `allow` com o padrão de URL `/graphql/execute.json/*`e garantir a ID do arquivo (por exemplo, `/0600`, é exclusiva no arquivo farm de exemplo).
Isso permite a solicitação HTTP GET para o endpoint de consulta persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` para AEM Publish.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### Exemplo de configuração de filtros

+ [Um exemplo do filtro Dispatcher pode ser encontrado no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)