---
title: Como desativar o armazenamento em cache do CDN
description: Saiba como desativar o armazenamento em cache de respostas HTTP no CDN da AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: cf006f24abbc5aa4b91277b91d68538c41d33e15
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Como desativar o armazenamento em cache do CDN

Saiba como desativar o armazenamento em cache de respostas HTTP no CDN da AEM as a Cloud Service. O cache de respostas é controlado por `Cache-Control`, `Surrogate-Control` ou `Expires` cabeçalhos de cache de resposta HTTP.

Normalmente, esses cabeçalhos de cache são definidos nas configurações do AEM Dispatcher vhost usando o `mod_headers`, mas também podem ser definidos no código Java™ personalizado em execução no próprio AEM Publish.

## Comportamento de cache padrão

O armazenamento em cache das respostas HTTP no CDN da AEM as a Cloud Service é controlado pelos seguintes cabeçalhos de resposta HTTP da origem `Cache-Control`, `Surrogate-Control` ou `Expires`.  As respostas de origem que contêm `private`, `no-cache` ou `no-store` em `Cache-Control` não são armazenadas em cache.

Revise o [comportamento padrão de armazenamento em cache](./enable-caching.md#default-caching-behavior) para Publicação e Autor do AEM quando um projeto do AEM baseado no Arquétipo de Projetos AEM for implantado.


## Desativar armazenamento em cache

A desativação do armazenamento em cache pode ter um impacto negativo no desempenho da sua instância do AEM as a Cloud Service, portanto, tenha cuidado ao desativar o comportamento padrão de armazenamento em cache.

No entanto, há alguns cenários em que você pode desejar desativar o armazenamento em cache, como:

- Desenvolver um novo recurso e desejar ver as alterações imediatamente.
- O conteúdo é seguro (destinado apenas a usuários autenticados) ou dinâmico (carrinho de compras, detalhes do pedido) e não deve ser armazenado em cache.

Para desativar o armazenamento em cache, você pode atualizar os cabeçalhos de cache de duas maneiras.

1. **Configuração do Dispatcher vhost:** disponível somente para publicação do AEM.
1. **Código Java™ personalizado:** disponível para publicação e autor do AEM.

Vamos analisar cada uma dessas opções.

### Configuração do Dispatcher vhost

Essa opção é a abordagem recomendada para desabilitar o armazenamento em cache, no entanto, só está disponível para o AEM Publish. Para atualizar os cabeçalhos de cache, use o módulo `mod_headers` e a diretiva `<LocationMatch>` no arquivo vhost do Apache HTTP Server. A sintaxe geral é a seguinte:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### Exemplo

Para desabilitar o cache CDN dos **tipos de conteúdo CSS** para alguns fins de solução de problemas, siga estas etapas.

Observe que, para ignorar o cache CSS existente, uma alteração no arquivo CSS é necessária para gerar uma nova chave de cache para o arquivo CSS.

1. No projeto do AEM, localize o arquivo vhsot desejado do diretório `dispatcher/src/conf.d/available_vhosts`.
1. Atualize o arquivo vhost (por exemplo, `wknd.vhost`) da seguinte maneira:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   Os arquivos vhost no diretório `dispatcher/src/conf.d/enabled_vhosts` são **symlinks** para os arquivos no diretório `dispatcher/src/conf.d/available_vhosts`. Portanto, se não houver, crie symlinks.
1. Implante as alterações do vhost no ambiente do AEM as a Cloud Service desejado usando o [Pipeline de Configuração da Camada da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) ou os [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration) do Cloud Manager.

### Código Java™ personalizado

Essa opção está disponível para Publicação no AEM e Autor. Para atualizar os cabeçalhos de cache, use o objeto `SlingHttpServletResponse` no código Java™ personalizado (servlet Sling, filtro de servlet Sling). A sintaxe geral é a seguinte:

```java
response.setHeader("Cache-Control", "private");
```
