---
title: Gerando Documento de Comunicações Interativas para canal de impressão usando o mecanismo de pasta monitorada
seo-title: Gerando Documento de Comunicações Interativas para canal de impressão usando o mecanismo de pasta monitorada
description: Usar pasta assistida para gerar documentos do canal de impressão
seo-description: Usar pasta assistida para gerar documentos do canal de impressão
feature: Comunicação interativa
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 1%

---


# Gerando Documento de Comunicações Interativas para canal de impressão usando o mecanismo de pasta monitorada

Depois de projetar e testar seu documento de canal de impressão, geralmente é necessário gerar o documento fazendo uma chamada REST ou gerando documentos de impressão usando o mecanismo de pasta de observação.

Este artigo explica o caso de uso da geração de documentos do canal de impressão usando o mecanismo de pasta monitorada.

Quando você solta um arquivo na pasta assistida, um script associado à pasta assistida é executado. Esse script é explicado no artigo abaixo.

O arquivo colocado na pasta assistida tem a seguinte estrutura. O código gerará instruções para todos os números de conta listados no documento XML.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

A listagem de código abaixo faz o seguinte:

Linha 1 - Caminho para o InterativeCommunicationsDocument

Linhas 15-20: Obter a lista de números de conta do documento XML solto na pasta assistida

Linhas 24-25: Obtenha o PrintChannelService e o Canal de impressão associados ao documento.

Linha 30: Passe o número da conta como o elemento principal para o Modelo de dados de formulário.

Linhas 32-36: Defina as Opções de Dados para o Documento que deve ser gerado.

Linha 38: Renderize o documento.

Linhas 39-40 - Salva o documento gerado no sistema de arquivos.

O endpoint REST do Modelo de dados de formulário espera uma id como um parâmetro de entrada. essa id é mapeada para um Atributo de solicitação chamado account number, como mostrado na captura de tela abaixo.

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


**Para testar isso em seu sistema local, siga as seguintes instruções:**

* Configure o Tomcat conforme descrito neste artigo [.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) O Tomcat tem o arquivo war que gera os dados de amostra.
* Configure o serviço também conhecido como usuário do sistema, conforme descrito neste [artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Verifique se o usuário do sistema tem permissões de leitura no nó a seguir. Para conceder as permissões login para [user admin](https://localhost:4502/useradmin) e pesquisar os &quot;dados&quot; do usuário do sistema e fornecer as permissões de leitura no nó a seguir, tabulação para a guia de permissões
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importe os seguintes pacotes no AEM usando o gerenciador de pacotes. Este pacote contém o seguinte:


* [Exemplo de documento de comunicações interativas](assets/retirementstatementprint.zip)
* [Script de pasta assistida](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configuração da fonte de dados](assets/datasource.zip)

* Abra o arquivo /etc/fd/watchfolder/scripts/PrintPDF.ecma . Certifique-se de que o caminho para o interativeCommunicationsDocument na linha 1 está apontando para o documento correto que você deseja imprimir

* Modifique o saveLocation de acordo com sua preferência na Linha 2

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


* Solte o arquivo accountnumbers.xml no diretório C:\RenderPrintChannel\input folder.

* Os arquivos PDF gerados são gravados no saveLocation, conforme especificado no script de ecma.

>[!NOTE]
>
>Se você planeja usar isso em um sistema operacional que não seja Windows, navegue até
>
>/etc/fd/watchfolder /config/PrintChannelDocument e altere folderPath de acordo com sua preferência

