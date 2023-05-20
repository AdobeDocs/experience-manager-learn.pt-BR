---
title: AEM Forms com Marketo (Parte 2)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados do formulário do AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 2%

---

# Serviço de autenticação da Marketo

As REST APIs do Marketo são autenticadas com OAuth 2.0 de duas pernas. Precisamos criar uma autenticação personalizada para autenticar no Marketo. Normalmente, essa autenticação personalizada é gravada em um pacote OSGI. O código a seguir mostra o autenticador personalizado que foi usado como parte deste tutorial.

## Serviço de autenticação personalizado

O código a seguir cria o objeto AuthenticationDetails que tem o access_token necessário para autenticação no Marketo

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

O MarketoAuthenticationService implementa a interface IAuthentication. Essa interface faz parte do AEM Forms Client SDK. O serviço obtém o token de acesso e o insere no HttpHeader do AuthenticationDetails. Depois que o HttpHeaders do objeto AuthenticationDetails é preenchido, o objeto AuthenticationDetails é retornado para a camada Dermis do modelo de dados do formulário.

Preste atenção à sequência de caracteres retornada pelo método getAuthenticationType. Essa cadeia de caracteres é usada quando você está configurando sua fonte de dados.

### Obter token de acesso

Uma interface simples é definida com um método que retorna o access_token. O código da classe que implementa essa interface está listado mais abaixo na página.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

O código a seguir é do serviço que retorna o access_token que deve ser usado para fazer as chamadas de API REST. O código neste serviço acessa os parâmetros de configuração necessários para fazer a chamada do GET. Como você pode ver, passamos a client_id,client_secret no URL do GET para gerar o access_token. Esse access_token é retornado ao aplicativo de chamada.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

A captura de tela abaixo mostra as propriedades de configuração que precisam ser definidas. Essas propriedades de configuração são lidas no código listado acima para obter o access_token

![config](assets/configuration-settings.png)

### Configuração

O código a seguir foi usado para criar as propriedades de configuração. Essas propriedades são específicas da sua instância do Marketo

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

O código a seguir lê as propriedades de configuração e retorna o mesmo por meio dos métodos Getter

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. Crie e implante o pacote no servidor AEM.
1. [Aponte seu navegador para configMgr](http://localhost:4502/system/console/configMgr) e procure por &quot;Marketo Credentials Service Configuration&quot;
1. Especifique as propriedades apropriadas específicas para sua instância do Marketo

## Próximas etapas

[Criar fonte de dados baseada em serviço RESTful](./part3.md)

