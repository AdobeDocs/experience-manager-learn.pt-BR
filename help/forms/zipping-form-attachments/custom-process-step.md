---
title: Etapa de processo personalizada para compactar anexos de arquivo
description: Etapa de processo personalizada para adicionar os anexos de formulário adaptáveis a um arquivo zip e armazenar o arquivo zip em uma variável de fluxo de trabalho
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: 1131dca8-882d-4904-8691-95468fb708b7
duration: 75
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---


# Etapa de processo personalizada

Uma etapa de processo personalizada foi implementada para criar o arquivo zip que contém os anexos de formulário. Se você não estiver familiarizado com a criação de um pacote OSGi, [siga estas instruções](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en).

O código na etapa de processo personalizada faz o seguinte:

- Consulte todos os anexos de formulário adaptável na pasta de carga útil. O nome da pasta é passado como um argumento de processo para a etapa do processo.
- Crie um arquivo zip contendo os anexos de formulário e armazene-o na pasta de carga útil.
- Defina o valor da variável de workflow (no_of_attachments).

```java
package com.aemforms.formattachments.core;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;

import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=Zip form attachments",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Zip form attachments"
})

public class ZipFormAttachments implements WorkflowProcess {

     private static final Logger log = LoggerFactory.getLogger(ZipFormAttachments.class);
     @Reference
     QueryBuilder queryBuilder;

     @Override
     public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException {
             String payloadPath = workItem.getWorkflowData().getPayload().toString();
             log.debug("The payload path is " + payloadPath);
             MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
             Session session = workflowSession.adaptTo(Session.class);
             Map<String, String> map = new HashMap<String, String>();
             map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
             map.put("type", "nt:file");
             Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
             query.setStart(0);
             query.setHitsPerPage(20);
             SearchResult result = query.getResult();
             log.debug("Get result hits " + result.getHits().size());
             ByteArrayOutputStream baos = new ByteArrayOutputStream();
             ZipOutputStream zipOut = new ZipOutputStream(baos);
             int no_of_attachments = result.getHits().size();
             for (Hit hit: result.getHits()) {
                     try {
                             String attachmentPath = hit.getPath();
                             log.debug("The hit path is " + hit.getPath());
                             Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                             InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                             ByteArrayOutputStream buffer = new ByteArrayOutputStream();
                             int nRead;
                             byte[] data = new byte[1024];
                             while ((nRead = attachmentStream.read(data, 0, data.length)) != -1) {
                                     buffer.write(data, 0, nRead);
                             }

                             buffer.flush();
                             byte[] byteArray = buffer.toByteArray();
                             ZipEntry zipEntry = new ZipEntry(hit.getTitle());
                             zipOut.putNextEntry(zipEntry);
                             zipOut.write(byteArray);
                             zipOut.closeEntry();

                     } catch (Exception e) {
                             log.debug("The error message is " + e.getMessage());
                     }
             }
             try {
                    zipOut.close();
                    Node payloadNode = session.getNode(payloadPath);
                    Node zippedFileNode =  payloadNode.addNode("zipped_attachments.zip", "nt:file");
                    javax.jcr.Node resNode = zippedFileNode.addNode("jcr:content", "nt:resource");

                    ValueFactory valueFactory = session.getValueFactory();
                    Document zippedDocument = new Document(baos.toByteArray());

                    Binary contentValue = valueFactory.createBinary(zippedDocument.getInputStream());
                    metaDataMap.put("no_of_attachments", no_of_attachments);

                    workflowSession.updateWorkflowData(workItem.getWorkflow(), workItem.getWorkflow().getWorkflowData());
                    log.debug("Updated workflow");
                    resNode.setProperty("jcr:data", contentValue);
                    session.save();
                    zippedDocument.close();

             } catch (IOException | RepositoryException e) {
                     log.error("Error in closing zipout", e);
             }
     }
}
```

>[!NOTE]
>
> Certifique-se de que você tenha uma variável chamada _no_of_attachments_ do tipo Double em seu fluxo de trabalho para que esse código funcione.

## Próximas etapas

[Preencher variáveis de fluxo de trabalho ArrayList com anexos e nome do anexo](./custom-process-step.md)


