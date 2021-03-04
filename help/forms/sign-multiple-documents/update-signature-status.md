---
title: Atualizar o Status da Assinatura do Formulário no Banco de Dados
description: Atualize o status da assinatura do formulário assinado no banco de dados usando o fluxo de trabalho do AEM
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 4%

---


# Atualizar status da assinatura

O fluxo de trabalho UpdateSignatureStatus é acionado quando o usuário conclui a cerimônia de assinatura. Este é o fluxo do workflow

![fluxo de trabalho principal](assets/update-signature.PNG)

Atualizar Status da Assinatura é uma etapa de processo personalizada.
O principal motivo para implementar a etapa do processo personalizado é estender um fluxo de trabalho do AEM. Este é o código personalizado usado para atualizar o status da assinatura.
O código nesta etapa do processo personalizado faz referência ao serviço SignMultipleForms .


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Ativos

O fluxo de trabalho de status da assinatura de atualização pode ser [baixado aqui](assets/update-signature-status-workflow.zip)

