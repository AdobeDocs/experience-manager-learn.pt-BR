---
title: Implementando Etapa de Processo Personalizada
description: Gravação de anexos do formulário adaptável no sistema de arquivos usando uma etapa de processo personalizada
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Etapa de processo personalizada

Este tutorial destina-se aos clientes do AEM Forms que precisam implementar uma etapa de processo personalizada. Uma etapa do processo pode executar um script ECMA ou chamar código Java™ personalizado para executar operações. Este tutorial explicará as etapas necessárias para implementar o WorkflowProcess que é executado pela etapa do processo.

O principal motivo para implementar uma etapa de processo personalizada é estender o fluxo de trabalho do AEM. Por exemplo, se estiver usando componentes do AEM Forms no modelo de fluxo de trabalho, talvez você queira executar as seguintes operações

* Salvar os anexos do formulário adaptável no sistema de arquivos
* Manipular os dados enviados

Para realizar o caso de uso acima, você normalmente gravará um serviço OSGi que é executado pela etapa do processo.

## Criar projeto Maven

A primeira etapa é criar um projeto maven usando o Arquétipo Maven Adobe apropriado. As etapas detalhadas estão listadas neste [artigo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=pt-BR). Depois de importar o projeto Maven para o Eclipse, você estará pronto para começar a escrever seu primeiro componente OSGi que pode ser usado na etapa do processo.


### Criar classe que implementa WorkflowProcess

Abra o projeto Maven no Eclipse IDE. Expanda a pasta **nomedoprojeto** > **núcleo**. Expanda a pasta `src/main/java`. Você deve ver um pacote que termina com `core`. Crie uma classe Java™ que implementa WorkflowProcess neste pacote. Será necessário substituir o método de execução. A assinatura do método execute é a seguinte:

```java
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException 
```

O método execute dá acesso às 3 variáveis a seguir:

**WorkItem**: a variável item de trabalho dará acesso aos dados relacionados ao fluxo de trabalho. A documentação da API pública está disponível [aqui.](https://helpx.adobe.com/br/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: essa variável workflowSession lhe dará a capacidade de controlar o fluxo de trabalho. A documentação da API pública está disponível [aqui](https://helpx.adobe.com/br/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html).

**MetaDataMap**: todos os metadados associados ao fluxo de trabalho. Todos os argumentos de processo passados para a etapa do processo estão disponíveis usando o objeto MetaDataMap.[Documentação da API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

Neste tutorial, vamos gravar os anexos adicionados ao Formulário adaptável no sistema de arquivos como parte do fluxo de trabalho do AEM.

Para realizar esse caso de uso, a seguinte classe Java™ foi escrita

Vamos analisar esse código

```java
package com.learningaemforms.adobe.core;

import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
        map.put("path", payloadPath + "/" + attachmentsPath);
        File saveLocationFolder = new File(saveToLocation);
        if (!saveLocationFolder.exists()) {
            saveLocationFolder.mkdirs();
        }

        map.put("type", "nt:file");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
        query.setStart(0);
        query.setHitsPerPage(20);

        SearchResult result = query.getResult();
        log.debug("Got  " + result.getHits().size() + " attachments ");
        Node attachmentNode = null;
        for (Hit hit: result.getHits()) {
            try {
                String path = hit.getPath();
                log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
                attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
                InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                Document attachmentDoc = new Document(documentStream);
                attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
                attachmentDoc.close();
            } catch (Exception e) {
                log.debug("Error saving file " + e.getMessage());
            }
```

Linha 1 - define as propriedades do componente. A propriedade `process.label` é o que você verá ao associar o componente OSGi à etapa do processo, conforme mostrado em uma das capturas de tela abaixo.

Linhas 13-15 - Os argumentos do processo transmitidos para esse componente OSGi são divididos usando o separador &quot;,&quot;. Os valores de attachmentPath e saveToLocation são extraídos da matriz de cadeias de caracteres.

* attachmentPath — é o mesmo local especificado no Formulário adaptável quando você configura a ação de envio do Formulário adaptável para chamar o Fluxo de trabalho do AEM. Esse é um nome da pasta na qual você deseja que os anexos sejam salvos no AEM com relação à carga útil do fluxo de trabalho.

* saveToLocation — este é o local em que você deseja que os anexos sejam salvos no sistema de arquivos do servidor AEM.

Esses dois valores são passados como argumentos de processo, conforme mostrado na captura de tela abaixo.

![EtapaProcesso](assets/implement-process-step.gif)

O serviço QueryBuilder é usado para consultar nós do tipo `nt:file` na pasta attachmentsPath. O restante do código repete os resultados da pesquisa para criar o objeto Documento e salvá-lo no sistema de arquivos.


>[!NOTE]
>
>Como estamos usando um objeto Documento específico do AEM Forms, é necessário incluir a dependência aemfd-client-sdk no projeto maven. A ID do grupo é `com.adobe.aemfd` e a ID dos artefatos é `aemfd-client-sdk`.

#### Criar e implantar

[Crie o pacote conforme descrito aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=pt-BR)
[Verifique se o pacote está implantado e no estado ativo](http://localhost:4502/system/console/bundles)

Crie um modelo de fluxo de trabalho. Arraste e solte a etapa do processo no modelo de fluxo de trabalho. Associe a etapa do processo a &quot;Salvar anexos do formulário adaptável no sistema de arquivos&quot;.

Forneça os argumentos de processo necessários separados por vírgula. Por exemplo, Attachments,c:\\scrappp\\. O primeiro argumento é a pasta quando os anexos do formulário adaptável serão armazenados em relação à carga do fluxo de trabalho. Esse deve ser o mesmo valor especificado ao configurar a ação de envio do Formulário adaptável. O segundo argumento é o local em que você deseja que os anexos sejam armazenados.

Crie um formulário adaptável. Arraste e solte o componente Anexos de arquivo no formulário. Configure a ação de envio do formulário para invocar o fluxo de trabalho criado nas etapas anteriores. Forneça o caminho de anexo apropriado.

Salve as configurações.

Visualize o formulário. Adicione alguns anexos e envie o formulário. Os anexos devem ser salvos no sistema de arquivos no local especificado por você no workflow.
