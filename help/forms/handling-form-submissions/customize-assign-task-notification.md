---
title: Personalizar a atribuição de notificação de tarefa
description: Incluir dados de formulário na atribuição de emails de notificação de tarefa
sub-product: formulários
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 2%

---


# Personalizar a atribuição de notificação de tarefa

O componente Atribuir tarefa é usado para atribuir tarefas a participantes do fluxo de trabalho. Quando uma tarefa é atribuída a um usuário ou grupo, uma notificação por email é enviada ao usuário ou aos membros do grupo definidos.
Normalmente, essa notificação por email conterá dados dinâmicos relacionados à tarefa. Esses dados dinâmicos são obtidos usando as [propriedades de metadados](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification) geradas pelo sistema.
Para incluir valores dos dados de formulário enviados na notificação por email, precisamos criar propriedade de metadados personalizada e usar essas propriedades de metadados personalizadas no modelo de email



## Criação da propriedade de metadados personalizada

A abordagem recomendada é criar um componente OSGI que implementa o método getUserMetadata do [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

O código a seguir cria quatro propriedades de metadados(_firstName_,_lastName_,_reason_ e _amountRequested_) e define seu valor a partir dos dados enviados. Por exemplo, o valor da propriedade de metadados _firstName_ é definido como o valor do elemento chamado firstName a partir dos dados enviados. O código a seguir parte do princípio de que os dados enviados do formulário adaptável estão no formato xml. Os formulários adaptáveis com base no esquema JSON ou no Modelo de dados de formulário geram dados no formato JSON.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## Usar as propriedades de metadados personalizados no modelo de email de notificação de tarefa

No modelo de email, você pode incluir a propriedade de metadados usando a seguinte sintaxe, onde amountRequested é a propriedade de metadados `${amountRequested}`

## Configurar Atribuir tarefa para usar a propriedade de metadados personalizada

Depois que o componente OSGi for criado e implantado no servidor AEM, configure o componente Atribuir tarefa como mostrado abaixo para usar propriedades de metadados personalizadas.


![Notificação de tarefa](assets/task-notification.PNG)

## Ativar o uso de propriedades de metadados personalizados

![Propriedades de metadados personalizados](assets/custom-meta-data-properties.PNG)

## Para experimentar isso no servidor

* [Configurar o Day CQ Mail Service](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Associar uma id de email válida a [usuário administrador](http://localhost:4502/security/users.html)
* Baixe e instale o [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Baixe o [Adaptive Form](assets/request-travel-authorization.zip) e importe para o AEM a partir do [interface do usuário de formulários e documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Implante e inicie o [Pacote Personalizado](assets/work-items-user-service-bundle.jar) usando o [console da Web](http://localhost:4502/system/console/bundles)
* [Visualizar e enviar o formulário](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

A notificação de atribuição de tarefa de envio de formulário é enviada para a ID de email associada ao usuário administrador. A captura de tela a seguir mostra um exemplo de notificação de atribuição de tarefa

![Notificação](assets/task-nitification-email.png)

>[!NOTE]
>O modelo de email para a notificação de tarefa de atribuição precisa estar no seguinte formato.
>
> assunto=Tarefa Atribuída - `${workitem_title}`
>
> message=String que representa seu modelo de email sem caracteres de nova linha.
