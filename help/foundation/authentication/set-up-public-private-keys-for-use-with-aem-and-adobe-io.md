---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM usa pares de chaves públicas/privadas para se comunicar com segurança com E/S de Adobe e outros serviços da Web. Este breve tutorial ilustra como chaves e armazenamentos de chaves compatíveis podem ser gerados usando a ferramenta de linha de comando openssl que funciona com E/S de AEM e Adobe. '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# Configurar chaves públicas e privadas para uso com E/S Adobe

AEM usa pares de chaves públicas/privadas para se comunicar com segurança com E/S de Adobe e outros serviços da Web. Este breve tutorial ilustra como chaves e armazenamentos de chaves compatíveis podem ser gerados usando a ferramenta de linha de [!DNL openssl] comando que funciona com E/S de AEM e Adobe.

>[!CAUTION]
>
>Este guia cria chaves autoassinadas úteis para desenvolvimento e uso em ambientes inferiores. Em cenários de produção, as chaves são normalmente geradas e gerenciadas pela equipe de segurança de TI de uma organização.

## Gerar o par de chave pública/privada {#generate-the-public-private-key-pair}

O [comando](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) da ferramenta de linha de comando [[!DNL req] [!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/req.html) pode ser usado para gerar um par de chaves compatível com E/S de Adobe e Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Para concluir o [!DNL openssl generate] comando, forneça as informações do certificado quando solicitado. E/S de Adobe e AEM não se importam com o que são esses valores, no entanto eles devem se alinhar e descrever sua chave.

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

## Adicionar par de teclas a um novo armazenamento de chaves {#add-key-pair-to-a-new-keystore}

Os pares de teclas podem ser adicionados a um novo [!DNL PKCS12] armazenamento de chaves. Como parte do [[!DNL openssl]'s [!DNL pcks12] comando,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) o nome do armazenamento de chaves (via `-  caname`), o nome da chave (via `-name`) e a senha do armazenamento de chaves (via `-  passout`) são definidos.

Esses valores são necessários para carregar o armazenamento de chaves e as teclas no AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

A saída deste comando é um `keystore.p12` arquivo.

>[!NOTE]
>
>Os valores de parâmetro de **[!DNL my-keystore]**, **[!DNL my-key]** e **[!DNL my-password]** devem ser substituídos pelos seus próprios valores.

## Verificar o conteúdo do armazenamento de chaves {#verify-the-keystore-contents}

A ferramenta [[!DNL keytool] de linha de](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) comando Java fornece visibilidade em um armazenamento de chaves para garantir que as chaves sejam carregadas com êxito no arquivo de armazenamento de chaves ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verifique o armazenamento de chaves na E/S do Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Adicionar o keystore a AEM {#adding-the-keystore-to-aem}

AEM usa a chave **** privada gerada para se comunicar com segurança com a E/S do Adobe e outros serviços da Web. Para que a chave privada seja acessível ao AEM, ela deve ser instalada no armazenamento de chaves de um usuário AEM.

Navegue até **AEM >[!UICONTROL Ferramentas]>[!UICONTROL Segurança]>[!UICONTROL Usuários]** e **edite o usuário** ao qual a chave privada deve ser associada.

### Criar um armazenamento de chaves AEM {#create-an-aem-keystore}

![Criar KeyStore em AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)*AEM >[!UICONTROL Ferramentas]>[!UICONTROL Segurança]>[!UICONTROL Usuários]> Editar usuário*

Se solicitado a criar um armazenamento de chaves, faça isso. Esse armazenamento de chaves existirá somente no AEM e NÃO é o armazenamento de chaves criado por meio do openssl. A senha pode ser qualquer coisa e não precisa ser a mesma usada no [!DNL openssl] comando.

### Instale a chave privada pelo keystore {#install-the-private-key-via-the-keystore}

![Adicionar chave privada em AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)*[!UICONTROL usuário]>[!UICONTROL Keystore]>[!UICONTROL Adicionar chave privada do keystore]*

No console de armazenamento de chaves do usuário, clique em **[!UICONTROL Adicionar chave privada do arquivo]** KeyStore e adicione as seguintes informações:

* **[!UICONTROL Novo alias]**: o alias da chave no AEM. Isso pode ser qualquer coisa e não precisa corresponder ao nome do keystore criado com o comando openssl.
* **[!UICONTROL Arquivo]** Keystore: a saída do comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Alias]** da chave privada: A senha definida no comando openssl pkcs12 por `-  passout` argumento.

* **[!UICONTROL Senha]** da chave privada: A senha definida no comando openssl pkcs12 por `-  passout` argumento.

### Verifique se a chave privada foi carregada no armazenamento de chaves AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verifique a chave privada no AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)*[!UICONTROL usuário]>[!UICONTROL Keystore]*

Quando a chave privada é carregada com êxito do armazenamento de chaves fornecido no armazenamento de chaves AEM, os metadados da chave privada são exibidos no console do armazenamento de chaves do usuário.

## Adicionando a chave pública à E/S do Adobe {#adding-the-public-key-to-adobe-i-o}

A chave pública correspondente deve ser carregada para E/S de Adobe para permitir que o usuário do serviço de AEM, que tem a chave pública privada correspondente, se comunique com segurança.

### Criar uma nova integração de E/S de Adobe {#create-a-adobe-i-o-new-integration}

![Criar uma nova integração de E/S de Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Criar integração]](https://console.adobe.io/)de E/S de Adobe >[!UICONTROL Nova integração]*

A criação de uma nova integração na E/S do Adobe requer o upload de um certificado público. Carregue o **certificate.crt** gerado pelo `openssl req` comando.

### Verifique se as chaves públicas estão carregadas no Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Verifique as chaves públicas na E/S do Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

As chaves públicas instaladas e suas datas de expiração estão listadas no console [!UICONTROL Integrações] na E/S do Adobe. Várias chaves públicas podem ser adicionadas por meio do botão **[!UICONTROL Adicionar uma chave]** pública.

Agora, AEM a chave privada e a integração de E/S do Adobe contém a chave pública correspondente, permitindo que AEM se comunique com segurança com a E/S do Adobe.
