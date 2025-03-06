---
title: Uso da API de direitos de uso
description: Exemplo de código para aplicar direitos de uso ao PDF fornecido
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
source-wordcount: '280'
ht-degree: 1%

---

# Fazer chamada de API

## Aplicar direitos de uso

Depois de receber o token de acesso, a próxima etapa é fazer uma solicitação de API para aplicar direitos de uso à PDF especificada. Isso envolve a inclusão do token de acesso no cabeçalho da solicitação para autenticar a chamada, garantindo o processamento seguro e autorizado do documento.

A seguinte função aplica os direitos de uso

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## Detalhamento de função:



* **Configurar Carga e Ponto de Extremidade da API**
   * Constrói a URL da API usando o `endPoint` fornecido e um `BUCKET` predefinido.
   * Define uma cadeia de caracteres JSON (`usageRights`) especificando os direitos a serem aplicados, como:
      * Comentários
      * Arquivos incorporados
      * Preenchimento de formulário
      * Exportação de dados do formulário

* **Carregar o arquivo do PDF**
   * Recupera o arquivo `withoutusagerights.pdf` do diretório `pdffiles`.
   * Registra um erro e sai se o arquivo não for encontrado.

* **Preparar a Solicitação HTTP**
   * Lê o arquivo PDF em uma matriz de bytes.
   * Usa `MultipartEntityBuilder` para criar uma solicitação de várias partes contendo:
      * O arquivo PDF como um corpo binário.
      * O JSON `usageRights` como um corpo de texto.
   * Configura uma solicitação HTTP `POST` com cabeçalhos:
      * `Authorization: Bearer <accessToken>` para autenticação.
      * `X-Adobe-Accept-Experimental: 1` (possivelmente necessário para compatibilidade de API).

* **Enviar a Solicitação e Manipular a Resposta**
   * Executa a solicitação HTTP usando `httpClient.execute(httpPost)`.
   * Lê a resposta (espera-se que seja a PDF atualizada com direitos de uso aplicados).
   * Grava o conteúdo PDF recebido em **&quot;ReaderExtended.pdf&quot;** em `SAVE_LOCATION`.

* **Manipulação e Limpeza de Erros**
   * Coleta e registra quaisquer erros `IOException`.
   * Garante que todos os recursos (fluxos, cliente HTTP, resposta) sejam fechados corretamente no bloco `finally`.

A seguir está o código main.java que invoca a função applyUsageRights

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

O método `main` é inicializado chamando `getAccessToken()` de `AccessTokenService`, que deve retornar um token válido.

* Em seguida, chama `applyUsageRights()` da classe `DocumentGeneration`, transmitindo:
   * O `accessToken` recuperado
   * O endpoint da API para aplicar direitos de uso.


## Próximas etapas

[Implante o projeto de amostra](sample-project.md)
