---
title: Como ativar o armazenamento em cache do CDN
description: Saiba como habilitar o armazenamento em cache de respostas HTTP no CDN da AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 1%

---

# Como ativar o armazenamento em cache do CDN

Saiba como habilitar o armazenamento em cache de respostas HTTP no CDN da AEM as a Cloud Service. O cache de respostas é controlado por `Cache-Control`, `Surrogate-Control` ou `Expires` cabeçalhos de cache de resposta HTTP.

Normalmente, esses cabeçalhos de cache são definidos nas configurações do AEM Dispatcher vhost usando o `mod_headers`, mas também podem ser definidos no código Java™ personalizado em execução no próprio AEM Publish.

## Comportamento de cache padrão

Quando as configurações personalizadas NÃO estiverem presentes, os valores padrão serão usados. Na captura de tela a seguir, você pode ver o comportamento padrão de armazenamento em cache de Publicação e Autor do AEM quando um [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) baseado no `mynewsite` AEM é implantado.

![Comportamento de cache padrão](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Revise a [Publicação do AEM - Vida padrão do cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) e [Autor do AEM - Vida padrão do cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) para obter mais informações.

Em resumo, o AEM as a Cloud Service armazena em cache a maioria dos tipos de conteúdo (HTML, JSON, JS, CSS e Assets) no AEM Publish e alguns tipos de conteúdo (JS, CSS) no AEM Author.

## Habilitar armazenamento em cache

Para alterar o comportamento padrão de armazenamento em cache, você pode atualizar os cabeçalhos de cache de duas maneiras.

1. **Configuração do Dispatcher vhost:** disponível somente para publicação do AEM.
1. **Código Java™ personalizado:** disponível para publicação e autor do AEM.

Vamos analisar cada uma dessas opções.

### Configuração do Dispatcher vhost

Essa opção é a abordagem recomendada para ativar o armazenamento em cache, no entanto, só está disponível para o AEM Publish. Para atualizar os cabeçalhos de cache, use o módulo `mod_headers` e a diretiva `<LocationMatch>` no arquivo vhost do Apache HTTP Server. A sintaxe geral é a seguinte:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

A seguir está um resumo da finalidade de cada **cabeçalho** e dos **atributos** aplicáveis ao cabeçalho.

|                     | Navegador da Web | CDN | Descrição |
|---------------------|:-----------:|:---------:|:-----------:|
| Controle de cache | ✔ | ✔ | Esse cabeçalho controla a vida útil do navegador da Web e do cache CDN. |
| Surrogate-Control | ✘ | ✔ | Esse cabeçalho controla a vida útil do cache da CDN. |
| Expira em | ✔ | ✔ | Esse cabeçalho controla a vida útil do navegador da Web e do cache CDN. |


- **max-age**: Este atributo controla o TTL ou o &quot;tempo de vida&quot; do conteúdo da resposta em segundos.
- **stale-while-revalidate**: este atributo controla o tratamento do _estado obsoleto_ do conteúdo da resposta na camada CDN quando a solicitação recebida está dentro do período especificado em segundos. O _estado obsoleto_ é o período de tempo após a expiração do TTL e antes da revalidação da resposta.
- **stale-if-error**: este atributo controla o tratamento do _estado obsoleto_ do conteúdo da resposta na camada CDN quando o servidor de origem está indisponível e a solicitação recebida está dentro do período especificado em segundos.

Revise os detalhes de [desatualização e revalidação](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) para obter mais informações.

#### Exemplo

Para aumentar a vida do navegador da Web e do cache da CDN do **tipo de conteúdo do HTML** para _10 minutos_ sem tratamento de estado obsoleto, siga estas etapas:

1. No projeto do AEM, localize o arquivo vhsot desejado do diretório `dispatcher/src/conf.d/available_vhosts`.
1. Atualize o arquivo vhost (por exemplo, `wknd.vhost`) da seguinte maneira:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   Os arquivos vhost no diretório `dispatcher/src/conf.d/enabled_vhosts` são **symlinks** para os arquivos no diretório `dispatcher/src/conf.d/available_vhosts`. Portanto, se não houver, crie symlinks.
1. Implante as alterações do vhost no ambiente do AEM as a Cloud Service desejado usando o [Pipeline de Configuração da Camada da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) ou os [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration) do Cloud Manager.

No entanto, para ter valores diferentes para a vida útil do navegador da Web e do cache CDN, você pode usar o cabeçalho `Surrogate-Control` no exemplo acima. Da mesma forma que para expirar o cache em uma data e hora específicas, você pode usar o cabeçalho `Expires`. Além disso, usando os atributos `stale-while-revalidate` e `stale-if-error`, você pode controlar o tratamento de estado obsoleto do conteúdo da resposta. O projeto WKND do AEM tem uma [configuração de cache de CDN de tratamento de estado obsoleto de referência](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155).

Da mesma forma, também é possível atualizar os cabeçalhos de cache de outros tipos de conteúdo (JSON, JS, CSS e Assets).

### Código Java™ personalizado

Essa opção está disponível para Publicação no AEM e Autor. No entanto, não é recomendável ativar o armazenamento em cache no AEM Author e manter o comportamento padrão de armazenamento em cache.

Para atualizar os cabeçalhos de cache, use o objeto `HttpServletResponse` no código Java™ personalizado (servlet Sling, filtro de servlet Sling). A sintaxe geral é a seguinte:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
