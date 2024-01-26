---
title: Como ativar o armazenamento em cache do CDN
description: Saiba como habilitar o armazenamento em cache de respostas HTTP no CDN do AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 200
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Como ativar o armazenamento em cache do CDN

Saiba como habilitar o armazenamento em cache de respostas HTTP no CDN do AEM as a Cloud Service. O armazenamento em cache de respostas é controlado pela `Cache-Control`, `Surrogate-Control`ou `Expires` Cabeçalhos de cache de resposta HTTP.

Esses cabeçalhos de cache normalmente são definidos em configurações de vhost do Dispatcher do AEM usando `mod_headers`, mas também pode ser definido no código Java™ personalizado em execução no próprio AEM Publish.

## Comportamento de cache padrão

Quando as configurações personalizadas NÃO estiverem presentes, os valores padrão serão usados. Na captura de tela a seguir, você pode ver o comportamento padrão de armazenamento em cache para AEM Publish e Author quando um [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) baseado `mynewsite` Projeto AEM implantado.

![Comportamento de cache padrão](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Revise o [Publicação no AEM - Vida útil do cache padrão](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) e [Autor do AEM - Vida útil do cache padrão](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) para obter mais informações.

Em resumo, o AEM as a Cloud Service armazena em cache a maioria dos tipos de conteúdo (HTML AEM AEM, JSON, JS, CSS e Assets) no Publish e alguns tipos de conteúdo (JS, CSS) no Author.

## Ativar armazenamento em cache

Para alterar o comportamento padrão de armazenamento em cache, você pode atualizar os cabeçalhos de cache de duas maneiras.

1. **Configuração do vhost do Dispatcher:** Disponível somente para publicação no AEM.
1. **Código Java™ personalizado:** Disponível para publicação no AEM e para Autor.

Vamos analisar cada uma dessas opções.

### Configuração do vhost do Dispatcher

Essa opção é a abordagem recomendada para ativar o armazenamento em cache, no entanto, só está disponível para publicação no AEM. Para atualizar os cabeçalhos de cache, use o `mod_headers` módulo e `<LocationMatch>` no arquivo vhost do Apache HTTP Server. A sintaxe geral é a seguinte:

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

A seguir, é apresentado um resumo da finalidade de cada **cabeçalho** e aplicável **atributos** para o cabeçalho.

|                     | Navegador da Web | CDN | Descrição |
|---------------------|:-----------:|:---------:|:-----------:|
| Controle de cache | ✔ | ✔ | Esse cabeçalho controla a vida útil do navegador da Web e do cache CDN. |
| Surrogate-Control | ✘ | ✔ | Esse cabeçalho controla a vida útil do cache da CDN. |
| Expira em | ✔ | ✔ | Esse cabeçalho controla a vida útil do navegador da Web e do cache CDN. |


- **max-age**: Esse atributo controla o TTL ou o &quot;tempo de vida&quot; do conteúdo da resposta em segundos.
- **stale-while-revalidate**: Esse atributo controla o _estado obsoleto_ o tratamento do conteúdo da resposta na camada CDN quando a solicitação recebida se dá dentro do período especificado em segundos. A variável _estado obsoleto_ é o período de tempo após a expiração do TTL e antes da revalidação da resposta.
- **stale-if-error**: Esse atributo controla o _estado obsoleto_ tratamento do conteúdo da resposta na camada CDN quando o servidor de origem estiver indisponível e a solicitação recebida estiver dentro do período especificado em segundos.

Revise o [desatualização e revalidação](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) detalhes para obter mais informações.

#### Exemplo

Para aumentar a vida útil do navegador da Web e do cache da CDN do **tipo de conteúdo HTML** para _10 minutos_ sem o tratamento de estado obsoleto, siga estas etapas:

1. No projeto AEM, localize o arquivo vhsot desejado em `dispatcher/src/conf.d/available_vhosts` diretório.
1. Atualizar o vhost (por exemplo, `wknd.vhost`) da seguinte forma:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   Os arquivos vhost em `dispatcher/src/conf.d/enabled_vhosts` diretório são **symlinks** aos arquivos em `dispatcher/src/conf.d/available_vhosts` diretório, portanto, crie symlinks se não estiver presente.
1. Implante as alterações do vhost no ambiente as a Cloud Service do AEM desejado usando o [Cloud Manager - Pipeline de configuração no nível da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) ou [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

No entanto, para ter valores diferentes para a vida útil do navegador da Web e do cache de CDN, você pode usar o `Surrogate-Control` no exemplo acima. Da mesma forma que para expirar o cache em uma data e hora específicas, você pode usar o `Expires` cabeçalho. Além disso, usando o `stale-while-revalidate` e `stale-if-error` atributos, é possível controlar o tratamento de estado obsoleto do conteúdo da resposta. O projeto AEM WKND tem um [tratamento de estado obsoleto de referência](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) Configuração do cache do CDN.

Da mesma forma, também é possível atualizar os cabeçalhos de cache para outros tipos de conteúdo (JSON, JS, CSS e Assets).

### Código Java™ personalizado

Essa opção está disponível para publicação no AEM e para Autor. No entanto, não é recomendável ativar o armazenamento em cache no AEM Author e manter o comportamento padrão do armazenamento em cache.

Para atualizar os cabeçalhos de cache, use o `HttpServletResponse` no código Java™ personalizado (servlet Sling, filtro de servlet Sling). A sintaxe geral é a seguinte:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
