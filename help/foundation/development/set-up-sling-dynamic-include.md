---
title: Configurar inclusão dinâmica do Sling para AEM
description: Uma apresentação em vídeo sobre como instalar e usar o Apache Sling Dynamic Include com AEM Dispatcher em execução no Apache HTTP Web Server.
version: 6.3, 6.4, 6.5
sub-product: fundação, sites
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 3%

---


# Configurar [!DNL Sling Dynamic Include]

Uma apresentação de vídeo sobre como instalar e usar [!DNL Apache Sling Dynamic Include] com [AEM Dispatcher](https://docs.adobe.com/content/help/pt-BR/experience-manager-dispatcher/using/dispatcher.html) em execução em [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Verifique se a versão mais recente do AEM Dispatcher está instalada localmente.

1. Baixe e instale o [[!DNL Sling Dynamic Include] pacote](https://sling.apache.org/downloads.cgi).
1. Configure [!DNL Sling Dynamic Include] via [!DNL OSGi Configuration Factory] em **http://&lt;host>:&lt;porta>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Ou, para adicionar a uma base de código AEM, crie o nó **sling:OsgiConfig** apropriado em:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Opcional) Repita a última etapa para permitir que os componentes em [conteúdo bloqueado (inicial) de modelos editáveis](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) também sejam fornecidos por [!DNL SDI]. A razão para a configuração adicional é que o conteúdo bloqueado de modelos editáveis é servido de `/conf` em vez de `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. Atualize o arquivo [!DNL Apache HTTPD Web server] de `httpd.conf` para habilitar o módulo [!DNL Include].

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Atualize o arquivo [!DNL vhost] para respeitar as diretivas de inclusão.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Atualize o arquivo dispatcher.any configuration para suportar (1) seletores `nocache` e (2) habilitar o suporte a TTL.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Deixar o `*` à direita na regra de `*.nocache.html*` global acima pode resultar em [problemas em solicitações de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Sempre reinicie [!DNL Apache HTTP Web Server] depois de fazer alterações nos arquivos de configuração ou `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Se você estiver usando [!DNL Sling Dynamic Includes] para servir as inclusões de borda (ESI), certifique-se de colocar em cache os cabeçalhos de [resposta relevantes no cache do dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Os cabeçalhos possíveis incluem:
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Tipo de conteúdo&quot;
>* &quot;Expira em&quot;
>* &quot;Última modificação&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Última modificação&quot;

>



## Materiais de suporte

* [Baixar conjunto de inclusão dinâmica do Sling](https://sling.apache.org/downloads.cgi)
* [Documentação de inclusão dinâmica do Apache Sling](https://github.com/Cognifide/Sling-Dynamic-Include)
