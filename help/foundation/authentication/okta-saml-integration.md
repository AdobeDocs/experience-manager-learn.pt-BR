---
title: Configuração do OKTA com AEM
description: Entenda várias definições de configuração para usar o logon único usando OKTA.
version: 6.5
topic: Integrations, Security, Administration
feature: Integrations
role: Admin
level: Experienced
jira: KT-12305
last-substantial-update: 2023-03-01T00:00:00Z
doc-type: Tutorial
exl-id: 460e9bfa-1b15-41b9-b8b7-58b2b1252576
duration: 157
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 0%

---

# Autenticar para o autor do AEM usando OKTA

> Consulte [Autenticação SAML 2.0](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/authentication/saml-2-0.html?lang=pt-BR) para obter instruções sobre como configurar o OKTA com o AEM as a Cloud Service.

O primeiro passo é configurar seu aplicativo no portal OKTA. Depois que o aplicativo for aprovado pelo administrador OKTA, você terá acesso ao certificado IdP e à URL de logon único. A seguir estão as configurações normalmente usadas para registrar o novo aplicativo.

* **Nome do Aplicativo:** Este é o nome do seu aplicativo. Atribua um nome exclusivo ao aplicativo.
* **Destinatário SAML:** Após a autenticação do OKTA, esta é a URL que seria acessada na sua instância AEM com a resposta SAML. O manipulador de autenticação SAML normalmente intercepta todos os URLs com / saml_login, mas seria preferível anexá-lo após a raiz do aplicativo.
* **Público-alvo de SAML**: esta é a URL do domínio do seu aplicativo. Não use o protocolo (http ou https) no URL do domínio.
* **ID de Nome SAML:** Selecione Email na lista suspensa.
* **Ambiente**: escolha seu ambiente apropriado.
* **Atributos**: estes são os atributos que você obtém sobre o usuário na resposta SAML. Especifique-os de acordo com suas necessidades.


![aplicativo-okta](assets/okta-app-settings-blurred.PNG)


## Adicionar o certificado OKTA (IdP) ao armazenamento de confiança AEM

Como as asserções SAML são criptografadas, precisamos adicionar o certificado IdP (OKTA) ao armazenamento de confiança do AEM para permitir a comunicação segura entre o OKTA e o AEM.
[Inicializar repositório de confiança](http://localhost:4502/libs/granite/security/content/truststore.html), se ainda não estiver inicializado.
Lembrar a senha do armazenamento de confiança. Precisaremos usar essa senha posteriormente neste processo.

* Navegue até [Repositório Global de Confiança](http://localhost:4502/libs/granite/security/content/truststore.html).
* Clique em &quot;Adicionar certificado do arquivo CER&quot;. Adicione o certificado IdP fornecido pelo OKTA e clique em enviar.

  >[!NOTE]
  >
  >Não mapeie o certificado para nenhum usuário

Ao adicionar o certificado ao armazenamento de confiança, você deve obter o alias do certificado, como mostrado na captura de tela abaixo. O nome do alias pode ser diferente no seu caso.

![Alias de certificado](assets/cert-alias.PNG)

**Anote o alias do certificado. Você precisa disso nas etapas posteriores.**

### Configurar manipulador de autenticação SAML

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Manipulador de autenticação Adobe Granite SAML 2.0&quot;.
Forneça as seguintes propriedades, conforme especificado abaixo
Estas são as propriedades de chave que precisam ser especificadas:

* **caminho** - Este é o caminho em que o manipulador de autenticação é acionado
* **URL do IdP**:Este é o seu URL do IdP fornecido pelo OKTA
* **Alias do Certificado IDP**:Este é o alias que você recebeu quando adicionou o certificado IdP ao repositório de confiança AEM
* **Id da Entidade do Provedor de Serviços**:Este é o nome do seu Servidor AEM
* **Senha do repositório de chaves**:Esta é a senha do repositório de confiança que você usou
* **Redirecionamento Padrão**:Esta é a URL para redirecionar na autenticação bem-sucedida
* **Atributo UserID**:uid
* **Usar Criptografia**:false
* **Criar Usuários do CRX Automaticamente**:true
* **Adicionar aos Grupos**:true
* **Grupos Padrão**:oktausers(Este é o grupo ao qual os usuários são adicionados. Você pode fornecer qualquer grupo existente dentro de AEM)
* **NamedIDPolicy**: especifica restrições no identificador de nome a ser usado para representar o assunto solicitado. Copie e cole a seguinte cadeia de caracteres destacada **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Atributos Sincronizados** - Estes são os atributos que estão sendo armazenados da asserção SAML no perfil AEM

![manipulador-de-autenticação-saml](assets/saml-authentication-settings-blurred.PNG)

### Configurar o filtro referenciador do Apache Sling

Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
Pesquise e abra &quot;Filtro referenciador do Apache Sling&quot;. Defina as seguintes propriedades conforme especificado abaixo:

* **Permitir Vazio**: falso
* **Permitir hosts**: nome de host do IdP (no seu caso, é diferente)
* **Permitir Host Regexp**: nome de host do IdP (no seu caso, é diferente)
A captura de tela das propriedades do Referenciador de filtro do Sling

![referrer-filter](assets/okta-referrer.png)

#### Configurar o log DEBUG para a integração OKTA

Ao configurar a integração OKTA no AEM, pode ser útil revisar os logs DEBUG do manipulador de autenticação SAML do AEM. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web AEM OSGi.

Lembre-se de remover ou desativar esse agente de log no Palco e na Produção para reduzir o ruído de log.

Ao configurar a integração OKTA no AEM, pode ser útil revisar os logs DEBUG do manipulador de autenticação SAML do AEM. Para definir o nível de log como DEBUG, crie uma nova configuração do Sling Logger por meio do console da Web AEM OSGi.
**Lembre-se de remover ou desabilitar este agente de log no Preparo e na Produção para reduzir o ruído do log.**
* Navegue até [configMgr](http://localhost:4502/system/console/configMgr)

* Pesquise e abra &quot;Configuração do logger de log do Apache Sling&quot;
* Crie um agente de log com a seguinte configuração:
   * **Nível de log**: depuração
   * **Arquivo de Log**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Clique em Salvar para salvar as configurações

#### Testar a configuração OKTA

Faça logoff da sua instância do AEM. Tente acessar o link. Você deve ver o SSO OKTA em ação.
