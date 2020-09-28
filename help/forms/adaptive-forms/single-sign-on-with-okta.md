---
title: Configuração do OKTA com AEM
description: Entenda várias configurações para usar logon único usando okta
feature: administration
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
translation-type: tm+mt
source-git-commit: 0b48ae445f4b32deeec08bcb68f805bf19992c9e
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# Autenticar para o autor de AEM usando OKTA

A primeira etapa é configurar seu aplicativo no portal OKTA. Depois que seu aplicativo for aprovado pelo administrador OKTA, você terá acesso ao certificado IdP e ao URL de logon único. Veja a seguir as configurações normalmente usadas no registro do novo aplicativo.

* **Nome do aplicativo:** Este é o nome do seu aplicativo. Certifique-se de atribuir um nome exclusivo ao seu aplicativo.
* **RECIPIENT SAML:** Após a autenticação do OKTA, este é o URL que seria acessado na sua instância AEM com a resposta SAML. Normalmente, o manipulador de autenticação SAML intercepta todos os URLs com / saml_login, mas seria preferível anexá-los depois da raiz do aplicativo.
* **AUDIÊNCIA** SAML: Este é o URL de domínio do seu aplicativo. Não use protocol(http ou https) no URL do domínio.
* **ID do nome SAML:** Selecione Email na lista suspensa.
* **Ambiente**: Escolha seu ambiente apropriado.
* **Atributos**: Esses são os atributos que você obtém sobre o usuário na resposta SAML. Especifique-os de acordo com suas necessidades.


![aplicação de octa](assets/okta-app-settings-blurred.PNG)


## Adicione o certificado OKTA (IdP) ao AEM Trust Store

Como as asserções SAML são criptografadas, precisamos adicionar o certificado IdP (OKTA) ao repositório de confiança AEM para permitir a comunicação segura entre OKTA e AEM.
[Inicializar o repositório](http://localhost:4502/libs/granite/security/content/truststore.html)de confiança, se já não tiver sido inicializado.
Lembre-se da senha do armazenamento confiável. Precisaremos usar essa senha posteriormente neste processo.

* Navegue até Armazenamento [de Confiança](http://localhost:4502/libs/granite/security/content/truststore.html)Global.
* Clique em &quot;Adicionar certificado do arquivo CER&quot;. Adicione o certificado IdP fornecido pelo OKTA e clique em Enviar.

   >[!NOTE]
   >
   >Não mapeie o certificado para nenhum usuário

Ao adicionar o certificado ao armazenamento confiável, você deve obter o alias do certificado, conforme mostrado na captura de tela abaixo. O nome do alias pode ser diferente no seu caso.

![alias do certificado](assets/cert-alias.PNG)

**Anote o alias do certificado. Você precisará disso nas etapas posteriores.**

### Configurar o manipulador de autenticação SAML

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Procure e abra &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Forneça as seguintes propriedades conforme especificado abaixoAs seguintes são as principais propriedades que precisam ser especificadas:

* **path** - Este é o caminho onde o manipulador de autenticação será acionado
* **IdP Url**:Este é o seu url IdP fornecido pelo OKTA
* **Alias** do certificado IDP: este é o alias obtido quando você adicionou o certificado IdP ao repositório de confiança AEM
* **ID** de entidade do provedor de serviço: este é o nome do servidor AEM
* **Senha do armazenamento** de chaves: esta é a senha do armazenamento de confiança que você usou
* **Redirecionamento** padrão: este é o URL para o qual redirecionar na autenticação bem-sucedida
* **Atributo** UserID:uid
* **Usar criptografia**:false
* **Criar usuários** CRX automaticamente:true
* **Adicionar a grupos**:true
* **Grupos** padrão:oktausers (este é o grupo ao qual os usuários serão adicionados. Você pode fornecer qualquer grupo existente dentro do AEM)
* **NamedIDPolicy**: Especifica restrições no identificador de nome a ser usado para representar o assunto solicitado. Copie e cole a seguinte string realçada **urn:oasis:names:tc:SAML:2.0:nameFormat:emailAddress**
* **Atributos** sincronizados - Esses são os atributos que estão sendo armazenados da asserção SAML em AEM perfil

![manipulador de autenticação de saml](assets/saml-authentication-settings-blurred.PNG)

### Configurar o filtro de Quem indicou Apache Sling

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Filtro de Quem indicou Apache Sling&quot;.Defina as seguintes propriedades conforme especificado abaixo:

* **Permitir vazio**: true
* **Permitir hosts**: Nome do host do IdP (será diferente no seu caso)
* **Permitir Host** Regexp: Nome do host do IdP (será diferente no seu caso)A captura de tela das propriedades da Quem indicou do Filtro de Quem indicou Sling

![filtro de quem indicou](assets/sling-referrer-filter.PNG)

#### Configurar o registro DEBUG para a integração OKTA

Ao configurar a integração OKTA no AEM, pode ser útil consultar os registros DEBUG para AEM manipulador de autenticação SAML. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web AEM OSGi.

Lembre-se de remover ou desabilitar esse agente de log no Palco e na Produção para reduzir o ruído do log.

Ao configurar a integração OKTA no AEM, pode ser útil consultar os registros DEBUG para AEM manipulador de autenticação SAML. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web AEM OSGi.
**Lembre-se de remover ou desabilitar esse agente de log no Palco e na Produção para reduzir o ruído do log.**
* Navegue até [configMgr](http://localhost:4502/system/console/configMgr)

* Pesquise e abra &quot;Configuração do Apache Sling Logging Logger&quot;
* Crie um agente de log com a seguinte configuração:
   * **Nível** de log: Depuração
   * **Arquivo** de log: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Clique em salvar para salvar suas configurações



#### Testar sua configuração OKTA

Desconecte-se da sua instância AEM. Tente acessar o link. Você deve ver OKTA SSO em ação.
