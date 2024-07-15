---
title: Criar Perfil De Campanha Usando Modelo De Dados De Formulário
description: Etapas envolvidas na criação de perfil do Adobe Campaign Standard usando o Modelo de dados do formulário do AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 110
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 3%

---

# Criar Perfil De Campanha Usando Modelo De Dados De Formulário {#create-campaign-profile-using-form-data-model}

Etapas envolvidas na criação de perfil do Adobe Campaign Standard usando o Modelo de dados do formulário do AEM Forms

## Criar autenticação personalizada {#create-custom-authentication}

Ao criar o Data Source com o arquivo swagger, o AEM Forms é compatível com os seguintes tipos de autenticação

* Nenhum
* OAuth 2.0
* Autenticação básica
* Chave da API
* Autenticação personalizada

![campaingfdm](assets/campaignfdm.gif)

Teremos que usar uma autenticação personalizada para fazer chamadas REST para o Adobe Campaign Standard.

Para usar a autenticação personalizada, teremos que desenvolver um componente OSGi que implementa a interface IAuthentication

O método getAuthDetails precisa ser implementado. Este método retornará o objeto AuthenticationDetails. Esse objeto AuthenticationDetails terá os cabeçalhos HTTP necessários definidos para fazer a chamada da API REST para o Adobe Campaign.

Veja a seguir o código que foi usado na criação da autenticação personalizada. O método getAuthDetails faz todo o trabalho. Criamos o objeto AuthenticationDetails. Em seguida, adicionamos os HttpHeaders apropriados a esse objeto e o retornamos.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## Criar Source de dados {#create-data-source}

A primeira etapa é criar o arquivo swagger. O arquivo swagger define a API REST que será usada para criar um perfil no Adobe Campaign Standard. O arquivo swagger define os parâmetros de entrada e os parâmetros de saída da API REST.

Uma fonte de dados é criada usando o arquivo swagger. Ao criar a Data Source, é possível especificar o tipo de autenticação. Nesse caso, usaremos autenticação personalizada para autenticar no Adobe Campaign. O código listado acima foi usado para autenticar no Adobe Campaign.

O arquivo Swagger de amostra é fornecido como parte das informações do ativo relacionadas a este artigo.**Altere o host e o basePath no arquivo swagger para corresponder à instância do ACS**

## Testar a solução {#test-the-solution}

Para testar a solução, siga as seguintes etapas:
* [Siga as etapas descritas aqui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Baixe e descompacte este arquivo para obter o arquivo swagger](assets/create-acs-profile-swagger-file.zip)
* Criar Data Source usando o arquivo swagger
Criar o modelo de dados de formulário e baseá-lo no Data Source criado na etapa anterior
* Crie um Formulário adaptável com base no Modelo de dados de formulário criado na etapa anterior.
* Arraste e solte os seguintes elementos da guia fontes de dados no Formulário adaptável

   * Email
   * Nome
   * Sobrenome
   * Celular

* Configure a ação de envio para &quot;Enviar usando o Modelo de dados de formulário&quot;.
* Configure o Modelo de dados para enviar adequadamente.
* Visualize o formulário. Preencha os campos e envie.
* Verifique se o perfil foi criado no Adobe Campaign Standard.
