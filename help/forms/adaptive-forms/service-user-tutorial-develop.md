---
title: Desenvolvimento com usuários de serviços no AEM Forms
seo-title: Desenvolvimento com usuários de serviços no AEM Forms
description: Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms
seo-description: Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Desenvolvimento com usuários de serviços no AEM Forms

Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms

Em versões anteriores do Adobe Experience Manager (AEM), o resolvedor de recursos administrativos era usado para processamento back-end, que exigia acesso ao repositório. O uso do resolvedor de recursos administrativos está obsoleto no AEM 6.3. Em vez disso, um usuário do sistema com permissões específicas no repositório é usado.

Este artigo percorre a criação de um usuário do sistema e configura as propriedades do mapeador de usuário.

1. Navegue até [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Fazer logon como &#39; admin &#39;
1. Clique em &#39; Administração do usuário &#39;
1. Clique em &#39; Criar usuário do sistema &#39;
1. Defina o tipo de usuário como &#39; data &#39; e clique no ícone verde para concluir o processo de criação do usuário do sistema
1. [Abrir configMgr](http://localhost:4502/system/console/configMgr)
1. Procure &#39; Serviço Mapeador de Usuário do Serviço Apache Sling &#39; e clique para abrir as propriedades
1. Clique no ícone *+* (mais) para adicionar o seguinte Mapeamento de serviço

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Clique em &#39; Salvar &#39;

Na configuração acima, DevelopingWithServiceUser.core é o nome simbólico do conjunto. getresouresolver é o nome do subserviço.data é o usuário do sistema criado na etapa anterior.

Também podemos obter o resolvedor de recursos em nome do usuário do fd-service. Este usuário de serviço é usado para serviços de documento. Por exemplo, se você deseja Certificar/Aplicar direitos de uso etc, usaremos o resolvedor de recursos do usuário fd-service para executar as operações

1. [Baixe e descompacte o arquivo zip associado a este artigo.](assets/developingwithserviceuser.zip)
1. Navegue até [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Carregar e start o pacote OSGi
1. Verifique se o pacote está no estado ativo
1. Agora você criou com êxito um Usuário *do* Sistema e também implantou o pacote *Usuário do* Serviço.

   Para fornecer acesso a /content, atribua ao usuário do sistema (&#39; dados &#39;) permissões de leitura no nó de conteúdo.

   1. Navegue até [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Procure os dados do usuário &#39; &#39;. Este é o mesmo usuário do sistema que você criou na etapa anterior.
   1. Duplo clique no usuário e, em seguida, clique na guia &#39; Permissões &#39;
   1. Atribua a &#39; read &#39; acesso à pasta &#39;content&#39;.
   1. Para usar o usuário do serviço para obter acesso à pasta /content, use o seguinte código

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Se quiser acessar o arquivo /content/dam/data.json em seu pacote, você usará o seguinte código. Este código supõe que você tenha concedido permissões de leitura para o usuário &quot;data&quot; no nó /content/dam/

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   O código completo da implementação é apresentado a seguir

   ```java
   package com.mergeandfuse.getserviceuserresolver.impl;
   
   import java.util.HashMap;
   
   import org.apache.sling.api.resource.LoginException;
   import org.apache.sling.api.resource.ResourceResolver;
   import org.apache.sling.api.resource.ResourceResolverFactory;
   import org.osgi.service.component.annotations.Component;
   import org.osgi.service.component.annotations.Reference;
   import com.mergeandfuse.getserviceuserresolver.GetResolver;
   
   @Component(service = GetResolver.class)
   public class GetResolverImpl implements GetResolver {
    @Reference
    ResourceResolverFactory resolverFactory;
    @Override
    public ResourceResolver getServiceResolver() {
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

