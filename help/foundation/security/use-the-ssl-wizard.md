---
title: Use o Assistente SSL em AEM
description: Assistente de configuração SSL da Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução por HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: 835c01cb2ad1d154437087c51c70a2daf90493dd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---

# Use o Assistente SSL em AEM

Assistente de configuração SSL da Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução por HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Para ambientes gerenciados, é melhor para o departamento de TI fornecer certificados e chaves confiáveis pela CA.
>
>Certificados autoassinados só devem ser usados para fins de desenvolvimento.

## Chave privada e download de certificado autoassinado

O zip a seguir contém os arquivos [!DNL DER] e [!DNL CRT] necessários para a configuração AEM SSL no host local e destinados apenas a fins de desenvolvimento local.

Os arquivos [!DNL DER] e [!DNL CERT] são fornecidos para conveniência e gerados usando as etapas descritas na seção Gerar chave privada e certificado autoassinado abaixo.

Se necessário, a frase de passagem do certificado é **admin**.

localhost - chave privada e certificate.zip autoassinado (expira em julho de 2028)

[Baixar o arquivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

## Chave privada e geração de certificado autoassinado

O vídeo acima descreve a configuração do SSL em uma instância do autor de AEM usando certificados autoassinados. Os comandos abaixo usando [[!DNL OpenSSL]](https://www.openssl.org/) podem gerar uma chave privada e um certificado a serem usados na Etapa 2 do assistente.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
