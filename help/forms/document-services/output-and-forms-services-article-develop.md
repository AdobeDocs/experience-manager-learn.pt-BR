---
title: Desenvolvimento com serviços de saída e Forms no AEM Forms
description: Saiba mais sobre como desenvolver com a API de serviço de saída e Forms no AEM Forms.
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---

# Desenvolvimento com serviços de saída e Forms no AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Saiba mais sobre como desenvolver com a API de serviço de saída e Forms no AEM Forms.

Neste artigo, analisaremos o seguinte

* [Serviço de saída](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) - Normalmente, esse serviço é usado para mesclar dados xml com modelo xdp ou pdf para gerar pdf nivelado.
* [FormsService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) - Este é um serviço muito versátil que permite renderizar o xdp como pdf e exportar/importar dados de e para um arquivo do PDF.


O trecho de código a seguir exporta dados do arquivo do PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

A Linha 1 extrai o arquivo PDF da solicitação

A Linha 2 extrai o saveLocation da solicitação

A linha 5 obtém o FormsService

A linha 6 exporta os xmlData do arquivo PDF

**Para testar o pacote de exemplo em seu sistema**

[Baixe e instale o pacote usando o gerenciador de pacotes da AEM](assets/using-output-and-form-service-api.zip)




incluir na lista de permissões **Depois de instalar o pacote, você terá que copiar as seguintes URLs no Filtro CSRF do Adobe Granite.**

1. Siga as etapas mencionadas abaixo para incluir na lista de permissões os caminhos mencionados acima.
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar filtro CSRF do Adobe Granite
1. Adicione os 3 caminhos a seguir nas seções excluídas e salve
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. Procure por &quot;Sling Referrer Filter&quot;
1. Marque a caixa de seleção &quot;Permitir vazio&quot;. (Essa configuração deve ser somente para fins de teste)

## Teste das amostras

Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite fazer solicitações POST no servidor.

* Instale o aplicativo Postman no sistema.
* Inicie o aplicativo e insira o URL apropriado
* Verifique se você selecionou &quot;POST&quot; na lista suspensa
* Certifique-se de especificar &quot;Autorização&quot; como &quot;Autenticação básica&quot;. Especifique o nome de usuário e a senha do AEM Server
* Especifique os parâmetros da solicitação na guia do corpo
* Clique no botão Enviar

O pacote contém 4 amostras. Os parágrafos a seguir explicam quando usar o serviço de saída ou o Forms Service, o url do serviço, os parâmetros de entrada que cada serviço espera

## Utilização do OutputService para mesclar dados com o modelo xdp

* Usar o Serviço de saída para mesclar dados com um documento xdp ou pdf para gerar um PDF nivelado
* **POSTAR URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Solicitar Parâmetros -**

   * **xdp_or_pdf_file** : o arquivo xdp ou pdf com o qual você deseja mesclar dados
   * **xmlfile**: o arquivo de dados xml que é mesclado com xdp_or_pdf_file
   * **saveLocation**: o local onde salvar o documento renderizado no sistema de arquivos. Por exemplo c:\\documents\\sample.pdf

### Uso da API FormsService

#### Importar dados

* Usar FormsService importData para importar dados para o arquivo do PDF
* **POSTAR URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html

* **Solicitar Parâmetros:**

   * **pdffile** : o arquivo pdf com o qual você deseja mesclar dados
   * **xmlfile**: o arquivo de dados xml que é mesclado com o arquivo pdf
   * **saveLocation**: o local onde salvar o documento renderizado no sistema de arquivos. Por exemplo, `c:\\outputsample.pdf`.

#### Exportar dados

* Usar a API FormsService exportData para exportar dados do arquivo do PDF
* **POSTAR URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Solicitar Parâmetros:**

   * **pdffile** : o arquivo pdf do qual você deseja exportar dados
   * **saveLocation**: o local para salvar os dados exportados no sistema de arquivos. Por exemplo c:\\documentos\\dados_exportados.xml

#### Renderizar XDP

* Renderizar modelo XDP como pdf estático/dinâmico
* Use a API renderPDFForm do FormsService para renderizar o modelo xdp como PDF
* **POSTAR URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* Parâmetro de solicitação:
   * xdpName: Nome do arquivo xdp a ser renderizado como pdf

[Você poderia importar essa coleção do carteiro para testar a API](assets/UsingDocumentServicesInAEMForms.postman_collection.json)

