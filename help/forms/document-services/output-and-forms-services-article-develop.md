---
title: Desenvolvimento com serviços de saída e Forms na AEM Forms
seo-title: Desenvolvimento com serviços de saída e Forms na AEM Forms
description: Usando a Saída e a API de serviço da Forms no AEM Forms
seo-description: Usando a Saída e a API de serviço da Forms no AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# Desenvolvimento com serviços de saída e Forms na AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Usando a Saída e a API de serviço da Forms no AEM Forms

Neste artigo, examinaremos o seguinte

* Serviço de saída - Normalmente, esse serviço é usado para unir dados xml ao modelo xdp ou pdf para gerar um pdf nivelado
* FormsService - este é um serviço muito versátil que permite exportar/importar dados de e para um arquivo PDF

O javadoc oficial da API AEM Forms está listado [aqui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

O trecho de código a seguir exporta dados do arquivo PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

A linha 1 extrai o pdffile da solicitação

A Linha2 extrai o saveLocation da solicitação

A Linha 5 obtém o FormsService

A linha 6 exporta o xmlData do arquivo PDF

**Para testar o pacote de amostra em seu sistema**

[Baixe e instale o pacote usando o gerenciador de pacote AEM](assets/outputandformsservice.zip)




**Depois de instalar o pacote, você terá que lista de permissões os seguintes URLs no Filtro CSRF do Adobe Granite.**

1. Siga as etapas abaixo para lista de permissões os caminhos mencionados acima.
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procurar o Filtro CSRF Adobe Granite
1. Adicione os três caminhos a seguir nas seções excluídas e salve
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Procurar por &quot;filtro Quem indicou Sling&quot;
1. Marque a caixa de seleção &quot;Permitir vazio&quot;. (Essa configuração deve ser somente para fins de teste)Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite que você faça solicitações de POST para seu servidor. Instale o aplicativo Postman no seu sistema.
Inicie o aplicativo e insira o seguinte URL para testar a API de dados de exportação

Certifique-se de ter selecionado &quot;POST&quot; na lista suspensahttp://localhost:4502/content/AemFormsSamples/exportdata.htmlão de especificar &quot;Autorização&quot; como &quot;Autenticação básica&quot;. Especifique o nome de usuário e a senha do AEM ServerNavegue até a guia &quot;Corpo&quot; e especifique os parâmetros de solicitação, conforme mostrado na imagem abaixo![de exportar](assets/postexport.png)Em seguida, clique no botão Enviar

A embalagem contém 3 amostras. Os parágrafos a seguir explicam quando usar o serviço de saída ou o Forms Service, o url do serviço, os parâmetros de entrada que cada serviço espera

**Mesclar dados e Achatar saída:**

* Use o serviço de saída para unir dados ao documento xdp ou pdf para gerar um pdf nivelado
* **POST URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parâmetros de solicitação -**

   * xdp_or_pdf_file : O arquivo xdp ou pdf com o qual você deseja unir dados
   * xmlfile: O arquivo de dados xml que será mesclado com xdp_or_pdf_file
   * saveLocation: O local para salvar o documento renderizado em seu sistema de arquivos

**Importar dados para o arquivo PDF:**
* Usar o FormsService para importar dados para um arquivo PDF
* **URL** do POST - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parâmetros de solicitação:**

   * pdffile: O arquivo pdf com o qual você deseja unir dados
   * xmlfile: O arquivo de dados xml que será unido ao arquivo pdf
   * saveLocation: O local para salvar o documento renderizado em seu sistema de arquivos. Por exemplo, c:\\\outputsample.pdf.

**Exportar dados do arquivo PDF**
* Usar o FormsService para exportar dados do arquivo PDF
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parâmetros de solicitação:**

   * pdffile: O arquivo pdf do qual você deseja exportar dados
   * saveLocation: O local para salvar os dados exportados em seu sistema de arquivos

[É possível importar essa coleção de cartazes para testar a API](assets/document-services-postman-collection.json)

