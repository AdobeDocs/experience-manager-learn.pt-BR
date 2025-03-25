---
title: Gerar documento de comunicações interativas para canal de impressão usando o mecanismo de pasta de observação
description: Usar pasta monitorada para gerar documentos de canal de impressão
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Gerar documento de comunicações interativas para canal de impressão usando o mecanismo de pasta de observação

Depois de projetar e testar o documento de canal de impressão, geralmente será necessário gerar o documento fazendo uma chamada REST ou gerando documentos de impressão usando o mecanismo de pasta de observação.

Este artigo explica o caso de uso de geração de documentos de canal de impressão usando o mecanismo de pasta monitorada.

Quando você solta um arquivo na pasta monitorada, um script associado à pasta monitorada é executado. Esse script é explicado no artigo abaixo.

O arquivo colocado na pasta monitorada tem a seguinte estrutura. O código gerará demonstrativos para todos os números de conta listados no documento XML.

&lt;números de conta>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

A listagem de código abaixo faz o seguinte:

Linha 1 - Caminho para o InterativeCommunicationsDocument

Linhas 15-20: obtenha a lista de números de contas do documento XML lançado na pasta monitorada

Linhas 24 -25: Obtenha o PrintChannelService e o Print Channel associados ao documento.

Linha 30: transmita o número da conta como o elemento principal para o Modelo de dados do formulário.

Linhas 32-36: Defina as Opções de Dados para o Documento a ser gerado.

Linha 38: renderiza o documento.

Linhas 39-40 - Salva o documento gerado no sistema de arquivos.

O ponto de extremidade REST do Modelo de dados de formulário espera uma ID como parâmetro de entrada. essa id é mapeada para um atributo de solicitação chamado account number, como mostrado na captura de tela abaixo.

![requestattribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**Para testar isto no seu sistema local, siga as seguintes instruções:**

* Configure o Tomcat conforme descrito neste [artigo.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat tem o arquivo war que gera os dados de amostra.
* Configure o serviço também conhecido como usuário do sistema conforme descrito neste [artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Verifique se esse usuário do sistema tem permissões de leitura no nó a seguir. Para conceder as permissões de logon a [user admin](https://localhost:4502/useradmin) e pesquisar os &quot;dados&quot; do usuário do sistema e conceder as permissões de leitura no seguinte nó, tabulação na guia de permissões
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importe o(s) seguinte(s) pacote(s) para o AEM usando o gerenciador de pacotes. Este pacote contém o seguinte:


* [Exemplo de documento de comunicações interativas](assets/retirementstatementprint.zip)
* [Script de pasta monitorada](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configuração da fonte de dados](assets/datasource.zip)

* Abra o arquivo /etc/fd/watchfolder/scripts/PrintPDF.ecma. Verifique se o caminho para o interativeCommunicationsDocument na linha 1 aponta para o documento correto que você deseja imprimir

* Modifique saveLocation de acordo com sua preferência na Linha 2

* Crie o arquivo accountnumbers.xml com o seguinte conteúdo

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* Solte o arquivo accountnumbers.xml na pasta C:\RenderPrintChannel\input.

* Os arquivos PDF gerados são gravados no saveLocation conforme especificado no script ecma.

>[!NOTE]
>
>Se você planeja usá-lo em um sistema operacional que não seja Windows, navegue até
>
>/etc/fd/watchfolder /config/PrintChannelDocument e altere o folderPath de acordo com sua preferência
