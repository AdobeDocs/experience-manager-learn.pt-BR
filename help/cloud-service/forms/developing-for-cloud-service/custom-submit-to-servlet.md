---
title: Criar manipulador de ação de envio personalizado
description: Envio de um formulário adaptável a um manipulador de envio personalizado
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Criar servlet para processar os dados enviados

Inicie seu projeto aem-banking no IntelliJ.
Crie um servlet simples para enviar os dados enviados para o arquivo de log. Verifique se o código está no projeto principal, conforme mostrado na captura de tela abaixo
![criar-servlet](assets/create-servlet.png)

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

## Criar manipulador de envio personalizado

Crie sua ação de envio personalizada na pasta `apps/bankingapplication` da mesma forma que criaria nas [versões anteriores do AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=pt-BR). Para fins deste tutorial, crio uma pasta chamada SubmitToAEMServlet no nó `apps/bankingapplication` no repositório do CRX.

O código a seguir no post.POST.jsp simplesmente encaminha a solicitação para o servlet montado em /bin/formstutorial. Este é o mesmo servlet que foi criado na etapa anterior

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

No projeto do AEM no IntelliJ, clique com o botão direito do mouse na pasta `apps/bankingapplication` e selecione Novo | Empacote e digite SubmitToAEMervlet após o aplicativo apps.bankingna caixa de diálogo novo pacote. Clique com o botão direito do mouse no nó SubmitToAEMervlet e selecione repo | Obtenha o comando para sincronizar o projeto do AEM com o repositório do servidor do AEM.


## Configurar formulário adaptável

Agora você pode configurar qualquer Formulário adaptável para enviar a este manipulador de envio personalizado chamado **Enviar ao AEM Servlet**

## Próximas etapas

[Registrar servlet usando tipo de recurso](./registering-servlet-using-resourcetype.md)
