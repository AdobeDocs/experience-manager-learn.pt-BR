---
title: Criar seu primeiro servlet no AEM Forms
description: Crie seu primeiro servlet sling para mesclar dados com o modelo de formulário.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# Sling Servlet

Um Servlet é uma classe usada para estender os recursos dos servidores que hospedam aplicativos acessados por meio de um modelo de programação de solicitação-resposta. Para tais aplicações, a tecnologia Servlet define classes de servlet específicas de HTTP.
Todos os servlets devem implementar a interface Servlet, que define os métodos do ciclo de vida.


Um servlet no AEM pode ser registrado como um serviço OSGi: é possível estender SlingSafeMethodsServlet para implementação somente leitura ou SlingAllMethodsServlet para implementar todas as operações RESTful.

## Código de servlet

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## Criar e implantar

Para criar seu projeto, siga as seguintes etapas:

* Abertura **janela da tela de comandos**
* Vá até `c:\aemformsbundles\mysite\core`
* Executar o comando `mvn clean install -PautoInstallBundle`
* O comando acima cria e implanta automaticamente o pacote na instância do AEM em execução em localhost:4502

O pacote também está disponível no seguinte local `C:\AEMFormsBundles\mysite\core\target`. O pacote também pode ser implantado no AEM usando o [Felix web console.](http://localhost:4502/system/console/bundles)


## Testar o resolvedor de servlet

Aponte seu navegador para a [URL do resolvedor de servlet](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Isso informa o servlet que é chamado para um determinado caminho, conforme visto na captura de tela abaixo
![servlet-resolver](assets/servlet-resolver.JPG)

## Testar o servlet usando o Postman

![Testar o servlet usando o Postman](assets/test-servlet-postman.JPG)

## Próximas etapas

[Incluir jar de terceiros](./include-third-party-jars.md)

