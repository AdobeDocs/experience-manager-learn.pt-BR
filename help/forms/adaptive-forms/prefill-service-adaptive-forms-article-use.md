---
title: Serviço de pré-preenchimento no Forms adaptável
seo-title: Serviço de pré-preenchimento no Forms adaptável
description: Preencha previamente formulários adaptáveis buscando dados de fontes de dados de backend.
seo-description: Preencha previamente formulários adaptáveis buscando dados de fontes de dados de backend.
sub-product: formulários
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Uso do serviço Prefill no Forms adaptável

É possível pré-preencher os campos de um formulário Adaptive usando dados existentes. Quando um usuário abre um formulário, os valores desses campos são preenchidos previamente. Há várias maneiras de preencher previamente campos de formulários adaptáveis. Neste artigo, observaremos o preenchimento prévio do formulário adaptável usando o serviço de preenchimento prévio da AEM Forms.

Para saber mais sobre vários métodos para preencher previamente formulários adaptáveis, [siga esta documentação](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Para preencher previamente o formulário adaptável usando o serviço de preenchimento prévio, será necessário criar uma classe que implemente a interface do DataProvider. O método getPrefillData terá a lógica de criar e retornar dados que o formulário adaptativo consumirá para pré-preencher os campos. Nesse método, é possível buscar os dados de qualquer fonte e retornar o fluxo de entrada do documento de dados. O exemplo de código a seguir busca as informações do perfil do usuário do usuário conectado e constrói um documento XML cujo fluxo de entrada é retornado para ser consumido pelos formulários adaptáveis.

No trecho de código abaixo temos uma classe que implementa a interface do DataProvider. Obtemos acesso ao usuário conectado e, em seguida, obtemos as informações de perfil do usuário conectado. Em seguida, criamos um documento XML com um elemento de nó raiz chamado &quot;dados&quot; e adicionamos os elementos apropriados a esse nó de dados. Depois que o documento XML é construído, o fluxo de entrada do documento XML é retornado.

Essa classe é então transformada em um pacote OSGi e implantada no AEM. Depois que o pacote é implantado, esse serviço de preenchimento prévio fica disponível para ser usado como serviço de preenchimento prévio do Formulário adaptável.

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

Para testar esse recurso no servidor, execute o seguinte procedimento:

* [Baixe e extraia o conteúdo do arquivo zip no seu computador](assets/prefillservice.zip)
* Verifique se as informações de perfil [do](http://localhost:4502/libs/granite/security/content/useradmin) usuário conectado foram preenchidas completamente. Isso é necessário para que a amostra funcione. A amostra não tem nenhuma verificação de erro para propriedades de perfil de usuário ausentes.
* Implantar o pacote usando o console da Web [AEM](http://localhost:4502/system/console/bundles)
* Criar formulário adaptável usando o XSD
* Associe o &quot;Serviço de preenchimento prévio de formulário personalizado&quot; como o serviço de preenchimento prévio do formulário adaptável
* Arraste e solte elementos do schema no formulário
* Visualizar o formulário

>[!NOTE]
>
>Se o formulário adaptável for baseado em XSD, verifique se o documento XML retornado pelo serviço de preenchimento prévio corresponde ao XSD no qual o formulário adaptável se baseia.
>
>Se o formulário adaptável não for baseado em XSD, então será necessário vincular os campos manualmente. Por exemplo, para vincular um campo de formulário adaptável ao elemento fname nos dados XML, você usará `/data/fname` a referência Bind do campo de formulário adaptável.

