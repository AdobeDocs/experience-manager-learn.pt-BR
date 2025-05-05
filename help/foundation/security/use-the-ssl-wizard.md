---
title: Usar o Assistente de SSL no AEM
description: Assistente de configuração SSL do Adobe Experience Manager para facilitar a configuração de uma instância do AEM para execução em HTTPS.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# Usar o Assistente de SSL no AEM

Saiba como configurar o SSL no Adobe Experience Manager para que ele seja executado em HTTPS usando o assistente SSL integrado.

>[!VIDEO](https://video.tv.adobe.com/v/33350?quality=12&learn=on&captions=por_br)


>[!NOTE]
>
>Para ambientes gerenciados, é melhor que o departamento de TI forneça chaves e certificados confiáveis de CA.
>
>Certificados autoassinados devem ser usados apenas para fins de desenvolvimento.

## Usando o Assistente de configuração de SSL

Navegue até __AEM Author > Tools > Security > SSL Configuration__ e abra o __Assistente de Configuração SSL__.

![Assistente de Configuração SSL](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Criar credenciais de armazenamento

Para criar um _Armazenamento de Chaves_ associado ao usuário do sistema `ssl-service` e um _Armazenamento de Confiança_ global, use a etapa do assistente __Credenciais de Armazenamento__.

1. Insira a senha e confirme a senha do __Armazenamento de Chaves__ associado ao usuário do sistema `ssl-service`.
1. Insira a senha e confirme a senha do __Armazenamento de Confiança__ global. Observe que é um armazenamento de confiança do sistema e, se já tiver sido criado, a senha inserida será ignorada.

   ![Instalação do SSL - Armazenar Credenciais](assets/use-the-ssl-wizard/store-credentials.png)

### Carregar chave privada e certificado

Para carregar a _chave privada_ e o _certificado SSL_, use a etapa do assistente __Chave e Certificado__.

Normalmente, seu departamento de TI fornece o certificado e a chave confiáveis da CA, no entanto, o certificado autoassinado pode ser usado para fins de __desenvolvimento__ e __teste__.

Para criar ou baixar o certificado autoassinado, consulte a [Chave privada e o certificado autoassinados](#self-signed-private-key-and-certificate).

1. Carregue a __Chave privada__ no formato DER (Distinguished Encoding Rules). Ao contrário do PEM, os arquivos codificados com DER não contêm instruções de texto simples como `-----BEGIN CERTIFICATE-----`
1. Carregar o __Certificado SSL__ associado no formato `.crt`.

   ![Instalação de SSL - Chave privada e certificado](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Atualizar detalhes do conector SSL

Para atualizar o _nome do host_ e a _porta_, use a etapa do assistente do __Conector SSL__.

1. Atualize ou verifique o valor de __Nome do Host HTTPS__. Ele deve corresponder ao `Common Name (CN)` do certificado.
1. Atualize ou verifique o valor da __Porta HTTPS__.

   ![Configuração de SSL - Detalhes do Conector SSL](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Verifique a configuração do SSL

1. Para verificar o SSL, clique no botão __Ir para URL HTTPS__.
1. Se estiver usando um certificado autoassinado, você verá um erro `Your connection is not private`.

   ![Configuração de SSL - Verificar AEM sobre HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Chave privada e certificado autoassinados

O zip a seguir contém arquivos [!DNL DER] e [!DNL CRT] necessários para configurar o AEM SSL localmente e destinados apenas a fins de desenvolvimento local.

Os arquivos [!DNL DER] e [!DNL CERT] são fornecidos para conveniência e gerados usando as etapas descritas na seção Gerar Chave Privada e Certificado Autoassinado abaixo.

Se necessário, a senha do certificado é **admin**.

Este host local - chave privada e certificate.zip autoassinado (expira em julho de 2028)

[Baixar o arquivo de certificado](assets/use-the-ssl-wizard/certificate.zip)

### Geração de chave privada e certificado autoassinado

O vídeo acima descreve a configuração do SSL em uma instância de autor do AEM usando certificados autoassinados. Os comandos abaixo que usam [[!DNL OpenSSL]](https://www.openssl.org/) podem gerar uma chave privada e um certificado a serem usados na Etapa 2 do assistente.

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
