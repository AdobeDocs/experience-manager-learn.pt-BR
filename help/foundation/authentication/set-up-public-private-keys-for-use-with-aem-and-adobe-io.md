---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: O AEM usa pares de chaves públicas/privadas para se comunicar com segurança com o Adobe I/O e outros serviços da Web. Este breve tutorial ilustra como chaves e armazenamentos de chaves compatíveis podem ser gerados usando a ferramenta de linha de comando openssl que funciona com AEM e Adobe I/O.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Development
role: Developer
level: Experienced
exl-id: 62ed9dcc-6b8a-48ff-8efe-57dabdf4aa66
last-substantial-update: 2022-07-17T00:00:00Z
thumbnail: KT-2450.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# Configurar chaves públicas e privadas para uso com o Adobe I/O

O AEM usa pares de chaves públicas/privadas para se comunicar com segurança com o Adobe I/O e outros serviços da Web. Este breve tutorial ilustra como chaves e armazenamentos de chaves compatíveis podem ser gerados usando o [!DNL openssl] ferramenta de linha de comando que funciona com AEM e Adobe I/O.

>[!CAUTION]
>
>Este guia cria chaves autoassinadas úteis para desenvolvimento e uso em ambientes inferiores. Em cenários de produção, as chaves são normalmente geradas e gerenciadas pela equipe de segurança de TI de uma organização.

## Gerar o par de chaves públicas/privadas {#generate-the-public-private-key-pair}

O [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) da ferramenta de linha de comando [[!DNL req] comando](https://www.openssl.org/docs/man1.0.2/man1/req.html) pode ser usada para gerar um par de chaves compatível com o Adobe I/O e o Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Para concluir a [!DNL openssl generate] , forneça as informações do certificado quando solicitado. Adobe I/O e AEM não se importam com o que são esses valores, no entanto, eles devem se alinhar e descrever sua chave.

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## Adicionar par de chaves a um novo armazenamento de chaves {#add-key-pair-to-a-new-keystore}

Os pares de chaves podem ser adicionados a um novo [!DNL PKCS12] armazenamento de chaves. Como parte de [[!DNL openssl]'s [!DNL pcks12] comando,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) o nome do armazenamento de chaves (via `-  caname`), o nome da chave (via `-name`) e a senha do repositório de chaves (via `-  passout`) são definidas.

Esses valores são necessários para carregar o armazenamento de chaves e as teclas no AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

A saída deste comando é uma `keystore.p12` arquivo.

>[!NOTE]
>
>Os valores de parâmetro de **[!DNL my-keystore]**, **[!DNL my-key]** e **[!DNL my-password]** serão substituídas pelos seus próprios valores.

## Verificar o conteúdo do armazenamento de chaves {#verify-the-keystore-contents}

O Java™ [[!DNL keytool] ferramenta de linha de comando](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) fornece visibilidade em um armazenamento de chaves para garantir que as chaves sejam carregadas com êxito no arquivo do armazenamento de chaves ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verificar o armazenamento de chaves no Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Adicionar o repositório de chaves ao AEM {#adding-the-keystore-to-aem}

AEM usa o **chave privada** para se comunicar com segurança com o Adobe I/O e outros serviços da Web. Para que a chave privada seja acessível ao AEM, ela deve ser instalada no repositório de chaves de um usuário AEM.

Navegar para **AEM > [!UICONTROL Ferramentas] > [!UICONTROL Segurança] > [!UICONTROL Usuários]** e **editar o usuário** a chave privada deve ser associada a.

### Criar um repositório de chaves AEM {#create-an-aem-keystore}

![Criar KeyStore em AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM > [!UICONTROL Ferramentas] > [!UICONTROL Segurança] > [!UICONTROL Usuários] > Editar usuário*

Quando solicitado a criar um armazenamento de chaves, faça isso. Este armazenamento de chaves existe somente no AEM e NÃO é o armazenamento de chaves criado por meio do openssl. A senha pode ser qualquer coisa e não precisa ser igual à senha usada no [!DNL openssl] comando.

### Instalar a chave privada por meio do keystore {#install-the-private-key-via-the-keystore}

![Adicionar chave privada no AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL Usuário] > [!UICONTROL Armazenamento de chaves] > [!UICONTROL Adicionar chave privada do repositório de chaves]*

No console do repositório de chaves do usuário, clique em **[!UICONTROL Adicionar arquivo Chave privada do arquivo KeyStore]** e adicione as seguintes informações:

* **[!UICONTROL Novo Alias]**: o alias da chave em AEM. Pode ser qualquer coisa e não precisa corresponder ao nome do repositório de chaves criado com o comando openssl.
* **[!UICONTROL Arquivo KeyStore]**: a saída do comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Senha do arquivo KeyStore]**: A senha definida no comando openssl pkcs12 via `-passout` argumento .
* **[!UICONTROL Alias da chave privada]**: O valor fornecido à `-name` argumento no comando openssl pkcs12 acima (ou seja, `my-key`).
* **[!UICONTROL Senha da chave privada]**: A senha definida no comando openssl pkcs12 via `-passout` argumento .

>[!CAUTION]
>
>A Senha do Arquivo KeyStore e a Senha da Chave Privada são iguais para ambas as entradas. Inserir uma senha incompatível resulta na não importação da chave.

### Verifique se a chave privada está carregada no armazenamento de chaves AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verificar chave privada no AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL Usuário] > [!UICONTROL Armazenamento de chaves]*

Quando a chave privada é carregada com êxito do armazenamento de chaves fornecido no armazenamento de chaves AEM, os metadados da chave privada são exibidos no console de armazenamento de chaves do usuário.

## Adicionar a chave pública ao Adobe I/O {#adding-the-public-key-to-adobe-i-o}

A chave pública correspondente deve ser carregada no Adobe I/O para permitir que o usuário do serviço de AEM, que tem a chave pública privada correspondente, se comunique com segurança.

### Criar uma nova integração Adobe I/O {#create-a-adobe-i-o-new-integration}

![Criar uma nova integração do Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Criar integração do Adobe I/O]](https://developer.adobe.com/console/) > [!UICONTROL Nova integração]*

Criar uma integração no Adobe I/O requer o upload de um certificado público. Faça upload do **certificate.crt** gerado pelo `openssl req` comando.

### Verifique se as chaves públicas estão carregadas no Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Verificar chaves públicas no Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

As chaves públicas instaladas e suas datas de expiração estão listadas na variável [!UICONTROL Integrações] no Adobe I/O. Várias chaves públicas podem ser adicionadas por meio do **[!UICONTROL Adicionar uma chave pública]** botão.

Agora, o AEM mantém a chave privada e a integração do Adobe I/O contém a chave pública correspondente, permitindo que AEM se comunique com o Adobe I/O de forma segura.
