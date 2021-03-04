---
title: Usar o assistente SSL no AEM
description: O assistente de configuração SSL do Adobe Experience Manager para facilitar a configuração de uma instância do AEM para execução por HTTPS.
seo-description: O assistente de configuração SSL do Adobe Experience Manager para facilitar a configuração de uma instância do AEM para execução por HTTPS.
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Usar o assistente SSL no AEM

O assistente de configuração SSL do Adobe Experience Manager para facilitar a configuração de uma instância do AEM para execução por HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Para ambientes gerenciados, é melhor para o departamento de TI fornecer certificados e chaves confiáveis pela CA.
>
>Certificados autoassinados só devem ser usados para fins de desenvolvimento.

## Chave privada e download de certificado autoassinado

O zip a seguir contém os arquivos [!DNL DER] e [!DNL CRT] necessários para a configuração do SSL do AEM no host local e destinados apenas a fins de desenvolvimento local.

Os arquivos [!DNL DER] e [!DNL CERT] são fornecidos para conveniência e gerados usando as etapas descritas na seção Gerar chave privada e certificado autoassinado abaixo.

Se necessário, a frase de passagem do certificado é **admin**.

localhost - chave privada e certificate.zip autoassinado (expira em julho de 2028)

[Baixar o arquivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

## Chave privada e geração de certificado autoassinado

O vídeo acima descreve a configuração do SSL em uma instância de autor do AEM usando certificados autoassinados. Os comandos abaixo usando [[!DNL OpenSSL]](https://www.openssl.org/) podem gerar uma chave privada e um certificado a serem usados na Etapa 2 do assistente.

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
