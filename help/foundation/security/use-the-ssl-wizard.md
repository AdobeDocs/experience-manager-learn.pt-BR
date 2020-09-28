---
title: Usar o Assistente SSL no AEM
description: Assistente de configuração de SSL da Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução em HTTPS.
seo-description: Assistente de configuração de SSL da Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução em HTTPS.
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---


# Usar o Assistente SSL no AEM

Assistente de configuração de SSL da Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução em HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Para ambientes gerenciados, é melhor para o departamento de TI fornecer certificados e chaves confiáveis pela CA.
>
>Os certificados autoassinados só podem ser utilizados para fins de desenvolvimento.

## Chave privada e download de certificado autoassinado

O zip a seguir contém [!DNL DER] e [!DNL CRT] arquivos necessários para a configuração AEM SSL no localhost e destina-se apenas a fins de desenvolvimento local.

Os arquivos [!DNL DER] e [!DNL CERT] os arquivos são fornecidos para conveniência e gerados usando as etapas descritas na seção Gerar chave privada e certificado autoassinado abaixo.

Se necessário, a senha do certificado é **admin**.

localhost - chave privada e certificado autoassinado.zip (expira em julho de 2028)

[Baixar o arquivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

## Chave privada e geração de certificado autoassinada

O vídeo acima mostra a configuração do SSL em uma instância do autor AEM usando certificados autoassinados. Os comandos abaixo usando [[!DNL OpenSSL]](https://www.openssl.org/) podem gerar uma chave privada e um certificado para serem usados na Etapa 2 do assistente.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
