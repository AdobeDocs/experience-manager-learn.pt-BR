---
title: Armazenamento e recuperação de dados de formulário do banco de dados MySQL - Servlet para armazenar dados de formulário
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas no armazenamento e na recuperação de dados de formulário
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: dd82f309-dd4e-42ce-8856-e51c898024f5
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '95'
ht-degree: 0%

---

# Servlet para armazenar dados de formulário

A próxima etapa é criar um servlet que irá inserir ou atualizar dados de formulário. O servlet chama os métodos apropriados do serviço OSGi para inserir ou atualizar o banco de dados. Os dados armazenados do formulário adaptável estão associados a um GUID. A mesma GUID é usada para atualizar os dados do formulário. Esse servlet é chamado quando o botão &quot;SaveAndContinueLater&quot; é clicado.

```java
package com.aemforms.saveandcontinue.core.servlets;

import java.io.IOException;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.aemforms.saveandcontinue.core.FetchStoredFormData;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
},
property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdata"
})
public class StoreDataInDB extends SlingAllMethodsServlet {
  private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
  private static final long serialVersionUID = 1L;
  @Reference
  FetchStoredFormData fetchStoredFormData;
  protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    log.debug("Inside my save af data servlet");
    if (request.getParameter("operation").equalsIgnoreCase("update")) {
      log.debug("The operation is update");
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.updateData(request.getParameter("guid"), request.getParameter("formdata"));
      log.debug("The guid I got was  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }
    }

    if (request.getParameter("operation").equalsIgnoreCase("insert")) {
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.storeFormData(request.getParameter("formdata"));
      log.debug("The guid on inserting data  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        log.debug("error in writing response  " + e.getMessage());
      }
    }

  }

}
```
