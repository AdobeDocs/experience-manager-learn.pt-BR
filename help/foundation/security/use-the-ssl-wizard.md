---
title: Usar o assistente de SSL no AEM
description: Assistente de configuração de SSL do Adobe Experience Manager para facilitar a configuração de uma instância do AEM para execução em HTTPS.
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
workflow-type: ht
source-wordcount: '448'
ht-degree: 100%

---

# Usar o assistente de SSL no AEM

Saiba como configurar o SSL no Adobe Experience Manager, a fim de executá-lo em HTTPS por meio do assistente de SSL integrado.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>Para ambientes gerenciados, é melhor que o departamento de TI forneça chaves e certificados de uma autoridade de certificação confiável.
>
>Certificados autoassinados devem ser usados apenas para fins de desenvolvimento.

## Usar o assistente de configuração de SSL

Navegue até __Autor do AEM > Ferramentas > Segurança > Configuração de SSL__ e abra o __Assistente de configuração de SSL__.

![Assistente de configuração de SSL](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Criar credenciais de armazenamento

Para criar um _armazenamento de chaves_ associado ao usuário do sistema `ssl-service` e um _armazenamento de confiança_ global, use a etapa do assistente __Credenciais de armazenamento__.

1. Insira e confirme a senha do __armazenamento de chaves__ associado ao usuário do sistema `ssl-service`.
1. Insira e confirme a senha do __armazenamento de confiança__ global. Observe que esse armazenamento de confiança se aplica a todo o sistema e, se já tiver sido criado, a senha inserida será ignorada.

   ![Configuração de SSL: armazenar credenciais](assets/use-the-ssl-wizard/store-credentials.png)

### Carregar chave privada e certificado

Para carregar a _chave privada_ e o _certificado SSL_, use a etapa do assistente __Chave e certificado__.

Normalmente, o seu departamento de TI fornece o certificado e a chave de uma autoridade de certificação confiável, mas é possível usar um certificado autoassinado para fins de __desenvolvimento__ e __teste__.

Para criar ou baixar um certificado autoassinado, consulte [Chave privada e certificado autoassinados](#self-signed-private-key-and-certificate).

1. Carregue a __chave privada__ no formato DER (Distinguished Encoding Rules). Ao contrário do formato PEM, os arquivos codificados em DER não contêm declarações de texto sem formatação, como `-----BEGIN CERTIFICATE-----`
1. Carregue o __certificado SSL__ associado no formato `.crt`.

   ![Configuração de SSL: chave privada e certificado](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Atualizar detalhes do conector de SSL

Para atualizar o _nome do host_ e a _porta_, use a etapa do assistente __Conector de SSL__.

1. Atualize ou verifique o valor do __nome do host HTTPS__, o qual deve corresponder ao `Common Name (CN)` do certificado.
1. Atualize ou verifique o valor da __porta HTTPS__.

   ![Configuração de SSL: detalhes do conector de SSL](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Verifique a configuração de SSL

1. Para verificar o SSL, clique no botão __Acessar o URL HTTPS__.
1. Se estiver usando um certificado autoassinado, você verá um erro `Your connection is not private`.

   ![Configuração de SSL: verificar o AEM por HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Chave privada e certificado autoassinados

O arquivo zip a seguir contém os arquivos [!DNL DER] e [!DNL CRT] necessários para configurar o SSL do AEM localmente, e servem apenas para fins de desenvolvimento local.

Os arquivos [!DNL DER] e [!DNL CERT] são fornecidos para a sua comodidade e gerados por meio das etapas descritas na seção “Gerar chave privada e certificado autoassinados” abaixo.

Se necessário, a senha do certificado é **admin**.

Este host local: private key and self-signed certificate.zip (expira em julho de 2028)

[Baixar o arquivo do certificado](assets/use-the-ssl-wizard/certificate.zip)

### Geração de chave privada e certificado autoassinados

O vídeo acima descreve a configuração de SSL em uma instância de criação do AEM usando certificados autoassinados. Os comandos abaixo baseados em [[!DNL OpenSSL]](https://www.openssl.org/) podem gerar uma chave privada e um certificado para uso na etapa 2 do assistente.

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
