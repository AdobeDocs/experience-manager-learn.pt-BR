---
title: Como desativar o armazenamento em cache do CDN
description: Saiba como desativar o armazenamento em cache de respostas HTTP no CDN do AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---


# Como desativar o armazenamento em cache do CDN

Saiba como desativar o armazenamento em cache de respostas HTTP no CDN do AEM as a Cloud Service. O armazenamento em cache de respostas é controlado pela `Cache-Control`, `Surrogate-Control`ou `Expires` Cabeçalhos de cache de resposta HTTP.

Esses cabeçalhos de cache normalmente são definidos em configurações de vhost do Dispatcher do AEM usando `mod_headers`, mas também pode ser definido no código Java™ personalizado em execução no próprio AEM Publish.

## Comportamento de cache padrão

Revise o comportamento padrão de armazenamento em cache para publicação e autor do AEM quando um [Arquétipo de projeto AEM](./enable-caching.md#default-caching-behavior) com base no AEM for implantado.

## Desativar armazenamento em cache

Desativar o armazenamento em cache pode ter um impacto negativo no desempenho da instância do AEM as a Cloud Service, portanto, tenha cuidado ao desativar o comportamento padrão de armazenamento em cache.

No entanto, há alguns cenários em que você pode desejar desativar o armazenamento em cache, como:

- Desenvolver um novo recurso e desejar ver as alterações imediatamente.
- O conteúdo é seguro (destinado apenas a usuários autenticados) ou dinâmico (carrinho de compras, detalhes do pedido) e não deve ser armazenado em cache.

Para desativar o armazenamento em cache, você pode atualizar os cabeçalhos de cache de duas maneiras.

1. **Configuração do vhost do Dispatcher:** Disponível somente para publicação no AEM.
1. **Código Java™ personalizado:** Disponível para publicação no AEM e para Autor.

Vamos analisar cada uma dessas opções.

### Configuração do vhost do Dispatcher

Essa opção é a abordagem recomendada para desabilitar o armazenamento em cache, no entanto, só está disponível para publicação no AEM. Para atualizar os cabeçalhos de cache, use o `mod_headers` módulo e `<LocationMatch>` no arquivo vhost do Apache HTTP Server. A sintaxe geral é a seguinte:

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Remove o cabeçalho de resposta desse nome, se existir. Se houver vários cabeçalhos com o mesmo nome, todos serão removidos.
    Cabeçalho não definido Cache-Controle
    O cabeçalho não definido expira em
    
    # Instrui a CDN a não armazenar a resposta em cache.
    Cabeçalho definido Controle de cache &quot;privado&quot;
    &lt;/locationmatch>
    &quot;

#### Exemplo

Para desativar o armazenamento em cache CDN do **Tipos de conteúdo CSS** para alguns fins de solução de problemas, siga estas etapas.

Observe que, para ignorar o cache CSS existente, uma alteração no arquivo CSS é necessária para gerar uma nova chave de cache para o arquivo CSS.

1. No projeto AEM, localize o arquivo vhsot desejado em `dispatcher/src/conf.d/available_vhosts` diretório.
1. Atualizar o vhost (por exemplo, `wknd.vhost`) da seguinte forma:

       &quot;conf
       &lt;locationmatch etc.clientlibs=&quot;&quot;>*.(css)$&quot;>
       # Remove o cabeçalho de resposta desse nome, se existir. Se houver vários cabeçalhos com o mesmo nome, todos serão removidos.
       Cabeçalho não definido Cache-Controle
       O cabeçalho não definido expira em
       
       # Instrui a CDN a não armazenar a resposta em cache.
       Cabeçalho definido Controle de cache &quot;privado&quot;
       &lt;/locationmatch>
       &quot;
   Os arquivos vhost em `dispatcher/src/conf.d/enabled_vhosts` diretório são **symlinks** aos arquivos em `dispatcher/src/conf.d/available_vhosts` diretório, portanto, crie symlinks se não estiver presente.
1. Implante as alterações do vhost no ambiente as a Cloud Service do AEM desejado usando o [Cloud Manager - Pipeline de configuração no nível da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) ou [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Código Java™ personalizado

Essa opção está disponível para publicação no AEM e para Autor. Para atualizar os cabeçalhos de cache, use o `SlingHttpServletResponse` no código Java™ personalizado (servlet Sling, filtro de servlet Sling). A sintaxe geral é a seguinte:

    &quot;java
    response.setHeader(&quot;Cache-Control&quot;, &quot;private&quot;);
    &quot;
