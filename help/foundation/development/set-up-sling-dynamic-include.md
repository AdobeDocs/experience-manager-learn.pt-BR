---
title: Configurar a inclusão dinâmica do Sling para AEM
description: Uma apresentação em vídeo da instalação e uso do Apache Sling Dynamic Include com AEM Dispatcher em execução no Apache HTTP Web Server.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Configurar [!DNL Sling Dynamic Include]

Uma apresentação em vídeo da instalação e utilização do [!DNL Apache Sling Dynamic Include] com [Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=pt-BR) em execução [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Verifique se a versão mais recente do AEM Dispatcher está instalada localmente.

1. Baixe e instale o [[!DNL Sling Dynamic Include] pacote](https://sling.apache.org/downloads.cgi).
1. Configurar [!DNL Sling Dynamic Include] por meio da [!DNL OSGi Configuration Factory] em **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Ou, para adicionar a uma base de código AEM, crie a variável **sling:OsgiConfig** nó em:

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

1. (Opcional) Repita a última etapa para permitir componentes em [conteúdo bloqueado (inicial) de modelos editáveis](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) a ser distribuído via [!DNL SDI] também. O motivo para a configuração adicional é que o conteúdo bloqueado dos modelos editáveis é exibido no `/conf` em vez de `/content`.

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

1. Atualizar [!DNL Apache HTTPD Web server]do `httpd.conf` arquivo para habilitar o [!DNL Include] módulo.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Atualize o [!DNL vhost] arquivo a ser respeitado inclui diretivas.

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

1. Atualizar o arquivo de configuração dispatcher.any para suporte (1) `nocache` seletores e (2) ative o suporte TTL.

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
   > Sair da trilha `*` desativado no glob `*.nocache.html*` regra acima, pode resultar em [problemas nas solicitações de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Sempre reiniciar [!DNL Apache HTTP Web Server] depois de fazer alterações em seus arquivos de configuração ou no `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Se você estiver usando [!DNL Sling Dynamic Includes] para veicular inclusões de borda (ESI), certifique-se de armazenar em cache informações [cabeçalhos de resposta no cache do dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Os possíveis cabeçalhos incluem o seguinte:
>
>* &quot;Cache-Control&quot;
>* &quot;Disposição de conteúdo&quot;
>* &quot;Content-Type&quot;
>* &quot;Expira&quot;
>* &quot;Última modificação&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Última modificação&quot;
>

## Materiais de suporte

* [Baixar o pacote de inclusão dinâmica do Sling](https://sling.apache.org/downloads.cgi)
* [Documentação de inclusão dinâmica do Apache Sling](https://github.com/Cognifide/Sling-Dynamic-Include)
