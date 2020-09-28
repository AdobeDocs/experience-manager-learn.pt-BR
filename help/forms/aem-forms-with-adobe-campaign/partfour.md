---
title: Criar Perfil de Campanha usando o modelo de dados de formulário
seo-title: Criar Perfil de Campanha usando o modelo de dados de formulário
description: Etapas envolvidas na criação do Adobe Campaign Standard perfil usando o AEM Forms Form Data Model
seo-description: Etapas envolvidas na criação do Adobe Campaign Standard perfil usando o AEM Forms Form Data Model
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 3%

---


# Criar Perfil de Campanha usando o modelo de dados de formulário {#create-campaign-profile-using-form-data-model}

Etapas envolvidas na criação do Adobe Campaign Standard perfil usando o AEM Forms Form Data Model

## Criar autenticação personalizada {#create-custom-authentication}

Ao criar a Fonte de Dados com o arquivo swagger, a AEM Forms oferece suporte aos seguintes tipos de autenticação

* Nenhum
* OAuth 2.0
* Autenticação básica
* Chave da API
* Autenticação personalizada

![campaignfdm](assets/campaignfdm.gif)

Teremos que usar a autenticação personalizada para fazer chamadas REST para a Adobe Campaign Standard.

Para usar a autenticação personalizada, teremos que desenvolver um componente OSGi que implemente a interface IAuthentication

O método getAuthDetails precisa ser implementado. Este método retornará o objeto AuthenticationDetails. Este objeto AuthenticationDetails terá os cabeçalhos HTTP necessários definidos para fazer a chamada REST API para a Adobe Campaign.

A seguir está o código usado na criação da autenticação personalizada. O método getAuthDetails faz todo o trabalho. Criamos um objeto AuthenticationDetails. Em seguida, adicionamos os HttpHeaders apropriados a esse objeto e retornamos esse objeto.

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

## Criar fonte de dados {#create-data-source}

A primeira etapa é criar o arquivo swagger. O arquivo swagger define a REST API que será usada para criar um perfil no Adobe Campaign Standard. O arquivo swagger define os parâmetros de entrada e de saída da REST API.

Uma fonte de dados é criada usando o arquivo swagger. Ao criar a Fonte de Dados, você pode especificar o tipo de autenticação. Nesse caso, usaremos autenticação personalizada para autenticar no Adobe Campaign. O código listado acima foi usado para autenticação no Adobe Campaign.

O arquivo de amostra swagger é fornecido a você como parte do ativo relacionado a este artigo.**Certifique-se de alterar o host e o basePath no arquivo swagger para que correspondam à sua instância ACS**

## Teste a solução {#test-the-solution}

Para testar a solução, siga as seguintes etapas:
* [Siga as etapas descritas aqui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Baixe e descompacte este arquivo para obter o arquivo swagger](assets/create-acs-profile-swagger-file.zip)
* Criar fonte de dados usando o arquivo swaggerCriar modelo de dados de formulário e baseá-lo na fonte de dados criada na etapa anterior
* Crie um formulário adaptável com base no Modelo de dados de formulário criado na etapa anterior.
* Arraste e solte os seguintes elementos da guia fontes de dados na guia Formulário adaptativo

   * E-mail
   * Nome
   * Sobrenome
   * Telefone celular

* Configure a ação de envio para &quot;Enviar usando o Modelo de dados de formulário&quot;.
* Configure o Modelo de dados para enviar apropriadamente.
* Visualizar o formulário. Preencha os campos e envie.
* Verifique se o perfil foi criado no Adobe Campaign Standard.
