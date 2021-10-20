---
title: Criando manipulador de ação de envio personalizado
description: Envio de formulário adaptável a um manipulador de envio personalizado
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8852
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Criar servlet para processar os dados enviados

Inicie seu projeto aem-banking no IntelliJ.
Crie um servlet simples para exibir os dados enviados para o arquivo de log. Verifique se o código está no projeto principal, como mostrado na captura de tela abaixo
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## Criar envio personalizado

Crie seu envio personalizado na pasta aplicativo/banco da mesma forma que você criaria na [versões anteriores do AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
O código a seguir no post.POST.jsp simplesmente encaminha a solicitação para o servlet montado em /bin/formstutorial. Este é o mesmo servlet que foi criado na etapa anterior

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## Configurar formulário adaptável

Agora você pode configurar o Formulário adaptável para enviar para este manipulador de envio personalizado chamado **Enviar para AEM Servlet**



