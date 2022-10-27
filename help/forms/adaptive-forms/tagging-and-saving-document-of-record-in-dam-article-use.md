---
title: Marcação e armazenamento do AEM Forms DoR no DAM
description: Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pela AEM Forms AEM DAM. A marcação do documento é feita com base nos dados de formulário enviados.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 0%

---

# Marcação e armazenamento do AEM Forms DoR no DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pela AEM Forms AEM DAM. A marcação do documento é feita com base nos dados de formulário enviados.

Uma tarefa comum dos clientes é armazenar e marcar o Documento de registro (DoR) gerado pela AEM Forms no DAM AEM. A marcação do documento precisa ser baseada nos dados enviados pela Adaptive Forms. Por exemplo, se o status de emprego nos dados enviados for &quot;Descontinuado&quot;, queremos adicionar uma tag ao documento com a tag &quot;Descontinuado&quot; e armazená-lo no DAM.

O caso de uso é o seguinte:

* Um usuário preenche o Formulário adaptável. Na forma adaptativa, o estado civil do usuário (ex-solteiro) e o status de emprego (ex-descontinuado) são capturados.
* No envio do formulário, um Fluxo de trabalho AEM é acionado. Esse workflow marca o documento com o status civil (Único) e o status de emprego (Descontinuado) e armazena o documento no DAM.
* Depois que o documento é armazenado no DAM, o administrador deve ser capaz de pesquisar o documento por essas tags. Por exemplo, a pesquisa em Único ou Aposentado buscaria as DoRs apropriadas.

Para atender a esse caso de uso, uma etapa do processo personalizado foi gravada. Nesta etapa, buscamos os valores dos elementos de dados apropriados a partir dos dados enviados. Em seguida, construímos o bloco da tag usando esse valor. Por exemplo, se o valor do elemento de status civil for &quot;Único&quot;, o título da tag se tornará **Pico:EmpregoStatus/Único. **Usando a API do TagManager , encontramos a tag e aplicamos a tag ao DoR.

Veja a seguir o código completo para marcar e armazenar o Documento de registro AEM DAM.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

Para que esse exemplo funcione em seu sistema, siga as etapas listadas abaixo:
* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e implante o pacote setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Esse é o pacote OSGI personalizado que define as tags a partir dos dados de formulário enviados.

* [Download do formulário adaptável de amostra](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Ir para Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Clique em Criar | Upload de arquivo e upload do tag-and-store-in-dam-adaptive-form.zip

* [Importe os ativos do artigo](assets/tag-and-store-in-dam-assets.zip) usando o gerenciador de pacotes AEM
* Abra o [formulário de amostra no modo de visualização](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **Preencha todos os campos** e enviar o formulário.
* [Navegar para a pasta Pico no DAM](http://localhost:4502/assets.html/content/dam/Peak). Você deve ver DoR na pasta Pico. Verifique as propriedades do documento. Ele deve ser marcado adequadamente.
Parabéns!! A amostra foi instalada com êxito no sistema

* Vamos explorar o [workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) que é acionado no envio do formulário.
* A primeira etapa do fluxo de trabalho cria um nome de arquivo exclusivo concatenando o nome do candidato e o país de residência.
* A segunda etapa do fluxo de trabalho passa pela hierarquia de tags e pelos elementos dos campos de formulário que precisam ser marcados. A etapa do processo extrai o valor dos dados enviados e constrói o título da tag que precisa marcar o documento.
* Se quiser armazenar DoR em uma pasta diferente no DAM, especifique o local da pasta usando as propriedades de configuração, conforme especificado na captura de tela abaixo.

Os outros dois parâmetros são específicos do DoR e do Caminho do arquivo de dados, conforme especificado nas opções de envio do formulário adaptável. Certifique-se de que os valores especificados aqui correspondem aos valores especificados nas opções de envio do formulário adaptável.

![Dor de tag](assets/tag_dor_service_configuration.gif)
