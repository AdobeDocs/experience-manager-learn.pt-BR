---
title: Configurar Sling Dynamic Include para AEM
description: Um vídeo de apresentação da instalação e uso do Apache Sling Dynamic Include com AEM Dispatcher em execução no Apache HTTP Web Server.
version: 6.3, 6.4, 6.5
sub-product: fundação, sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 5%

---


# Configurar [!DNL Sling Dynamic Include]

Uma apresentação de vídeo sobre como instalar e usar [!DNL Apache Sling Dynamic Include] com [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=pt-BR) em execução em [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Verifique se a versão mais recente do AEM Dispatcher está instalada localmente.

1. Baixe e instale o [[!DNL Sling Dynamic Include] pacote](https://sling.apache.org/downloads.cgi).
1. Configure [!DNL Sling Dynamic Include] através do [!DNL OSGi Configuration Factory] em **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Ou, para adicionar a uma base de código AEM, crie o nó apropriado **sling:OsgiConfig** em:

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

1. (Opcional) Repita a última etapa para permitir que os componentes no conteúdo [bloqueado (inicial) de modelos editáveis](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/page-templates-editable.html) também sejam veiculados via [!DNL SDI]. A razão para a configuração adicional é que o conteúdo bloqueado dos modelos editáveis é veiculado a partir de `/conf` em vez de `/content`.

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

1. Atualize o arquivo `httpd.conf` de [!DNL Apache HTTPD Web server] para habilitar o módulo [!DNL Include].

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

1. Atualize o arquivo de configuração dispatcher.any para suportar (1) `nocache` seletores e (2) habilitar o suporte ao TTL.

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
   > Deixar o `*` à direita na regra glob `*.nocache.html*` acima, pode resultar em [problemas em solicitações de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Sempre reinicie [!DNL Apache HTTP Web Server] após fazer alterações nos arquivos de configuração ou `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Se você estiver usando [!DNL Sling Dynamic Includes] para fornecer inclusões do lado da borda (ESI), certifique-se de armazenar em cache os cabeçalhos [de resposta relevantes no cache do dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Os cabeçalhos possíveis incluem:
>
>* &quot;Cache-Control&quot;
>* &quot;Disposição de conteúdo&quot;
>* &quot;Tipo de conteúdo&quot;
>* &quot;Expira em&quot;
>* &quot;Última modificação&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Última modificação&quot;

>



## Materiais de apoio

* [Baixar o pacote Sling Dynamic Include](https://sling.apache.org/downloads.cgi)
* [Documentação de inclusão dinâmica do Apache Sling](https://github.com/Cognifide/Sling-Dynamic-Include)
