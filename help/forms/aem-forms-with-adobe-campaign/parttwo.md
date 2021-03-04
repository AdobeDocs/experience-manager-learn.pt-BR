---
title: Criação do perfil da campanha no envio do formulário adaptável
seo-title: Criação do perfil da campanha no envio do formulário adaptável
description: Este artigo explicará as etapas necessárias para criar um perfil no Adobe Campaign Standard em um envio de formulário adaptável. Esse processo utiliza o mecanismo de envio personalizado para lidar com o envio do formulário adaptativo.
seo-description: Este artigo explicará as etapas necessárias para criar um perfil no Adobe Campaign Standard em um envio de formulário adaptável. Esse processo utiliza o mecanismo de envio personalizado para lidar com o envio do formulário adaptativo.
uuid: f3cb7b3c-1a1c-49eb-9447-a9e52c675244
feature: '"Formulários adaptáveis, Modelo de dados de formulário"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 46ec4136-4898-4b01-86bb-ac638a29b242
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---


# Criação do perfil de campanha no envio do formulário adaptável {#creating-campaign-profile-on-adaptive-form-submission}

Este artigo explicará as etapas necessárias para criar um perfil no Adobe Campaign Standard em um envio de formulário adaptável. Esse processo utiliza o mecanismo de envio personalizado para lidar com o envio do formulário adaptativo.

Este tutorial percorrerá as etapas de criação do perfil do Campaign no envio adaptável de formulários. Para realizar esse caso de uso, precisamos fazer o seguinte

* Crie um AEM Service (CampaignService) para criar o Perfil do Adobe Campaign Standard usando a REST API
* Criar uma ação de envio personalizada para lidar com o envio do formulário adaptável
* Chame o método createProfile do CampaignService

## Criar AEM Service {#create-aem-service}

Crie o AEM Service para criar um Perfil do Adobe Campaign. Esse serviço do AEM buscará as credenciais do Adobe Campaign na configuração do OSGI. Depois que as credenciais da campanha são obtidas, o token de acesso é gerado e o uso do token de acesso HTTP Post é feito para criar o perfil no Adobe Campaign. Este é o código para criação de perfil .

```java
package aemformwithcampaign.core.services.impl;

import static io.jsonwebtoken.SignatureAlgorithm.RS256;

import java.io.BufferedInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.KeySpec;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

import org.apache.http.Consts;
import org.apache.http.HttpEntity;
import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
import io.jsonwebtoken.Jwts;

@Component(service = CampaignService.class, immediate = true)
public class CampaignServiceImpl implements CampaignService {
private final Logger log = LoggerFactory.getLogger(getClass());

@Reference
CampaignConfigurationService config;
@Reference
GetResolver getResolver;
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String AUTH_SERVER_FQDN = "ims-na1.adobelogin.com";
private static final String AUTH_ENDPOINT = "/ims/exchange/jwt/";
private static final String CREATE_PROFILE_ENDPOINT = "/campaign/profileAndServicesExt/profile/";

@SuppressWarnings("unused")
@Override
public String getAccessToken() throws IOException, NoSuchAlgorithmException, InvalidKeySpecException {
// TODO Auto-generated method stub
log.info("JWT: Generating Token");

String apikey = config.getApiKey();
log.debug("The API Key i got was " + apikey);
String techact = config.getTechAcct();
String orgid = config.getOrgId();
String clientsecret = config.getClientSecret();
String realm = config.getDomainRealm();

HttpClient httpClient = HttpClientBuilder.create().build();
Long expirationTime = System.currentTimeMillis() / 1000 + 86400L;
try {
ResourceResolver rr = getResolver.getServiceResolver();
Resource privateKeyRes = rr.getResource("/etc/key/campaign/private.key");
InputStream is = privateKeyRes.adaptTo(InputStream.class);
BufferedInputStream bis = new BufferedInputStream(is);
ByteArrayOutputStream buf = new ByteArrayOutputStream();
int result = bis.read();
while (result != -1) {
byte b = (byte) result;
buf.write(b);
result = bis.read();
}

String privatekeyString = buf.toString();
privatekeyString = privatekeyString.replaceAll("\\n", "").replace("-----BEGIN PRIVATE KEY-----", "")
.replace("-----END PRIVATE KEY-----", "");
log.debug("The sanitized private key string is " + privatekeyString);
// Create the private key
KeyFactory keyFactory = KeyFactory.getInstance("RSA");
log.debug("The key factory algorithm is " + keyFactory.getAlgorithm());
byte[] byteArray = privatekeyString.getBytes();
log.debug("The array length is " + byteArray.length);
// KeySpec ks = new
// PKCS8EncodedKeySpec(Base64.getDecoder().decode(keyString));
byte[] encodedBytes = javax.xml.bind.DatatypeConverter.parseBase64Binary(privatekeyString);
// KeySpec ks = new PKCS8EncodedKeySpec(byteArray);
KeySpec ks = new PKCS8EncodedKeySpec(encodedBytes);
String metascopes[] = new String[] { "ent_campaign_sdk" };
RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(ks);
HashMap<String, Object> jwtClaims = new HashMap<>();
jwtClaims.put("iss", orgid);
jwtClaims.put("sub", techact);
jwtClaims.put("exp", expirationTime);
jwtClaims.put("aud", "https://" + AUTH_SERVER_FQDN + "/c/" + apikey);
// jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + realm,
// true);
for (String metascope : metascopes) {
jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + metascope, java.lang.Boolean.TRUE);
}

// Create the final JWT token
String jwtToken = Jwts.builder().setClaims(jwtClaims).signWith(RS256, privateKey).compact();
log.debug("#####The jwtToken is  ####" + jwtToken + "#######");
HttpHost authServer = new HttpHost(AUTH_SERVER_FQDN, 443, "https");
HttpPost authPostRequest = new HttpPost(AUTH_ENDPOINT);
authPostRequest.addHeader("Cache-Control", "no-cache");
List<NameValuePair> params = new ArrayList<NameValuePair>();
params.add(new BasicNameValuePair("client_id", apikey));
params.add(new BasicNameValuePair("client_secret", clientsecret));
params.add(new BasicNameValuePair("jwt_token", jwtToken));
authPostRequest.setEntity(new UrlEncodedFormEntity(params, Consts.UTF_8));
HttpResponse response = httpClient.execute(authServer, authPostRequest);
if (200 != response.getStatusLine().getStatusCode()) {
HttpEntity ent = response.getEntity();
String content = EntityUtils.toString(ent);
log.error("JWT: Server Returned Error\n", response.getStatusLine().getReasonPhrase());
log.error("ERROR DETAILS: \n", content);
throw new IOException("Server returned error: " + response.getStatusLine().getReasonPhrase());
}
HttpEntity entity = response.getEntity();
JsonObject jo = new JsonParser().parse(EntityUtils.toString(entity)).getAsJsonObject();

log.debug("Returning access_token " + jo.get("access_token").getAsString());
return jo.get("access_token").getAsString();


} catch (Exception e) {
e.printStackTrace();
}
return null;
}

@Override
public String createProfile(JsonObject profile) {
// TODO Auto-generated method stub
String jwtToken = null;
try {
jwtToken = getAccessToken();
} catch (NoSuchAlgorithmException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (InvalidKeySpecException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (IOException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
}
String tenant = config.getTenant();
String apikey = config.getApiKey();
String path = "/" + tenant + CREATE_PROFILE_ENDPOINT;
log.debug("The API Key is " + apikey);
log.debug("###The Path is " + path);
HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
HttpPost postReq = new HttpPost(path);
postReq.addHeader("Cache-Control", "no-cache");
postReq.addHeader("Content-Type", "application/json");
postReq.addHeader("X-Api-Key", apikey);
postReq.addHeader("Authorization", "Bearer " + jwtToken);
StringEntity se = null;
log.debug("Creating profile for" + profile.toString());
try {
se = new StringEntity(profile.toString());

} catch (UnsupportedEncodingException e1) {
// TODO Auto-generated catch block
e1.printStackTrace();
}
postReq.setEntity(se);
HttpClient httpClient = HttpClientBuilder.create().build();
HttpResponse result;
try {
result = httpClient.execute(server, postReq);
// JSONObject responseJson = new
// JSONObject(EntityUtils.toString(result.getEntity()));
JsonObject responseJson = new JsonParser().parse(EntityUtils.toString(result.getEntity()))
.getAsJsonObject();
log.debug("The response on creating profile is " + responseJson.toString());
return responseJson.get("PKey").getAsString();
// return responseJson.getString("PKey");

} catch (ClientProtocolException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (IOException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

}
```

## Envio personalizado {#custom-submit}

Crie um manipulador de envio personalizado para lidar com o envio do Formulário adaptativo. Neste manipulador de envio personalizado, faremos uma chamada para o método createProfile do CampaignService. O método createProfile aceita um JSONObject que representa o perfil que precisa ser criado.

Para saber mais sobre o manipulador de envio personalizado no AEM Forms, siga este [link](/help/forms/adaptive-forms/custom-submit-aem-forms-article.md)

Este é o código no envio personalizado

```java
aemforms.campaign.core.CampaignService addNewProfile = sling.getService(aemforms.campaign.core.CampaignService.class);
com.google.gson.JsonObject profile = new com.google.gson.JsonObject();
profile.addProperty("email",request.getParameter("email"));
profile.addProperty("firstName",request.getParameter("fname"));
profile.addProperty("lastName",request.getParameter("lname"));
profile.addProperty("mobilePhone",request.getParameter("phone"));

String pkey = addNewProfile.createProfile(profile);
```

## Testar a solução {#test-the-solution}

Depois de definir o serviço e a ação de envio personalizado, estamos prontos para testar nossa solução. Para testar a solução, execute as seguintes etapas


* [Siga as etapas conforme descrito aqui](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Importe o Adaptive Form e o Custom Submit Handler usando o gerenciador](assets/create-acs-profile-on-af-submission.zip) de pacotes. Este pacote contém o Formulário Adaptável configurado para enviar para a ação de envio personalizada.
* Visualize o [formulário](http://localhost:4502/content/dam/formsanddocuments/createcampaignprofile/jcr:content?wcmmode=disabled)
* Preencha todos os campos e envie
* Um novo perfil será criado na instância do ACS
