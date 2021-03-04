---
title: Configuração do OKTA com o AEM
description: Entender várias configurações para usar o logon único usando o okta
feature: Formulários adaptáveis
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
topic: Administração
role: Administrador
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 0%

---


# Autenticar para o autor do AEM usando OKTA

O primeiro passo é configurar seu aplicativo no portal OKTA. Depois que seu aplicativo for aprovado pelo administrador OKTA, você terá acesso ao certificado IdP e ao URL de logon único. Veja a seguir as configurações normalmente usadas no registro do novo aplicativo.

* **Nome do aplicativo:** esse é o nome do seu aplicativo. Dê um nome exclusivo ao aplicativo.
* **Recipient SAML:** após a autenticação de OKTA, esse é o URL que seria acessado em sua instância do AEM com a resposta SAML. O manipulador de autenticação SAML normalmente intercepta todos os URLs com / saml_login, mas seria preferível anexá-los depois da raiz do aplicativo.
* **Público-alvo** SAML: Este é o URL de domínio do seu aplicativo. Não use o protocolo (http ou https) no URL do domínio.
* **ID de nome de SAML:** selecione Email na lista suspensa.
* **Ambiente**: Escolha o ambiente apropriado.
* **Atributos**: Esses são os atributos que você obtém sobre o usuário na resposta do SAML. Especifique-as de acordo com suas necessidades.


![aplicação de okta](assets/okta-app-settings-blurred.PNG)


## Adicionar o certificado OKTA (IdP) ao armazenamento confiável do AEM

Como as asserções de SAML são criptografadas, precisamos adicionar o certificado IdP (OKTA) ao armazenamento confiável do AEM, para permitir a comunicação segura entre OKTA e AEM.
[Inicializar armazenamento de confiança](http://localhost:4502/libs/granite/security/content/truststore.html), se ainda não tiver sido inicializado.
Lembre-se da senha do repositório de confiança. Precisaremos usar essa senha posteriormente neste processo.

* Navegue até [Armazenamento de Confiança Global](http://localhost:4502/libs/granite/security/content/truststore.html).
* Clique em &quot;Adicionar certificado do arquivo CER&quot;. Adicione o certificado IdP fornecido pelo OKTA e clique em enviar.

   >[!NOTE]
   >
   >Não mapeie o certificado para nenhum usuário

Ao adicionar o certificado ao armazenamento confiável, você deve obter o alias do certificado, conforme mostrado na captura de tela abaixo. O nome do alias pode ser diferente no seu caso.

![Alias do certificado](assets/cert-alias.PNG)

**Anote o alias do certificado. Você precisará disso nas etapas posteriores.**

### Configurar o manipulador de autenticação SAML

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Manipulador de autenticação do Adobe Granite SAML 2.0&quot;.
Forneça as seguintes propriedades conforme especificado abaixo
Estas são as propriedades principais que precisam ser especificadas:

* **caminho**  - Esse é o caminho onde o manipulador de autenticação será acionado
* **IdP Url**: este é o URL IdP fornecido por OKTA
* **Alias** do certificado IDP: é o alias obtido ao adicionar o certificado IdP ao armazenamento confiável do AEM
* **ID da entidade do provedor de serviços**: esse é o nome do seu servidor AEM
* **Senha do armazenamento** de chaves: esta é a senha do armazenamento de confiança que você usou
* **Redirecionamento padrão**: este é o URL para o qual redirecionar na autenticação bem-sucedida
* **Atributo** UserID:uid
* **Usar criptografia**: false
* **Criar automaticamente usuários** do CRX:true
* **Adicionar a Grupos**:true
* **Grupos** padrão: oktausers (esse é o grupo ao qual os usuários serão adicionados. Você pode fornecer qualquer grupo existente no AEM)
* **NamedIDPolicy**: Especifica restrições no identificador de nome a ser usado para representar o assunto solicitado. Copie e cole a seguinte string realçada **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Atributos sincronizados**  - esses são os atributos que estão sendo armazenados a partir da asserção de SAML no perfil do AEM

![manipulador de autenticação de saml](assets/saml-authentication-settings-blurred.PNG)

### Configurar o filtro de referência do Apache Sling

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Filtro de referenciador do Apache Sling&quot;.Defina as seguintes propriedades conforme especificado abaixo:

* **Permitir vazio**: true
* **Permitir Hosts**: Nome do host do IdP (será diferente no seu caso)
* **Permitir Host** Regexp: Nome do host do IdP (Será diferente no seu caso) Captura de tela Propriedades do Referenciador de Filtro do Referenciador do Sling

![referrer-filter](assets/sling-referrer-filter.PNG)

#### Configurar o registro DEBUG para a integração OKTA

Ao configurar a integração OKTA no AEM, pode ser útil consultar os logs DEBUG do manipulador de autenticação SAML do AEM. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web OSGi do AEM.

Lembre-se de remover ou desativar esse logger no Stage e na Production para reduzir o ruído de log.

Ao configurar a integração OKTA no AEM, pode ser útil consultar os logs DEBUG do manipulador de autenticação SAML do AEM. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web OSGi do AEM.
**Lembre-se de remover ou desativar esse logger no Stage e na Production para reduzir o ruído de log.**
* Navegue até [configMgr](http://localhost:4502/system/console/configMgr)

* Pesquise e abra &quot;Configuração do Apache Sling Logging Logger&quot;
* Crie um logger com a seguinte configuração:
   * **Nível** de log: Depurar
   * **Arquivo** de log: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Clique em Salvar para salvar suas configurações



#### Testar sua configuração OKTA

Faça logoff da sua instância do AEM. Tente acessar o link. Você deve ver o SSO OKTA em ação.
