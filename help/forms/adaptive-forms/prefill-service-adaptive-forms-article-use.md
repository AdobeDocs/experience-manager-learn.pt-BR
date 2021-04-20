---
title: Serviço de preenchimento prévio em formulários adaptáveis
seo-title: Serviço de preenchimento prévio em formulários adaptáveis
description: Preencha previamente formulários adaptáveis buscando dados de fontes de dados de backend.
seo-description: Preencha previamente formulários adaptáveis buscando dados de fontes de dados de backend.
sub-product: formulários
feature: Adaptive Forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# Uso do serviço de preenchimento prévio em formulários adaptáveis

É possível preencher previamente os campos de um formulário adaptável usando dados existentes. Quando um usuário abre um formulário, os valores desses campos são preenchidos previamente. Existem várias maneiras de preencher previamente campos de formulários adaptáveis. Neste artigo, observaremos o preenchimento prévio de formulários adaptáveis usando o serviço de preenchimento prévio do AEM Forms.

Para saber mais sobre vários métodos para preencher previamente formulários adaptáveis, [siga esta documentação](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Para preencher previamente o formulário adaptável usando o serviço de preenchimento prévio, é necessário criar uma classe que implemente a interface do DataProvider. O método getPrefillData terá a lógica de criar e retornar dados que o formulário adaptável consumirá para pré-preencher os campos. Nesse método, é possível buscar os dados de qualquer origem e retornar o fluxo de entrada do documento de dados. O código de exemplo a seguir busca as informações do perfil do usuário do usuário conectado e constrói um documento XML cujo fluxo de entrada é retornado para ser consumido pelos formulários adaptáveis.

No trecho de código abaixo, temos uma classe que implementa a interface do DataProvider. Obtemos acesso ao usuário conectado e, em seguida, obtemos as informações de perfil do usuário conectado. Em seguida, criamos um documento XML com um elemento de nó raiz chamado &quot;dados&quot; e anexamos os elementos apropriados a esse nó de dados. Depois que o documento XML é construído, o fluxo de entrada do documento XML é retornado.

Essa classe é então transformada em pacote OSGi e implantada no AEM. Depois que o pacote é implantado, esse serviço de preenchimento prévio fica disponível para ser usado como serviço de preenchimento prévio do Formulário adaptável.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

Para testar esse recurso em seu servidor, execute o seguinte procedimento

* [Baixe e extraia o conteúdo do arquivo zip no seu computador](assets/prefillservice.zip)
* Certifique-se de que as informações de perfil do usuário [logadas estão preenchidas completamente. ](http://localhost:4502/libs/granite/security/content/useradmin) Isso é necessário para que a amostra funcione. A amostra não tem nenhum erro ao verificar propriedades ausentes do perfil de usuário.
* Implante o pacote usando o [console da Web do AEM](http://localhost:4502/system/console/bundles)
* Criar formulário adaptável usando o XSD
* Associe &quot;Serviço de pré-preenchimento de formulário do Aem personalizado&quot; como o serviço de pré-preenchimento do formulário adaptável
* Arraste e solte elementos do esquema no formulário
* Visualizar o formulário

>[!NOTE]
>
>Se o formulário adaptável for baseado em XSD, verifique se o documento XML retornado pelo serviço de preenchimento corresponde ao XSD no qual o formulário adaptável se baseia.
>
>Se o formulário adaptável não for baseado em XSD, será necessário vincular os campos manualmente. Por exemplo, para vincular um campo de formulário adaptável ao elemento fname nos dados XML, você usará `/data/fname` na Referência de vínculo do campo de formulário adaptável.

