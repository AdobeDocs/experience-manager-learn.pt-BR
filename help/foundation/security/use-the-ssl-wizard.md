---
title: Usar o assistente SSL no AEM
description: Assistente de configuração SSL do Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução em HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Usar o assistente SSL no AEM

Assistente de configuração SSL do Adobe Experience Manager para facilitar a configuração de uma instância AEM para execução em HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

Abra o __Assistente de configuração do SSL__ pode ser aberto diretamente navegando até __Autor do AEM > Ferramentas > Segurança > Configuração de SSL__.

>[!NOTE]
>
>Para ambientes gerenciados, é melhor que o departamento de TI forneça chaves e certificados confiáveis de CA.
>
>Certificados autoassinados devem ser usados apenas para fins de desenvolvimento.

## Download da chave privada e do certificado autoassinado

O zip a seguir contém [!DNL DER] e [!DNL CRT] arquivos necessários para configurar o AEM SSL no host local e destinados somente a fins de desenvolvimento local.

A variável [!DNL DER] e [!DNL CERT] os arquivos são fornecidos para conveniência e gerados usando as etapas descritas na seção Gerar chave privada e certificado autoassinado abaixo.

Se necessário, a senha do certificado é **administrador**.

localhost - chave privada e certificate.zip autoassinado (expira em julho de 2028)

[Baixar o arquivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

## Geração de chave privada e certificado autoassinado

O vídeo acima descreve a configuração do SSL em uma instância de autor do AEM usando certificados autoassinados. Os comandos abaixo que usam [[!DNL OpenSSL]](https://www.openssl.org/) O pode gerar uma chave privada e um certificado para serem usados na Etapa 2 do assistente.

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
