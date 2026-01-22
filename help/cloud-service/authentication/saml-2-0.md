---
title: SAML 2.0 em AEM como Cloud Service
description: Aprenda a configurar a autenticação SAML 2.0 em AEM como um serviço Cloud Service Publish.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 1%

---

# Autenticação SAML 2.0{#saml-2-0-authentication}

Aprenda a configurar e autenticar usuários finais (não AEM autores) para uma IDP compatível com SAML 2.0 da sua escolha.

## Que SAML para AEM como Cloud Service?

A integração do SAML 2.0 com a AEM Publish (ou Visualização) permite que os usuários finais de uma experiência da Web com base em AEM autenticassem para um IDP não Adobe Systems (Provedor de identidade) e acessem AEM como um usuário nomeado e autorizado.

|                       | Autor do AEM | AEM Publish |
|-----------------------|:----------:|:-----------:|
| Suporte para SAML 2.0 | ✘ | ✔ |

+++ Entenda o fluxo do SAML 2.0 com AEM

O fluxo típico de uma integração do AEM Publish SAML é o seguinte:

1. O usuário faz uma solicitação para AEM Publish indica que a autenticação é necessária.
   + O usuário solicita um recurso protegido por CUGs/ACL.
   + O usuário solicita um recurso que esteja sujeito a um Requisito de Authentication.
   + O usuário segue um link ao terminal fazer logon de AEM que `/system/sling/login`solicita explicitamente a ação do fazer logon.
1. AEM faz um AuthnRequest para o IDP, solicitando que o IDP start processo de autenticação.
1. O usuário é autenticado no IDP.
   + O IDP solicita as credenciais ao usuário.
   + O usuário já está autenticado com o IDP e não precisa fornecer mais credenciais.
1. O IDP gera uma asserção SAML contendo os dados do usuário e a assina usando o certificado privado do IDP.
1. O IDP envia a asserção SAML via HTTP POST, por meio do navegador da Web do usuário (RESPECTIVE_PROTECTED_PATH/saml_login), para o AEM Publish.
1. AEM Publish recebe a asserção saml, e valida a integridade e autenticidade da asserção saml usando o certificado público IDP.
1. AEM Publish gerencia a AEM registro usuário com base na configuração SAML 2.0 OSGi e no conteúdo da Asserção de SAML.
   + Cria usuário
   + Sincroniza os atributos do usuário
   + Atualiza a associação do grupo de usuários do AEM
1. O AEM Publish define o cookie AEM `login-token` na resposta HTTP, que é usado para autenticar solicitações subsequentes ao AEM Publish.
1. AEM Publish redireciona usuário para URL em AEM Publish conforme especificado pelo `saml_request_path` cookie.

+++

## Apresentação da configuração

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Este vídeo mostra a configuração da integração do SAML 2.0 com o AEM como um serviço de Cloud Service Publish e o uso do Okta como IDP.

## Pré-requisitos

As seguintes condições são necessárias ao configurar a autenticação SAML 2.0:

+ Acesso do Gerenciador de implantação ao Cloud Manager
+ AEM Acesso de administrador ao AEM as a Cloud Service ambiente
+ Acesso de administrador ao IDP
+ Opcionalmente, o acesso a um chaveiro público/privado usado para criptografar cargas SAML
+ AEM Sites páginas (ou árvores página), publicadas em AEM Publish e [protegidas por Grupos fechados de usuários (CUGs)](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

O SAML 2.0 é compatível somente para autenticar usos em AEM Publish ou Visualização. Para gerenciar a autenticação do AEM Author usando e o IDP, [integre o IDP ao Adobe IMS](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html).


## Instalar o certificado público IDP no AEM

O certificado público do IDP é adicionado ao armazenamento global de confiança da AEM e usado para validar que a asserção SAML enviada pelo IDP é válida.

+++Fluxo de assinatura de asserção SAML

![SAML 2.0 - Assinatura de Asserção SAML IDP](./assets/saml-2-0/idp-signing-diagram.png)

1. O usuário é autenticado no IDP.
1. O IDP gera uma asserção SAML contendo os dados do usuário.
1. O IDP assina a asserção SAML usando o certificado privado do IDP.
1. O IDP inicia uma lado do cliente HTTP POST para AEM o ponto de extremidade SAML (`.../saml_login`) de Publish que inclui a asserção SAML assinada.
1. AEM Publish recebe o POST HTTP contendo a asserção SAML assinada, pode validar a assinatura usando o certificado público IDP.

+++

![Adicione o certificado público do IDP ao Global Trust Store](./assets/saml-2-0/global-trust-store.png)

1. Obtenha o __arquivo de certificado__ público do IDP. Esse certificado permite AEM validar a asserção SAML fornecida em AEM pela IDP.

   O certificado está no formato PEM e deve ser semelhante:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Faça logon no AEM Autor como um Administrador AEM.
1. Navegue até __Ferramentas > Security > Trust Store__.
1. Criar ou abra o Armazenamento global de confiança. Se estiver criando um Global Trust Store, armazenamento o senha algum lugar seguro.
1. Expanda __Adicionar certificado do arquivo CER__.
1. Selecione __Selecionar Arquivo__ de certificado e upload o arquivo de certificado fornecido pelo IDP.
1. Deixe __o certificado de mapa para o usuário__ em branco.
1. Selecione __Enviar__.
1. O certificado recém-adicionado aparece acima do certificado Adicionar na __seção de arquivos__ CRT.
1. Observe o alias ____, pois esse valor é usado na configuração[&#x200B; OSGi do &#x200B;](#saml-2-0-authentication-handler-osgi-configuration)manipulador SAML 2.0 Authentication.
1. Selecione __Salvar &amp;Fechar__.

O Global Trust Store é configurado com o certificado público do IDP em AEM Autor, mas como o SAML só é usado em AEM Publish, o Global Trust Store deve ser replicado para AEM Publish para que o certificado público de IDP seja acessível lá.

![Replicar o armazenamento global de confiança para AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. Navegue até __Ferramentas > pacotes__ de > de implantação.
1. Criar um pacote
   + Nome do pacote: `Global Trust Store`
   + Versão: `1.0.0`
   + Grupo: `com.your.company`
1. Editar o novo __pacote do Armazenamento global de__ confiança.
1. Selecione o __guia Filtros__ e adicione um filtro para o caminho `/etc/truststore`raiz.
1. Selecione __Concluído__ e depois __Salvar__.
1. Selecione a __botão de compilação__ para o pacote do __armazenamento global de confiança__ .
1. Depois de criado, selecione __Mais__ > __replicar__ para ativar o Global Trust Store nó (`/etc/truststore`) para AEM Publish.

## Criar armazenamento de chaves do serviço de autenticação{#authentication-service-keystore}

_A criação de um armazenamento de chaves para o serviço de autenticação é necessária quando a [configuração do manipulador de autenticação SAML 2.0 OSGi propriedade `handleLogout` está definida `true`](#saml-20-authenticationsaml-2-0-authentication) como ou quando [a ecripção de asserção](#install-aem-public-private-key-pair) de AuthnRequest/SAML é necessária_

1. Faça logon no AEM Autor como um Administrador AEM, para upload a chave privada.
1. Navegue até __Ferramentas > Segurança > Usuários__, selecione o usuário __serviço de autenticação__ e selecione __Propriedades__ na barra de ações superior.
1. Selecione a guia __Armazenamento de chaves__.
1. Crie ou abra o armazenamento de chaves. Se estiver criando um armazenamento de chaves, mantenha a senha segura.
   + Um [armazenamento de chaves público/privado está instalado neste armazenamento de chaves](#install-aem-public-private-key-pair) somente se a criptografia de assinatura AuthnRequest/SAML é necessária.
   + Se essa integração SAML der suporte a logout, mas não a assinatura AuthnRequest/asserção SAML, um armazenamento de chaves vazio será suficiente.
1. Selecione __Salvar &amp;Fechar__.
1. Crie um pacote contendo o usuário __authentication-service__ atualizado.

   _Usar a solução temporária a seguir usando pacotes :_

   1. Navegue até __Ferramentas > Implantação > Pacotes__.
   1. Criar um pacote
      + Nome do pacote: `Authentication Service`
      + Versão: `1.0.0`
      + Grupo: `com.your.company`
   1. Editar o novo __pacote Authentication Service Key Store__ .
   1. Selecione o __guia Filtros__ e adicione um filtro para o caminho `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`raiz.
      + Ele `<AUTHENTICATION SERVICE UUID>` pode ser encontrado navegando para __Ferramentas > Usuários__ > de Segurança e selecionando __usuário do serviço__ de autenticação. O UUID é a última parte do URL.
   1. Selecione __Concluído__ e __depois Salvar__.
   1. Selecione a __botão de compilação__ para o pacote da __Authentication Service Key Store__ .
   1. Depois de criado, selecione __Mais__ > __Replicar__ para ativar os armazenamento de chave do Serviço de Authentication para AEM Publish.

## Instalar AEM par de chaves públicas/privadas{#install-aem-public-private-key-pair}

_A instalação do AEM par de chaves públicas/privadas é opcional_

AEM Publish pode ser configurada para assinar AuthnRequests (para IDP) e criptografar afirmações SAML (para AEM). Isso é feito fornecendo uma chave privada para AEM Publish, e está correspondendo a chave pública com o IDP.

+++ Entenda o fluxo de assinatura de AuthnRequest (opcional)

A AuthnRequest (a solicitação ao IDP do AEM Publish que inicia o processo de logon) pode ser assinada pelo AEM Publish. Para fazer isso, o AEM Publish assina a AuthnRequest usando a chave privada, e garante que o IDP valida a assinatura usando a chave pública. Isso garante ao IDP que o AuthnRequest foi iniciado e solicitado pelo AEM Publish, e não por terceiros mal-intencionados.

![SAML 2.0 - Assinatura AuthnRequest do SP](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. O usuário faz uma solicitação HTTP para AEM Publish que resulte em uma autenticação SAML solicitação para o IDP.
1. AEM Publish gera o solicitação SAML para enviar para o IDP.
1. AEM Publish assina o SAML solicitação usando a chave privada de AEM.
1. AEM Publish inicia o AuthnRequest, um redirecionar HTTP lado do cliente ao IDP que contém o solicitação SAML assinado.
1. O IDP recebe a AuthnRequest e valida a assinatura usando a chave pública da AEM, garantindo AEM Publish iniciada a AuthnRequest.
1. AEM Publish então valida a integridade e autenticidade da asserção saml descriptografada usando o certificado público IDP.

+++

+++ Entenda o fluxo de criptografia de asserção de SAML (opcional)

Todas as comunicações HTTP entre IDP e AEM Publish devem ser via HTTPS e, portanto, seguras por padrão. No entanto, conforme necessário, as afirmações SAML podem ser criptografadas na evento confidencialidade extra é necessária além da fornecida pelo HTTPS. Para fazer isso, o IDP criptografa os dados de Asserção de SAML usando a chave privada, e AEM Publish descriptografa a asserção de SAML usando a chave privada.

![SAML 2.0 - Criptografia de asserção de SAML do SP](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. O usuário autentica para IDP.
1. O IDP gera uma asserção SAML contendo os dados do usuário e a assina usando o certificado privado do IDP.
1. O IDP criptografa a asserção SAML com a chave pública da AEM, o que requer que a AEM chave privada seja descriptografada.
1. A asserção SAML criptografada é enviada por meio dos navegador da Web do usuário para AEM Publish.
1. AEM Publish recebe a asserção saml, e a descriptografa usando a chave privada de AEM.
1. Solicitações do IDP usuário a autenticação.

+++

Tanto a assinatura de AuthnRequest quanto a criptografia de asserção saml são opcionais, no entanto, ambas estão ativadas, usando o [manipulador de autenticação SAML 2.0 OSGi propriedade `useEncryption`](#saml-20-authenticationsaml-2-0-authentication), o que significa que ambos ou nem podem ser usados.

![chave do serviço de autenticação AEM armazenamento](./assets/saml-2-0/authentication-service-key-store.png)

1. Obter a chave pública, a chave privada (PKCS#8 no formato DER) e o arquivo da cadeia de certificados (esta pode ser a chave pública) usada para assinar a AuthnRequest e criptografar a asserção de SAML. As chaves normalmente são fornecidas pelo equipe de segurança da organização de TI.

   + Um par de chave autoassinado pode ser gerado usando __o openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Faça upload da chave pública para o IDP.
   + Usando o `openssl` método acima, a chave pública é o `aem-public.crt` arquivo.
1. Faça logon no AEM Autor como um Administrador AEM, para upload a chave privada.
1. Navegue até __Ferramentas > Security > Trust Store__ e selecione __usuário do serviço__ de autenticação e selecione __Propriedades__ na barra de ação superior.
1. Navegue até __Ferramentas > Usuários__ > de segurança e selecione __usuário do serviço__ de autenticação e selecione __Propriedades__ na barra de ações superior.
1. Selecione o guia do __Keystore__ .
1. Criar ou abrir o repositório de chaves. Se estiver criando um armazenamento de chaves, mantenha a senha segura.
1. Selecione __Adicionar chave privada do arquivo__ DER e adicione a chave privada e o arquivo em cadeia a AEM:
   + __Alias__: forneça um nome significativo, geralmente o nome da IDP.
   + __Arquivo de__ chave privada: fazer upload do arquivo de chave privada (PKCS#8 no formato DER).
      + Usando o `openssl` método acima, este é o `aem-private-pkcs8.der` arquivo
   + __Selecione o arquivo__ da cadeia de certificados: faça o upload do arquivo da cadeia de acompanhamento (essa pode ser a chave pública).
      + Usando o método `openssl` acima, este é o arquivo `aem-public.crt`
   + Selecionar __Enviar__
1. O certificado adicionado recentemente aparece acima da seção __Adicionar certificado do arquivo CRT__.
   + Anote o __alias__, pois ele é usado na [configuração OSGi do manipulador de autenticação SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Selecione __Salvar e fechar__.
1. Crie um pacote contendo o usuário __authentication-service__ atualizado.

   _Use a seguinte solução temporária usando pacotes :_

   1. Navegue até __Ferramentas > pacotes__ de > de implantação.
   1. Criar um pacote
      + Nome do pacote: `Authentication Service`
      + Versão: `1.0.0`
      + Grupo: `com.your.company`
   1. Editar o novo __pacote Authentication Service Key Store__ .
   1. Selecione o __guia Filtros__ e adicione um filtro para o caminho `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`raiz.
      + Ele `<AUTHENTICATION SERVICE UUID>` pode ser encontrado navegando para __Ferramentas > Usuários__ > de Segurança e selecionando __usuário do serviço__ de autenticação. O UUID é a última parte do URL.
   1. Selecione __Concluído__ e __depois Salvar__.
   1. Selecione a __botão de compilação__ para o pacote da __Authentication Service Key Store__ .
   1. Depois de criado, selecione __Mais__ > __Replicar__ para ativar os armazenamento de chave do Serviço de Authentication para AEM Publish.

## Configurar o manipulador de autenticação SAML 2.0{#configure-saml-2-0-authentication-handler}

A configuração SAML da AEM é executada por meio da __configuração OSGi Adobe Systems Granite SAML 2.0 Authentication Handler__ .
A configuração é uma configuração de fábrica do OSGi, ou seja, uma única AEM como Cloud Service Publish serviço pode ter várias configurações da SAML cobrindo árvores de recursos discretas do repositório; isso é útil para implantações de AEM em vários sites.

+++ Glossário de configuração do MANIPULADOR OSGi Authentication SAML 2.0

### Adobe Systems configuração do Granite SAML 2.0 Authentication OsGi do Handler{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | propriedade OSGi | Obrigatório | Formato do valor | Valor padrão | Descrição |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Caminhos | `path` | ✔ | Matriz de strings | `/` | AEM caminhos para os qual este manipulador de autenticação é usado. |
| URL DO IDP | `idpUrl` | ✔ | String |                           | IDP URL o solicitação de autenticação SAML. |
| Alias do certificado IDP | `idpCertAlias` | ✔ | String |                           | O alias do certificado IDP encontrado no armazenamento global de confiança da AEM |
| REDIRECIONAR HTTP DO IDP | `idpHttpRedirect` | ✘ | Booleano | `false` | Indica se um redirecionamento HTTP para o IDP URL em vez de enviar um AuthnRequest. Definido para `true` autenticação iniciada com IDP. |
| Identificador IDP | `idpIdentifier` | ✘ | String |                           | ID ID exclusiva para garantir AEM usuário e grupo singularidade. Se estiver vazio, será `serviceProviderEntityId` usado em vez disso. |
| URL de serviço ao consumidor de asserção | `assertionConsumerServiceURL` | ✘ | String |                           | O `AssertionConsumerServiceURL` atributo URL no AuthnRequest especificando para onde a `<Response>` mensagem deve ser enviada AEM. |
| ID da entidade SP | `serviceProviderEntityId` | ✔ | String |                           | Identifica exclusivamente AEM ao IDP; geralmente o nome AEM host. |
| Criptografia do SP | `useEncryption` | ✘ | Booleano | `true` | Indica se o IDP criptografa asserções SAML. Exige `spPrivateKeyAlias` e `keyStorePassword` deve ser definido. |
| Alias da chave privada SP | `spPrivateKeyAlias` | ✘ | String |                           | O alias da chave privada na `authentication-service` chave do armazenamento do usuário. Necessário se `useEncryption` estiver definido como `true`. |
| Senha do armazenamento de chave da controladora | `keyStorePassword` | ✘ | String |                           | A senha de &quot;serviço de autenticação&quot; usuário chaves armazenamento. Obrigatório se `useEncryption` estiver definido como `true`. |
| Redirecionar padrão | `defaultRedirectUrl` | ✘ | String | `/` | O URL de redirecionamento padrão após a autenticação bem-sucedida. Pode ser relativo ao host AEM (por exemplo, `/content/wknd/us/en/html`). |
| Atributo de ID de usuário | `userIDAttribute` | ✘ | String | `uid` | O nome do atributo de asserção SAML contendo a ID de usuário do AEM usuário. Deixe em branco para utilizar `Subject:NameId`. |
| usuários AEM criados Automático | `createUser` | ✘ | Booleano | `true` | Indica se os usuários do AEM são criados após a autenticação bem-sucedida. |
| Caminho intermediário do usuário do AEM | `userIntermediatePath` | ✘ | String |                           | Ao criar AEM usuários, esse valor é usado como caminho intermediário (por exemplo, `/home/users/<userIntermediatePath>/jane@wknd.com`). Precisa `createUser` ser definido como `true`. |
| atributos usuário AEM | `synchronizeAttributes` | ✘ | Matriz de strings |                           | Lista de mapeamentos de atributos saml para armazenamento no AEM usuário, no formato `[ "saml-attribute-name=path/relative/to/user/node" ]` (por exemplo, `[ "firstName=profile/givenName" ]`). Consulte a [lista completa dos atributos](#aem-user-attributes) de AEM de nativo. |
| Adicionar usuário a grupos de AEM | `addGroupMemberships` | ✘ | Booleano | `true` | Indica se uma AEM usuário é automaticamente adicionada a AEM usuário grupos após a autenticação bem-sucedida. |
| atributo AEM associação de grupo | `groupMembershipAttribute` | ✘ | String | `groupMembership` | O nome do atributo de asserção SAML que contém uma lista de AEM usuário agrupa os usuário devem ser adicionados. Precisa `addGroupMemberships` ser definido como `true`. |
| Grupos padrão do AEM | `defaultGroups` | ✘ | Matriz de string |                           | Uma lista de grupos de usuários autenticados do AEM é sempre adicionada ao (por exemplo, `[ "wknd-user" ]`). Exige que `addGroupMemberships` seja definido como `true`. |
| Formato NameIDPolicy | `nameIdFormat` | ✘ | String | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | O valor do parâmetro de formato NameIDPolicy a ser enviado na mensagem AuthnRequest. |
| Armazenar resposta do SAML | `storeSAMLResponse` | ✘ | Booleano | `false` | Indica se o `samlResponse` valor está armazenado na AEM `cq:User` nó. |
| Gerenciar logout | `handleLogout` | ✘ | Booleano | `false` | Indica se o logout solicitação é tratado por este manipulador de autenticação SAML. Requer `logoutUrl` que seja definido. |
| Fazer logoff URL | `logoutUrl` | ✘ | String |                           | O URL do IDP para o qual o logout do SAML solicitação é enviado. Obrigatório se `handleLogout` estiver definido como `true`. |
| Tolerância de relógio | `clockTolerance` | ✘ | Número inteiro | `60` | Tolerância de desvio do relógio IDP e AEM (SP) ao validar asserções SAML. |
| Método Digest | `digestMethod` | ✘ | String | `http://www.w3.org/2001/04/xmlenc#sha256` | O algoritmo de compilação que o IDP usa ao assinar uma mensagem SAML. |
| Método de assinatura | `signatureMethod` | ✘ | String | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | O algoritmo de assinatura que o IDP usa ao assinar uma mensagem SAML. |
| Tipo de sincronizar de identidade | `identitySyncType` | ✘ | `default` ou `idp` | `default` | Não altere `from` o padrão de AEM como uma Cloud Service. |
| classificação de serviços | `service.ranking` | ✘ | Número inteiro | `5002` | Configurações de classificação mais altas são preferenciais para o mesmo `path`. |

### atributos usuário AEM{#aem-user-attributes}

O AEM usa os seguintes atributos de usuário, que podem ser preenchidos por meio da propriedade `synchronizeAttributes` na configuração OSGi do Manipulador de autenticação SAML 2.0 do Adobe Granite.  Todos os atributos IDP podem ser sincronizados com qualquer propriedade de usuário do AEM, no entanto, o mapeamento para o AEM usa propriedades de atributo (listadas abaixo) permite que o AEM as use naturalmente.

| Atributo do usuário | Caminho de propriedade relativo do `rep:User` nó |
|--------------------------------|--------------------------|
| Título (por exemplo, `Mrs`) | `profile/title` |
| Nome (ou seja, nome) | `profile/givenName` |
| Nome da família (ou seja, sobrenome) | `profile/familyName` |
| Cargo | `profile/jobTitle` |
| Endereço de e-mail | `profile/email` |
| Endereço | `profile/street` |
| Cidade | `profile/city` |
| Código postal | `profile/postalCode` |
| País | `profile/country` |
| Número de telefone | `profile/phoneNumber` |
| Sobre mim | `profile/aboutMe` |

+++

1. Crie um arquivo de configuração OSGi em seu projeto em `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` e abra-o no IDE.
   + Alterar `/wknd-examples/` para o `/<project name>/`
   + O identificador após o `~` nome do arquivo deve identificar essa configuração de forma exclusiva, por isso pode ser o nome do IDP, como `...~okta.cfg.json`. O valor deve ser alfanumérico com hifens.
1. Colar o seguinte JSON no `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` arquivo e atualize as `wknd` referências conforme necessário.

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. Atualize os valores conforme necessário ao seu projeto. Consulte o glossário __de configuração do__ MANIPULADOR OSGi Authentication do SAML 2.0 acima para obter a configuração propriedade descrições. As `path` árvores de conteúdo devem ser protegidas por Grupos de usuários fechados (CUGs) e necessitam de autenticação e esse manipulador de autenticação deve ser responsável pela proteção.
1. Recomenda-se, mas não é obrigatório, usar OSGi ambiente variáveis e segredos, quando os valores podem mudar de sincronizar com o ciclo de lançamento, ou quando os valores forem diferentes entre tipos e níveis de serviço ambiente semelhantes. Os valores padrão podem ser definidos usando a `$[env:..;default=the-default-value]"` sintaxe como mostrado acima.

As configurações osGi por ambiente (`config.publish.dev` `config.publish.stage`e `config.publish.prod`) podem ser definidas com atributos específicos se a configuração saml varia entre ambientes.

### Usar criptografia

Ao [criptografar a asserção](#encrypting-the-authnrequest-and-saml-assertion) AuthnRequest e SAML, as seguintes propriedades são necessárias: `useEncryption`, `spPrivateKeyAlias`e `keyStorePassword`. Isso `keyStorePassword` contém um senha portanto, o valor não deve ser armazenado no arquivo de configuração OSGi, mas sim injetado usando [valores de configuração secretos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=pt-BR#secret-configuration-values)

+++Opcionalmente, atualize a configuração do OSGi para usar criptografia

1. Abra `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` o IDE.
1. Adicione as três propriedades `useEncryption`, `spPrivateKeyAlias` e `keyStorePassword` conforme mostrado abaixo.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. As três propriedades de configuração do OSGi necessárias para criptografia são:

+ `useEncryption` definido como `true`
+ `spPrivateKeyAlias` contém o alias de entrada do keystore para a chave privada usada pela integração SAML.
+ `keyStorePassword` contém uma [configuração secreta osGi variável](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=pt-BR#secret-configuration-values) contendo os `authentication-service` senha do reposicionamento de chaves do usuário.

+++

## Configurar filtro referenciador

Durante o processo de autenticação SAML, o IDP inicia uma lado do cliente HTTP POST para AEM o ponto final de `.../saml_login` Publish. Se o IDP e o AEM Publish existirem em diferentes origem, a Filtrar do Referenciador da __Publish AEM é__ configurada por meio da configuração OSGi para permitir POSTs HTTP da origem do IDP.

1. Criar (ou edite) um arquivo de configuração OSGi no seu projeto em `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Alterar `/wknd-examples/` para o `/<project name>/`
1. Verifique se o `allow.empty` valor está definido `true`como , a `allow.hosts` (ou se preferir `allow.hosts.regexp`) contém o origem e `filter.methods` inclui `POST`a IDP. A configuração do OSGi deve ser semelhante a:

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish suporta uma única configuração de filtro do Referenciador, portanto, mescle os requisitos de configuração do SAML com quaisquer configurações existentes.

As configurações de OSGi por ambiente (`config.publish.dev`, `config.publish.stage` e `config.publish.prod`) poderão ser definidas com atributos específicos se o `allow.hosts` (ou `allow.hosts.regex`) variar entre ambientes.

## Configurar o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens)

Durante o processo de autenticação SAML, o IDP inicia um POST HTTP do lado do cliente para o ponto de extremidade `.../saml_login` do AEM Publish. Se o IDP e o AEM Publish existirem em hosts/domínios diferentes, o __CORS (CRoss-Origin Resource Sharing) do AEM Publish__ deve ser configurado para permitir POSTs HTTP do host/domínio do IDP.

O cabeçalho `Origin` dessa solicitação HTTP POST geralmente tem um valor diferente do host de publicação do AEM, exigindo a configuração do CORS.

Ao testar a autenticação SAML no AEM SDK local (`localhost:4503`), o IDP pode definir o cabeçalho `Origin` como `null`. Em caso afirmativo, adicione `"null"` à lista `alloworigin`.

1. Crie um arquivo de configuração OSGi em seu projeto em `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Altere `/wknd-examples/` para o nome do seu projeto
   + O identificador depois de `~` no nome do arquivo deve identificar exclusivamente essa configuração, portanto, pode ser o nome do IDP, como `...CORSPolicyImpl~okta.cfg.json`. O valor deve ser alfanumérico com hifens.
1. Colar o seguinte JSON no `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` arquivo.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

As configurações osGi por ambiente (`config.publish.dev` `config.publish.stage`e `config.publish.prod`) podem ser definidas com atributos específicos se elas `alloworigin` variam e `allowedpaths` variam entre ambientes.

## Configurar AEM Dispatcher para permitir POSTs HTTP SAML

Após a autenticação bem-sucedida no IDP, o IDP orquestrará uma POST HTTP de volta ao ponto final registrado `/saml_login` de AEM (configurado no IDP). Esta POST HTTP a `/saml_login` ser bloqueada por padrão em Dispatcher, por isso deve ser explicitamente permitida usando as seguintes Dispatcher regra:

1. Abra `dispatcher/src/conf.dispatcher.d/filters/filters.any` o IDE.
1. Adicione à parte inferior do arquivo uma regra para POSTs HTTP para URLs que terminam com `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Ao implantar várias configurações SAML em AEM para vários caminhos protegidos e endpoints de IDP distintos, assegure-se de que as publicações de IDP no terminal RESPECTIVE_PROTECTED_PATH/saml_fazer logon para selecionar a configuração SAML apropriada no lado AEM. Se houver duplicado configurações SAML para o mesmo caminho protegido, a seleção da configuração SAML ocorrerá aleatoriamente.

Se URL regravação no servidor Web Apache estiver configurado (`dispatcher/src/conf.d/rewrites/rewrite.rules`), certifique-se de que as solicitações para os `.../saml_login` pontos finais não sejam acidentalmente mutiladas.

## Associação de grupo dinâmico

A Associação de Grupo Dinâmica é um recurso no [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) que aumenta o desempenho da avaliação e do provisionamento de grupos. Esta seção descreve como usuários e grupos são armazenados quando este recurso é ativado e como modificar a configuração do Manipulador de autenticação SAML para ativá-lo em ambientes novos ou existentes.

### Como ativar a associação de grupo dinâmico para usuários de SAML em novos ambientes

Para melhorar significativamente grupo desempenho da avaliação em novos AEM como ambientes Cloud Service, o ativação do recurso Associação de Grupo Dinâmico é recomendado em novos ambientes.
Essa também é uma etapa necessária quando a sincronização de dados é ativada. Mais detalhes [aqui](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) .
Para fazer isso, adicione a seguinte propriedade ao arquivo de configuração OSGI:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Com essa configuração, usuários e grupos são criados como [Usuários Externos do Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). No AEM, os usuários e grupos externos têm um `rep:principalName` padrão composto por `[user name];[idp]` ou `[group name];[idp]`.
Observe que as Listas de controle de acesso (ACL) estão associadas ao PrincipalName de usuários ou grupos.
Ao implantar esta configuração em uma implantação existente na qual anteriormente `identitySyncType` não foi especificado ou definido como `default`, novos usuários e grupos serão criados e a ACL deve ser aplicada a esses novos usuários e grupos. Observe que os grupos externos não podem conter usuários locais. [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html) pode ser usado para criar uma ACL para grupos externos SAML, mesmo que eles só sejam criados quando o usuário realizar um logon.
Para evitar essa refatoração na ACL, um [recurso de migração](#automatic-migration-to-dynamic-group-membership-for-existing-environments) padrão foi implementado.

### Como as associações são armazenadas em grupos locais e externos com associação de grupo dinâmica

Em grupos locais, os membros do grupo são armazenados no atributo oak: `rep:members`. O atributo contém a lista de uid de cada membro do grupo. Detalhes adicionais podem ser encontrados [aqui](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
Exemplo:

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

Grupos externos com associação de grupo dinâmicos não armazenamento nenhum membro na entrada de grupo.
A associação de grupo é armazenada nas entradas dos usuários. A documentação adicional pode ser encontrada [aqui](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Por exemplo, esta é a nó OAK para o grupo:

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

Este é o nó de um membro usuário desse grupo:

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### Como ativar a associação de grupo dinâmico para usuários de SAML em ambientes existentes

Como explicado na seção anterior, o formato de usuários e grupos externos é um pouco diferente daquele usado para usuários e grupos locais. É possível definir uma nova ACL para grupos externos e provisionar novos usuários externos ou usar a migração ferramenta conforme descrito abaixo.

#### Ativação de associação de grupo dinâmicos para ambientes existentes com usuários externos

O manipulador Authentication SAML cria usuários externos quando a seguinte propriedade é especificada: `"identitySyncType": "idp"`. Nesse caso, é possível ativar associação de grupo dinâmicos modificando essa propriedade para: `"identitySyncType": "idp_dynamic"`. Nenhuma migração é necessária.

#### Migração automática para associação de grupo dinâmicos em ambientes existentes com usuários locais

O manipulador Authentication SAML cria usuários locais quando a seguinte propriedade é especificada: `"identitySyncType": "default"`. Esse também é o valor padrão quando o propriedade não é especificado. Nesta seção, descrevemos as etapas executadas pelo procedimento de migração automática.

Quando essa migração é habilitada, ela é realizada durante usuário autenticação e consiste nas seguintes etapas:
1. O usuário local é migrado para um usuário externo enquanto mantém o nome de usuário original. Isso implica que os usuários locais migrados, que agora atuam como usuários externos, retêm seu nome de usuário original em vez de seguir a sintaxe de nomenclatura mencionada na seção anterior. Uma propriedade adicional será adicionada chamada: `rep:externalId` com o valor de `[user name];[idp]`. A usuário `PrincipalName` não é modificada.
2. Para cada grupo externo recebido no Asserção de SAML, é criado um grupo externo. Se um grupo local correspondente existir, a grupo externa será adicionada aos grupo locais como membros.
3. A usuário é adicionada como membro da grupo externa.
4. O usuário local é então removido de todos os grupos locais de Saml de que ele era membro. Os grupos locais de Saml são identificados pelo OAK propriedade: `rep:managedByIdp`. Essa propriedade é definida pelo manipulador de Authentication Saml quando o atributo `syncType` não é especificado ou definido como `default`.

Para instância, se antes da migração `user1` for um usuário local e for membro de grupo `group1`local, após a migração ocorrerão as seguintes alterações:
`user1` torna-se um usuário externo. O atributo `rep:externalId` é adicionado ao perfil dele.
`user1`torna-se membro de grupo externos: `group1;idp`não é mais membro direto de grupo locais: `user1` `group1` é um membro do grupo local: `group1;idp`.`group1`

`user1` é, então, um membro do grupo local: `group1` embora a herança

A associação de grupo para grupos externos é armazenada na usuário perfil no propriedade `rep:externalPrincipalNames`

### Configurar a migração automática para o associação de grupo dinâmico

1. Habilite a propriedade `"identitySyncType": "idp_dynamic_simplified_id"` no arquivo de configuração OSGi SAML: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`
2. Configure o novo serviço OSGi com PID de fábrica começando com: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Por exemplo, um PID pode ser: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`. Defina a seguinte propriedade:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Para migrar várias configurações SAML, várias configurações de fábrica OSGi para `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` devem ser criadas, cada uma especificando um `idpIdentifier` para migrar.

## Implantando configuração SAML

As configurações do OSGi devem ser confirmadas no Git e implantadas no AEM as a Cloud Service usando o Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Implante a ramificação Git do Cloud Manager de destino (neste exemplo, `develop`), usando um pipeline de implantação de Empilhamento completo.

## Chamar a autenticação SAML

O fluxo de autenticação SAML pode ser chamado de uma AEM web página, criando links especialmente elaborados ou um botão. Os parâmetros descritos abaixo podem ser definidos de forma programática conforme necessário, portanto, por exemplo, um botão de logon pode definir o `saml_request_path`, que é o local em que o usuário é levado após a autenticação SAML bem-sucedida, para páginas AEM diferentes, com base no contexto do botão.

## Armazenamento em cache seguro ao usar SAML

Na instância de publicação do AEM, a maioria das páginas normalmente é armazenada em cache. No entanto, para caminhos protegidos por SAML, o armazenamento em cache deve ser desativado ou o armazenamento em cache seguro ativado usando a configuração auth_checker. Para obter mais informações, consulte os detalhes fornecidos [aqui](https://experienceleague.adobe.com/pt-br/docs/experience-manager-dispatcher/using/configuring/permissions-cache)

Observe que, se você armazenar em cache caminhos protegidos sem ativar o auth_checker, poderá enfrentar um comportamento imprevisível.

### solicitação GET

A autenticação SAML pode ser invocada criando uma solicitação GET do HTTP no formato:

`HTTP GET /system/sling/login`

e fornecendo parâmetros query:

| Nome do parâmetro de consulta | Valor do parâmetro de consulta |
|----------------------|-----------------------|
| `resource` | Qualquer caminho JCR ou sub-caminho que seja o manipulador de autenticação SAML escuta, conforme definido na [Adobe Systems Granite SAML 2.0 Authentication o propriedade da configuração do](#configure-saml-2-0-authentication-handler) `path` Manipulador OSGi. |
| `saml_request_path` | O URL caminho para o qual o usuário deve ser tomado após a autenticação SAML bem-sucedida. |

Por exemplo, este link HTML acionará o fluxo de login saml e, após o sucesso, leve a usuário para `/content/wknd/us/en/protected/page.html`. Esses query parâmetros podem ser definidos de forma programática, conforme necessário.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## solicitação POST

A autenticação SAML pode ser invocada criando uma solicitação HTTP POST no formato:

`HTTP POST /system/sling/login`

e fornecendo os dados do formulário:

| Nome dos dados do formulário | Valor dos dados do formulário |
|----------------------|-----------------------|
| `resource` | Qualquer caminho JCR ou sub-caminho que seja o manipulador de autenticação SAML escuta, conforme definido na [Adobe Systems Granite SAML 2.0 Authentication o propriedade da configuração do](#configure-saml-2-0-authentication-handler) `path` Manipulador OSGi. |
| `saml_request_path` | O URL caminho para o qual o usuário deve ser tomado após a autenticação SAML bem-sucedida. |


Por exemplo, esse botão do HTML usará um POST HTTP para acionar o fluxo de logon SAML e, caso seja bem-sucedido, levará o usuário para `/content/wknd/us/en/protected/page.html`. Esses parâmetros de dados de formulário podem ser definidos de forma programática, conforme necessário.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Configuração do Dispatcher

Os métodos HTTP GET e POST exigem acesso do cliente aos pontos de extremidade `/system/sling/login` da AEM e, portanto, devem ser permitidos por meio do AEM Dispatcher.

Permitir os padrões de URL necessários com base em se o GET ou POST foi usado

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
