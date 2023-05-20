---
title: Configuração do OKTA com AEM
description: Entender várias definições de configuração para usar o logon único usando o okta
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 1%

---

# Autenticar para o autor do AEM usando OKTA

O primeiro passo é configurar seu aplicativo no portal OKTA. Depois que o aplicativo for aprovado pelo administrador OKTA, você terá acesso ao certificado IdP e à URL de logon único. A seguir estão as configurações normalmente usadas para registrar o novo aplicativo.

* **Nome do aplicativo:** Este é o nome do seu aplicativo. Atribua um nome exclusivo ao aplicativo.
* **Destinatário SAML:** Após a autenticação do OKTA, este é o URL que seria acessado em sua instância AEM com a resposta SAML. O manipulador de autenticação SAML normalmente intercepta todos os URLs com / saml_login, mas seria preferível anexá-lo após a raiz do aplicativo.
* **Público-alvo de SAML**: este é o URL do domínio do seu aplicativo. Não use o protocolo (http ou https) no URL do domínio.
* **ID do nome SAML:** Selecione Email na lista suspensa.
* **Ambiente**: escolha o ambiente apropriado.
* **Atributos**: estes são os atributos que você obtém sobre o usuário na resposta SAML. Especifique-os de acordo com suas necessidades.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Adicionar o certificado OKTA (IdP) ao armazenamento de confiança AEM

Como as asserções SAML são criptografadas, precisamos adicionar o certificado IdP (OKTA) ao armazenamento de confiança do AEM para permitir a comunicação segura entre o OKTA e o AEM.
[Inicializar armazenamento de confiança](http://localhost:4502/libs/granite/security/content/truststore.html), se ainda não tiver sido inicializado.
Lembrar a senha do armazenamento de confiança. Precisaremos usar essa senha posteriormente neste processo.

* Navegue até [Armazenamento global de confiança](http://localhost:4502/libs/granite/security/content/truststore.html).
* Clique em &quot;Adicionar certificado do arquivo CER&quot;. Adicione o certificado IdP fornecido pelo OKTA e clique em enviar.

   >[!NOTE]
   >
   >Não mapeie o certificado para nenhum usuário

Ao adicionar o certificado ao armazenamento confiável, você deve obter o alias do certificado conforme mostrado na captura de tela abaixo. O nome do alias pode ser diferente no seu caso.

![Alias de certificado](assets/cert-alias.PNG)

**Anote o alias do certificado. Você precisará disso nas etapas posteriores.**

### Configurar manipulador de autenticação SAML

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Manipulador de autenticação Adobe Granite SAML 2.0&quot;.
Forneça as seguintes propriedades conforme especificado abaixo As propriedades principais que precisam ser especificadas são as seguintes:

* **caminho** - Esse é o caminho em que o manipulador de autenticação é acionado
* **URL do IdP**: Este é o URL do seu IdP fornecido pelo OKTA
* **Alias do certificado IDP**:Este é o alias que você recebeu quando adicionou o certificado IdP ao AEM trust store
* **ID da entidade do provedor de serviços**:Este é o nome do seu Servidor AEM
* **Senha do armazenamento de chaves**: esta é a senha do armazenamento de confiança que você usou
* **Redirecionamento padrão**:Este é o URL para redirecionar na autenticação bem-sucedida
* **Atributo UserID**:uid
* **Usar criptografia**:false
* **Criar automaticamente usuários do CRX**:true
* **Adicionar a grupos**:true
* **Grupos padrão**:oktausers(Este é o grupo ao qual os usuários são adicionados. Você pode fornecer qualquer grupo existente dentro de AEM)
* **NamedIDPolicy**: especifica restrições no identificador de nome a ser usado para representar o assunto solicitado. Copie e cole a seguinte cadeia de caracteres destacada **urn:oasis:nomes:tc:SAML:2.0:nameidformat:emailAddress**
* **Atributos Sincronizados** - Esses são os atributos que estão sendo armazenados da asserção SAML no perfil AEM

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configurar o filtro referenciador do Apache Sling

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Filtro referenciador do Apache Sling&quot;. Defina as seguintes propriedades conforme especificado abaixo:

* **Permitir vazio**: falso
* **Permitir hosts**: nome de host do IdP (no seu caso, é diferente)
* **Permitir host de expressão regular**: nome de host do IdP (diferente no seu caso) Captura de tela das propriedades do referenciador de filtro do Sling

![referrer-filter](assets/okta-referrer.png)

#### Configurar o log DEBUG para a integração OKTA

Ao configurar a integração OKTA no AEM, pode ser útil revisar os logs DEBUG para o manipulador de autenticação AEM SAML. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web AEM OSGi.

Lembre-se de remover ou desativar esse agente de log no Palco e na Produção para reduzir o ruído de log.

Ao configurar a integração OKTA no AEM, pode ser útil revisar os logs DEBUG para o manipulador de autenticação AEM SAML. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web AEM OSGi.
**Lembre-se de remover ou desativar esse agente de log no Palco e na Produção para reduzir o ruído de log.**
* Navegue até [configMgr](http://localhost:4502/system/console/configMgr)

* Pesquise e abra &quot;Configuração do logger de log do Apache Sling&quot;
* Crie um agente de log com a seguinte configuração:
   * **Nível de registro**: Depuração
   * **Arquivo de log**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Clique em Salvar para salvar as configurações

#### Testar a configuração OKTA

Faça logoff da sua instância do AEM. Tente acessar o link. Você deve ver o SSO OKTA em ação.
