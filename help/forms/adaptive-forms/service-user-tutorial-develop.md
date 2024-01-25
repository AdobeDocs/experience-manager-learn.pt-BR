---
title: Desenvolvimento com usuários de serviço no AEM Forms
description: Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
duration: 159
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Desenvolvimento com usuários de serviço no AEM Forms

Este artigo o orienta pelo processo de criação de um usuário de serviço no AEM Forms

Em versões anteriores do Adobe Experience Manager (AEM), o resolvedor de recursos administrativos era usado para processamento de back-end, o que exigia acesso ao repositório. O uso do resolvedor de recursos administrativos foi descontinuado no AEM 6.3. Em vez disso, é usado um usuário do sistema com permissões específicas no repositório.

Saiba mais sobre os detalhes de [criação e uso de usuários de serviço no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

Este artigo aborda a criação de um usuário do sistema e a configuração das propriedades do mapeador do usuário.

1. Navegue até [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Fazer logon como &quot;administrador&quot;
1. Clique em &quot;Administração de usuários&quot;
1. Clique em &quot;Criar usuário do sistema&quot;
1. Defina o tipo de ID do usuário como &quot;dados&quot; e clique no ícone verde para concluir o processo de criação do usuário do sistema
1. [Abrir configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar por _Serviço Mapeador de usuário do Apache Sling Service_ e clique em para abrir as propriedades
1. Clique em *+* ícone (mais) para adicionar o seguinte Service Mapping

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Clique em &quot;Salvar&quot;

Na configuração acima, DevelopingWithServiceUser.core é o nome simbólico do pacote. getresourceresolver é o nome do subserviço.data é o usuário do sistema criado na etapa anterior.

Também podemos obter o resolvedor de recursos em nome do usuário do serviço fd. Este usuário de serviço é usado para serviços de documento. Por exemplo, se você quiser Certificar/Aplicar direitos de uso, usaremos o resolvedor de recursos do usuário do serviço fd para executar as operações

1. [Baixe e descompacte o arquivo zip associado a este artigo.](assets/developingwithserviceuser.zip)
1. Navegue até [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Carregue e inicie o pacote OSGi
1. Verifique se o pacote está no estado ativo
1. Agora você criou um *Usuário do sistema* e também implantou o *Pacote de usuário do serviço*.

   Para fornecer acesso a /content, forneça ao usuário do sistema (&quot;dados&quot;) permissões de leitura no nó de conteúdo.

   1. Navegue até [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Pesquisar dados do usuário &#39;. É o mesmo usuário do sistema criado na etapa anterior.
   1. Clique duas vezes no usuário e clique na guia &quot;Permissões&quot;
   1. Conceda acesso de leitura à pasta de conteúdo.
   1. Para usar o usuário do serviço para obter acesso à pasta /content, use o seguinte código



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

Se quiser acessar o arquivo /content/dam/data.json em seu pacote, você usará o código a seguir. Esse código supõe que você tenha dado permissões de leitura ao usuário de &quot;dados&quot; no nó /content/dam/

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

O código completo da implementação é fornecido abaixo

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
