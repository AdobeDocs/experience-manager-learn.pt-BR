---
title: Montagem de anexos de formulário
description: Montagem de anexos de formulário na ordem especificada
feature: Assembler
version: 6.4,6.5
jira: KT-6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
duration: 175
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 0%

---

# Montagem de anexos de formulário

Este artigo fornece ativos para montar anexos de formulário adaptáveis em uma ordem especificada. Os anexos de formulário precisam estar em formato pdf para que esse código de exemplo funcione. Veja a seguir o caso de uso.
O usuário que preenche um formulário adaptável anexa um ou mais documentos pdf ao formulário.
No envio do formulário, monte os anexos do formulário para gerar um pdf. Você pode especificar a ordem na qual os anexos são montados para gerar o pdf final.

## Criar componente OSGi que implementa a interface WorkflowProcess

Crie um componente OSGi que implemente a [interface com.adobe.granite.workflow.exec.WorkflowProcess](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). O código neste componente pode ser associado ao componente da etapa do processo no fluxo de trabalho do AEM. O método execute da interface com.adobe.granite.workflow.exec.WorkflowProcess é implementado neste componente.

Quando um formulário adaptável é enviado para acionar um fluxo de trabalho do AEM, os dados enviados são armazenados no arquivo especificado na pasta de carga. Por exemplo, este é o arquivo de dados enviado. Precisamos montar os anexos especificados na etiqueta de cartão e extrato bancário.
![dados enviados](assets/submitted-data.JPG).

### Obter os nomes das tags

A ordem dos anexos é especificada como argumentos de etapa do processo no fluxo de trabalho, conforme mostrado na captura de tela abaixo. Aqui, estamos montando os anexos adicionados ao campo descartado, seguidos pelos demonstrativos bancários

![etapa do processo](assets/process-step.JPG)

O trecho de código a seguir extrai os nomes dos anexos dos argumentos do processo

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Criar DDX a partir dos nomes dos anexos

Depois, é necessário criar [Descrição do documento XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) documento que é usado pelo serviço Assembler para montar documentos. Veja a seguir o DDX que foi criado a partir dos argumentos do processo. O elemento NoForms permite nivelar documentos XFA antes de serem montados. Observe que os elementos de origem de PDF estão na ordem correta, conforme especificado nos argumentos do processo.

![ddx-xml](assets/ddx.PNG)

### Criar mapa de documentos

Em seguida, criamos um mapa de documentos com o nome do anexo como a chave e o anexo como o valor. O serviço Query Builder foi usado para consultar os anexos no caminho de carga e criar o mapa de documentos. Este mapa de documento junto com o DDX é necessário para o serviço de montagem montar o pdf final.

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### Usar AssemblerService para reunir os documentos

Depois que o DDX e o mapa do documento forem criados, a próxima etapa será usar o AssemblerService para montar os documentos.
O código a seguir monta e retorna o pdf montado.

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### Salvar o PDF montado na pasta de carga útil

A etapa final é salvar o pdf montado na pasta de carga útil. Esse pdf pode ser acessado nas etapas subsequentes do fluxo de trabalho para processamento adicional.
O trecho de código a seguir foi usado para salvar o arquivo na pasta de carga útil

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

Veja a seguir a estrutura da pasta de carga útil após os anexos de formulário serem montados e armazenados.

![estrutura de carga útil](assets/payload-structure.JPG)

### Para que esse recurso funcione no servidor AEM

* Baixe o [Montar formulário de anexos de formulário](assets/assemble-form-attachments-af.zip) ao seu sistema local.
* Importe o formulário do[Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) página.
* Baixar [fluxo de trabalho](assets/assemble-form-attachments.zip) e importe para AEM usando o gerenciador de pacotes.
* Baixe o [pacote personalizado](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* Implante e inicie o pacote usando o [console da web](http://localhost:4502/system/console/bundles)
* Aponte seu navegador para [Formulário MontarAnexos](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Adicionar um anexo no documento de ID e alguns documentos em pdf à seção de extratos bancários
* Enviar o formulário para acionar o fluxo de trabalho
* Verifique o fluxo de trabalho [pasta de carga no crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) para o pdf montado

>[!NOTE]
> Se você ativou o agente de log para o pacote personalizado, o DDX e o arquivo montado serão gravados na pasta da instalação do AEM.
