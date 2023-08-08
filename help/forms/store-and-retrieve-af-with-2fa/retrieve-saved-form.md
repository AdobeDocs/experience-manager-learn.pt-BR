---
title: Recuperar formulário adaptável salvo
description: Servlet para renderizar o formulário adaptável com dados salvos
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6553
thumbnail: 6553.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d722cb9c-6c8a-44de-aaea-fc07a555b864
source-git-commit: 51e21c11df63d33a6900fbc331a756f2a7655bcb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 1%

---

# Recuperar formulário salvo

A próxima etapa é criar um servlet que renderizará o formulário adaptável com os dados salvos e seus anexos.
O código de servlet a seguir é executado depois que o código OTP é verificado. Os dados do formulário adaptável e seu mapa de anexos de arquivo associado à ID exclusiva do aplicativo são buscados no banco de dados. O objeto de solicitação é preenchido com os dados salvos do formulário adaptável e o mapa de anexos de arquivo. A solicitação é então encaminhada para renderizar o formulário &quot;storeafwithattachments&quot; pré-preenchido com os dados originais e seus anexos.

```java
import java.io.IOException;
import java.io.StringReader;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.xml.sax.InputSource;

import com.google.gson.JsonObject;
import com.saveAndResume.core.SaveAndFetchDataFromDB;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/renderaf"
})

public class RenderFormWithDataAndAttachments extends SlingAllMethodsServlet {
  @Reference
  SaveAndFetchDataFromDB saveAndFetchFromDB;
  Logger log = LoggerFactory.getLogger(this.getClass());

  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
    try {
      log.debug("Inside w3cDocumentFromString");
      DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
      InputSource is = new InputSource();
      is.setCharacterStream(new StringReader(xmlString));
      return db.parse(is);
    } catch (Exception e) {
      log.error(e.getMessage());
      return null;
    }
  }
  protected void doPost(final SlingHttpServletRequest request, final SlingHttpServletResponse response) throws ServletException {
    log.debug("In do POST ######");
    String applicationNo = "/afData/afUnboundData/data/ApplicationNumber";
    String submittedData = request.getParameter("jcr:data");
    Document submittedXml = this.w3cDocumentFromStrng(submittedData);
    XPath xPath = XPathFactory.newInstance().newXPath();
    Enumeration < String > params = request.getParameterNames();
    while (params.hasMoreElements()) {
      String paramName = params.nextElement();
      log.debug("The param Name is " + paramName);
      String data = request.getParameter(paramName);
      log.debug("The data  is " + data);
    }

    SyntheticSlingHttpServletGetRequest syntheticRequest = new SyntheticSlingHttpServletGetRequest(request);
    try {
      Node applicationNode = (Node) xPath.compile(applicationNo).evaluate(submittedXml, XPathConstants.NODE);
      log.debug("The application number we got was " + applicationNode.getTextContent());
      JsonObject afDataObject = saveAndFetchFromDB.getAFFormDataWithAttachments(applicationNode.getTextContent());
      log.debug("$$$$ The data that is set in request object is " + afDataObject.get("afData").getAsString());
      request.setAttribute("data", afDataObject.get("afData").getAsString());
      JsonObject customMap = new JsonObject();
      customMap.addProperty("fileAttachmentMap", afDataObject.get("afAttachments").getAsString());
      request.setAttribute("customContextProperty", customMap.toString());
      request.getRequestDispatcher("/content/forms/af/storeafwithattachments.html").forward(syntheticRequest, response);
    } catch (ServletException | IOException | XPathExpressionException exception) {

       log.error(exception.getMessage());
    }
  }

}
```

## SyntheticSlingHttpServletGetRequest

```java
package saveandresume.core.servlets;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.wrappers.SlingHttpServletRequestWrapper;

public class SyntheticSlingHttpServletGetRequest extends SlingHttpServletRequestWrapper {
    private static final String METHOD_GET = "GET";

    public SyntheticSlingHttpServletGetRequest(final SlingHttpServletRequest request) {
        super(request);
    }

    @Override
    public String getMethod() {
        return METHOD_GET;
    }
}
```


## Próximas etapas

[Criar biblioteca cliente para chamar o servlet para armazenar dados de formulário](./create-client-lib.md)
