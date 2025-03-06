---
title: Gerar token de acesso
description: Exemplo de código para gerar um token de acesso para chamar a API de serviços de documento da Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Gerar token de acesso

Um token de acesso é uma credencial usada para autenticar e autorizar solicitações a uma API REST. Normalmente, é emitido por um servidor de autenticação (como OAuth 2.0 ou OpenID Connect) depois que um usuário ou aplicativo faz logon com êxito. Ao chamar uma API de Serviços de documento Forms segura, um token de acesso é incluído no cabeçalho da solicitação para verificar a identidade do cliente.
Veja a seguir um exemplo de solicitação de envio de um token de acesso

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

O código a seguir foi usado para gerar o token de acesso

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AccessTokenService {
    private static final Logger logger = LoggerFactory.getLogger(AccessTokenService.class);
    
    public String getAccessToken() {
        ClassLoader classLoader = AccessTokenService.class.getClassLoader();

        try (InputStream inputStream = classLoader.getResourceAsStream("credentials/server_credentials.json")) {
            if (inputStream == null) {
                logger.error("File not found: credentials/server_credentials.json");
                throw new IllegalArgumentException("Missing credentials file");
            }

            try (InputStreamReader reader = new InputStreamReader(inputStream, StandardCharsets.UTF_8);
                 CloseableHttpClient httpClient = HttpClients.createDefault()) {
                 
                JsonObject jsonObject = JsonParser.parseReader(reader).getAsJsonObject();
                String clientId = jsonObject.get("clientId").getAsString();
                String clientSecret = jsonObject.get("clientSecret").getAsString();
                String adobeIMSV3TokenEndpointURL = jsonObject.get("adobeIMSV3TokenEndpointURL").getAsString();
                String scopes = jsonObject.get("scopes").getAsString();

                HttpPost request = new HttpPost(adobeIMSV3TokenEndpointURL);
                request.setHeader("Content-Type", "application/x-www-form-urlencoded");

                String requestBody = "grant_type=client_credentials&client_id=" + clientId +
                        "&client_secret=" + clientSecret +
                        "&scope=" + scopes;

                request.setEntity(new StringEntity(requestBody, ContentType.APPLICATION_FORM_URLENCODED));

                logger.info("Requesting access token from {}", adobeIMSV3TokenEndpointURL);

                try (CloseableHttpResponse response = httpClient.execute(request)) {
                    String responseString = EntityUtils.toString(response.getEntity());
                    JsonObject jsonResponse = JsonParser.parseString(responseString).getAsJsonObject();
                    
                    if (!jsonResponse.has("access_token")) {
                        logger.error("Access token not found in response: {}", responseString);
                        return null;
                    }

                    String accessToken = jsonResponse.get("access_token").getAsString();
                    logger.info("Successfully obtained access token.");
                    return accessToken;
                }
            }
        } catch (Exception e) {
            logger.error("Error fetching access token", e);
        }
        return null;
    }
}
```

A classe AccessTokenService é responsável pela recuperação de um token de acesso OAuth do serviço de autenticação IMS da Adobe. Ele lê as credenciais do cliente de um arquivo JSON (server_credentials.json), constrói uma solicitação de autenticação e a envia ao endpoint do token do Adobe. A resposta é então analisada para extrair o token de acesso, que é usado para chamadas seguras de API. A classe inclui registro adequado e tratamento de erros para garantir confiabilidade e depuração mais fácil.

## Próximas etapas

[Fazer chamadas de API](./make-api-calls.md)