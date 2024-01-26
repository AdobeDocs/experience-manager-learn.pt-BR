---
title: Preencher o serviço no Adaptive Forms
description: Preencha previamente os formulários adaptáveis buscando dados das fontes de dados de back-end.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
duration: 136
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# Uso do serviço de preenchimento prévio no Forms adaptável

É possível preencher previamente os campos de um formulário adaptável usando dados existentes. Quando um usuário abre um formulário, os valores desses campos são preenchidos previamente. Há várias maneiras de preencher previamente os campos de formulários adaptáveis. Neste artigo, analisaremos o preenchimento prévio de formulários adaptáveis usando o serviço de preenchimento prévio do AEM Forms.

Para saber mais sobre vários métodos para preencher formulários adaptáveis, [siga esta documentação](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Para preencher previamente um formulário adaptável usando o serviço de preenchimento, você deve criar uma classe que implemente a `com.adobe.forms.common.service.DataXMLProvider` interface. O método `getDataXMLForDataRef` terá a lógica de criar e retornar dados que o formulário adaptável consumirá para preencher previamente os campos. Neste método, você pode buscar os dados de qualquer fonte e retornar o fluxo de entrada do documento de dados. O código de exemplo a seguir busca as informações de perfil do usuário conectado e constrói um documento XML cujo fluxo de entrada é retornado para ser consumido pelos formulários adaptáveis.

No trecho de código abaixo, temos uma classe que implementa a interface DataXMLProvider. Obtemos acesso ao usuário conectado e buscamos as informações de perfil do usuário conectado. Em seguida, criamos um documento XML com um elemento de nó raiz chamado &quot;dados&quot; e anexamos elementos apropriados a esse nó de dados. Depois que o documento XML é construído, o fluxo de entrada do documento XML é retornado.

Essa classe é então transformada em um pacote OSGi e implantada no AEM. Depois que o pacote é implantado, esse serviço de preenchimento prévio está disponível para ser usado como serviço de preenchimento do seu Formulário adaptável.

>[!NOTE]
>
>Você pode preencher previamente o formulário usando dados xml ou json usando a abordagem listada neste artigo.

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

Para testar esse recurso no servidor, execute o seguinte procedimento

* Verifique se o está conectado [perfil do usuário](http://localhost:4502/security/users.html) informações são preenchidas. O exemplo procura as propriedades FirstName, LastName e Email do usuário conectado.
* [Baixe e extraia o conteúdo do arquivo zip no seu computador](assets/prefillservice.zip)
* Implante o pacote prefill.core-1.0.0-SNAPSHOT usando o [Console da Web AEM](http://localhost:4502/system/console/bundles)
* Importe o formulário adaptável usando a opção Criar | Upload de arquivo do [Seção FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Verifique se [formulário](http://localhost:4502/editor.html/content/forms/af/prefill.html) está usando **&quot;Serviço de pré-preenchimento personalizado do AEM Forms&quot;** como o serviço de preenchimento prévio. Isso pode ser verificado nas propriedades de configuração do **Contêiner de formulário** seção.
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Você deve ver o formulário preenchido com os valores corretos.

>[!NOTE]
>
>Se você ativou a depuração do com.aem.prefill.core.PrefillAdaptiveForm, o arquivo de dados xml gerado é gravado na pasta de instalação do servidor AEM.

