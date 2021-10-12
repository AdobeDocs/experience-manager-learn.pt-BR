---
title: Desenvolvimento com usuários de serviço no AEM Forms
description: Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
source-git-commit: f1afccdad8d819604c510421204f59e7b3dc68e4
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---

# Desenvolvimento com usuários de serviço no AEM Forms

Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms

Em versões anteriores do Adobe Experience Manager (AEM), o resolvedor de recursos administrativos era usado para processamento de back-end que exigia acesso ao repositório. O uso do resolvedor de recursos administrativos está obsoleto no AEM 6.3. Em vez disso, um usuário do sistema com permissões específicas no repositório é usado.

Saiba mais sobre os detalhes de [criar e usar usuários de serviço no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

Este artigo aborda a criação de um usuário do sistema e a configuração das propriedades do mapeador de usuários.

1. Navegue até [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Efetuar logon como &#39; admin &#39;
1. Clique em &quot;Administração de usuário&quot;
1. Clique em &quot;Criar usuário do sistema&quot;
1. Defina o tipo de usuário como &#39; data &#39; e clique no ícone verde para concluir o processo de criação do usuário do sistema
1. [Abrir configMgr](http://localhost:4502/system/console/configMgr)
1. Procure por &#39; Serviço Mapeador de Usuário do Apache Sling Service &#39; e clique para abrir as propriedades
1. Clique no ícone *+* (mais) para adicionar o seguinte Mapeamento de serviços

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Clique em &#39; Salvar &#39;

Na configuração acima, DevelopingWithServiceUser.core é o nome simbólico do pacote. getresourceresolver é o nome do subserviço.data é o usuário do sistema criado na etapa anterior.

Também podemos obter o resolvedor de recursos em nome do usuário do fd-service. Esse usuário de serviço é usado para serviços de documento. Por exemplo, se você deseja Certificar/Aplicar direitos de uso etc, usaremos o resolvedor de recursos do usuário do fd-service para executar as operações

1. [Baixe e descompacte o arquivo zip associado a este artigo.](assets/developingwithserviceuser.zip)
1. Navegue até [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Faça o upload e inicie o pacote OSGi
1. Verifique se o pacote está no estado ativo
1. Agora você criou com êxito um *Usuário do Sistema* e também implantou o *Pacote de Usuário do Serviço*.

   Para fornecer acesso a /content, forneça ao usuário do sistema (&#39; data &#39;) permissões de leitura no nó de conteúdo.

   1. Navegue até [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Procure os dados do usuário &#39; &#39;. Esse é o mesmo usuário do sistema criado na etapa anterior.
   1. Clique duas vezes no usuário e, em seguida, clique na guia &#39; Permissões &#39;
   1. Conceder acesso de leitura à pasta &#39;conteúdo&#39;.
   1. Para usar o usuário de serviço para obter acesso à pasta /content, use o seguinte código



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

Se quiser acessar o arquivo /content/dam/data.json no seu pacote, use o seguinte código. Esse código supõe que você tenha dado permissões de leitura ao usuário &quot;dados&quot; no nó /content/dam/

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
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
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
