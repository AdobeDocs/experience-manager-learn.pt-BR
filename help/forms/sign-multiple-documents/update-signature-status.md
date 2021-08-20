---
title: Atualizar o Status da Assinatura do Formulário no Banco de Dados
description: Atualize o status da assinatura do formulário assinado no banco de dados usando o fluxo de trabalho AEM
feature: Formulários adaptáveis
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 3%

---


# Atualizar status da assinatura

O fluxo de trabalho UpdateSignatureStatus é acionado quando o usuário conclui a cerimônia de assinatura. Este é o fluxo do workflow

![fluxo de trabalho principal](assets/update-signature.PNG)

Atualizar Status da Assinatura é uma etapa de processo personalizada.
O principal motivo para implementar a etapa de processo personalizado é estender um fluxo de trabalho AEM. Este é o código personalizado usado para atualizar o status da assinatura.
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

