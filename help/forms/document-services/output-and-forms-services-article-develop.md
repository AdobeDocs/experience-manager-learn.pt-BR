---
title: Desenvolvimento com serviços de saída e Forms no AEM Forms
seo-title: Desenvolvimento com serviços de saída e Forms no AEM Forms
description: Uso da API de serviço do Output e Forms no AEM Forms
seo-description: Uso da API de serviço do Output e Forms no AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: Serviço de saída
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 67be45dbd72a8af8b9ab60452ff15081c6f9f192
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Desenvolvimento com serviços de saída e Forms no AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Uso da API de serviço do Output e Forms no AEM Forms

Neste artigo, analisaremos o seguinte

* Serviço de saída - Normalmente, esse serviço é usado para unir dados xml ao modelo xdp ou pdf para gerar pdf nivelado. Para obter mais detalhes, consulte o[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para o serviço de saída.
* FormsService - é um serviço muito versátil que permite exportar/importar dados de e para arquivo PDF. Para obter mais detalhes, consulte o [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html) para o serviço Forms.


O trecho de código a seguir exporta dados do arquivo PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

A Linha 1 extrai o arquivo pdffile da solicitação

A Linha2 extrai o saveLocation da solicitação

A Linha 5 obtém o FormsService

A Linha 6 exporta o xmlData do arquivo PDF

**Para testar o pacote de amostra em seu sistema**

[Baixe e instale o pacote usando o gerenciador de pacotes de AEM](assets/outputandformsservice.zip)




**Depois de instalar o pacote, você terá que lista de permissões os seguintes URLs no Adobe Granite CSRF Filter.**

1. Siga os passos mencionados abaixo para lista de permissões os caminhos mencionados acima.
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procure por Filtro CSRF do Adobe Granite
1. Adicione os três caminhos a seguir nas seções excluídas e salve
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Pesquise por &quot;Filtro do referenciador do Sling&quot;
1. Marque a caixa de seleção &quot;Permitir vazio&quot;. (Essa configuração deve ser somente para fins de teste)
Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite fazer solicitações de POST para seu servidor. Instale o aplicativo Postman no seu sistema.
Inicie o aplicativo e insira o seguinte URL para testar a API de dados de exportação

Certifique-se de ter selecionado &quot;POST&quot; na lista suspensa
http://localhost:4502/content/AemFormsSamples/exportdata.html
Especifique &quot;Autorização&quot; como &quot;Autenticação básica&quot;. Especificar o nome de usuário e a senha do servidor AEM
Navegue até a guia &quot;Corpo&quot; e especifique os parâmetros da solicitação, conforme mostrado na imagem abaixo
![exportar](assets/postexport.png)
Em seguida, clique no botão Send

A embalagem contém 3 amostras. Os parágrafos a seguir explicam quando usar o serviço de saída ou o Forms Service, o url do serviço, os parâmetros de entrada que cada serviço espera

**Mesclar dados e Nivelar saída:**

* Use o Serviço de Saída para unir dados ao documento xdp ou pdf para gerar pdf nivelado
* **POST URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parâmetros da solicitação -**

   * xdp_or_pdf_file : O arquivo xdp ou pdf com o qual você deseja mesclar dados
   * xmlfile: O arquivo de dados xml que será mesclado com xdp_or_pdf_file
   * saveLocation: O local para salvar o documento renderizado em seu sistema de arquivos

**Importar dados para o arquivo PDF:**
* Usar o FormsService para importar dados para um arquivo PDF
* **URL do POST**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parâmetros da solicitação:**

   * pdffile : O arquivo pdf com o qual você deseja mesclar dados
   * xmlfile: O arquivo de dados xml que será unido ao arquivo pdf
   * saveLocation: O local para salvar o documento renderizado em seu sistema de arquivos. Por exemplo c:\\\outputsample.pdf.

**Exportar dados do arquivo PDF**
* Usar o FormsService para exportar dados do arquivo PDF
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parâmetros da solicitação:**

   * pdffile : O arquivo pdf do qual você deseja exportar dados.
   * saveLocation: O local para salvar os dados exportados em seu sistema de arquivos

[É possível importar essa coleção de cartazes para testar a API](assets/document-services-postman-collection.json)

