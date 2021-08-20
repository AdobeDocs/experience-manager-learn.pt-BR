---
title: Criar perfil de campanha usando modelo de dados de formulário
description: Etapas envolvidas na criação do perfil do Adobe Campaign Standard usando o Modelo de dados de formulário do AEM Forms
feature: Formulários adaptáveis
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 4%

---


# Criar perfil de campanha usando modelo de dados de formulário {#create-campaign-profile-using-form-data-model}

Etapas envolvidas na criação do perfil do Adobe Campaign Standard usando o Modelo de dados de formulário do AEM Forms

## Criar autenticação personalizada {#create-custom-authentication}

Ao criar a Fonte de Dados com o arquivo swagger, o AEM Forms oferece suporte aos seguintes tipos de autenticação:

* Nenhum
* OAuth 2.0
* Autenticação básica
* Chave da API
* Autenticação personalizada

![campaingfdm](assets/campaignfdm.gif)

Teremos que usar a autenticação personalizada para fazer chamadas REST para o Adobe Campaign Standard.

Para usar a autenticação personalizada, teremos que desenvolver um componente OSGi que implemente a interface de autenticação IAuthentication

O método getAuthDetails precisa ser implementado. Este método retornará o objeto AuthenticationDetails. Este objeto AuthenticationDetails terá o conjunto de cabeçalhos HTTP necessário para fazer a chamada da API REST para o Adobe Campaign.

Este é o código usado na criação da autenticação personalizada. O método getAuthDetails faz todo o trabalho. Criamos o objeto AuthenticationDetails . Em seguida, adicionamos os HttpHeaders apropriados a esse objeto e retornamos esse objeto.

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

O primeiro passo é criar o arquivo swagger. O arquivo swagger define a REST API, que será usada para criar um perfil no Adobe Campaign Standard. O arquivo do swagger define os parâmetros de entrada e os parâmetros de saída da API REST.

Uma fonte de dados é criada usando o arquivo swagger. Ao criar a Fonte de Dados, é possível especificar o tipo de autenticação. Nesse caso, usaremos autenticação personalizada para autenticar em relação ao Adobe Campaign. O código listado acima foi usado para autenticação em relação ao Adobe Campaign.

O arquivo de troca de amostras é fornecido a você como parte do ativo relacionado a este artigo.**Certifique-se de alterar o host e o basePath no arquivo swagger para corresponder à instância do ACS**

## Testar a solução {#test-the-solution}

Para testar a solução, siga as seguintes etapas:
* [Siga as etapas conforme descrito aqui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Baixe e descompacte este arquivo para obter o arquivo do swagger](assets/create-acs-profile-swagger-file.zip)
* Criar fonte de dados usando o arquivo swagger
Criar Modelo de Dados de Formulário e baseá-lo na Fonte de Dados criada na etapa anterior
* Crie um formulário adaptável com base no modelo de dados de formulário criado na etapa anterior.
* Arraste e solte os seguintes elementos da guia fontes de dados no Formulário adaptável

   * E-mail
   * Nome
   * Sobrenome
   * Telefone móvel

* Configure a ação de envio para &quot;Enviar usando o Modelo de dados de formulário&quot;.
* Configure o Modelo de dados para enviar adequadamente.
* Visualizar o formulário. Preencha os campos e envie.
* Verifique se o perfil foi criado no Adobe Campaign Standard.
