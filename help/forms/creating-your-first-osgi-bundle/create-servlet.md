---
title: Criar seu primeiro servlet no AEM Forms
description: Crie seu primeiro servlet sling para unir dados ao modelo de formulário.
feature: Formulários adaptáveis
version: 6.4,6.5
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 2%

---


# Servlet Sling

Um Servlet é uma classe usada para estender os recursos de servidores que hospedam aplicativos acessados por meio de um modelo de programação de resposta de solicitação. Para esses aplicativos, a tecnologia Servlet define classes de servlet específicas para HTTP.
Todos os servlets devem implementar a interface Servlet, que define métodos de ciclo de vida.


Um servlet no AEM pode ser registrado como serviço OSGi: você pode estender SlingSafeMethodsServlet para implementação somente leitura ou SlingAllMethodsServlet para implementar todas as operações RESTful.

## Código Servlet

```java
import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

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

* Abrir **janela de prompt de comando**
* Vá até `c:\aemformsbundles\learningaemforms\core`
* Execute o comando `mvn clean install -PautoInstallBundle`
* O comando acima criará e implantará automaticamente o pacote em sua instância de AEM em execução no localhost:4502

O pacote também estará disponível no seguinte local `C:\AEMFormsBundles\learningaemforms\core\target`. O pacote também pode ser implantado em AEM usando o [console da Web Felix.](http://localhost:4502/system/console/bundles)


## Testar o Servlet Resolver

Aponte seu navegador para o [URL do resolvedor de servlet](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Isso informará o servlet que será chamado para um determinado caminho, como visto na captura de tela abaixo
![servlet-resolver](assets/servlet-resolver.JPG)

## Teste o servlet usando o Postman

![test-servlet-postman](assets/test-servlet-postman.JPG)
