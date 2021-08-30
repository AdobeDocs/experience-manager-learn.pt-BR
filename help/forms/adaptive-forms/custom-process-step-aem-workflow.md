---
title: Etapa de processo personalizada
description: Gravação de anexos de formulário adaptável no sistema de arquivos usando a etapa do processo personalizado
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
source-git-commit: 2b7f0f6c34803672cc57425811db89146b38a70a
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 0%

---


# Etapa de processo personalizada

Este tutorial é destinado aos clientes da AEM Forms que precisam implementar a etapa de processo personalizada. Uma etapa do processo pode executar o script ECMA ou chamar o código java personalizado para executar operações. Este tutorial explicará as etapas necessárias para implementar o WorkflowProcess que é executado pela etapa do processo.

O principal motivo para implementar a etapa de processo personalizado é estender o fluxo de trabalho do AEM. Por exemplo, se você estiver usando componentes do AEM Forms no modelo de fluxo de trabalho, talvez queira executar as seguintes operações

* Salve os anexos do formulário adaptável no sistema de arquivos
* Manipular os dados enviados

Para realizar o caso de uso acima, você normalmente gravará um serviço OSGi que é executado pela etapa do processo.

## Criar projeto Maven

O primeiro passo é criar um projeto maven usando o Adobe Archetype apropriado. As etapas detalhadas são listadas neste [artigo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Depois que o projeto maven for importado para o eclipse, você estará pronto para começar a gravar seu primeiro componente OSGi que pode ser usado na etapa do processo.


### Criar classe que implemente o WorkflowProcess

Abra o projeto maven no eclipse IDE. Expanda a pasta **projectname** > **core**. Expanda a pasta src/main/java. Você deve ver um pacote que termine com &quot;núcleo&quot;. Crie a classe Java que implementa WorkflowProcess neste pacote. Você precisará substituir o método de execução. A assinatura do método execute é a seguinte
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)aciona WorkflowException
O método execute dá acesso às 3 variáveis a seguir

**WorkItem**: A variável workItem dará acesso aos dados relacionados ao workflow. A documentação da API pública está disponível [aqui.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Essa variável workflowSession fornecerá a capacidade de controlar o workflow. A documentação da API pública está disponível [aqui](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Todos os metadados associados ao workflow. Quaisquer argumentos de processo passados para a etapa do processo estão disponíveis usando o objeto MetaDataMap .[Documentação da API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

Neste tutorial, gravaremos os anexos adicionados ao Formulário adaptável no sistema de arquivos como parte do fluxo de trabalho do AEM.

Para realizar esse caso de uso, a seguinte classe java foi gravada

Vamos dar uma olhada neste código

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

Linha 1 - define as propriedades do seu componente. A propriedade process.label é o que você verá ao associar o componente OSGi à etapa do processo, conforme mostrado em uma das capturas de tela abaixo.

Linhas 13-15 - Os argumentos do processo transmitidos para este componente OSGi são divididos usando o separador &quot;,&quot;. Os valores para attachmentPath e saveToLocation são então extraídos da matriz de sequências de caracteres.

* attachmentPath - Esse é o mesmo local especificado no formulário adaptável quando você configura a ação de envio do formulário adaptável para chamar AEM fluxo de trabalho. Este é um nome da pasta na qual você deseja que os anexos sejam salvos em AEM em relação à carga do fluxo de trabalho.

* saveToLocation - esse é o local em que você deseja que os anexos sejam salvos no sistema de arquivos do servidor AEM.

Esses dois valores são passados como argumentos de processo, conforme mostrado na captura de tela abaixo.

![ProcessStep](assets/implement-process-step.gif)

O serviço QueryBuilder é usado para consultar nós do tipo nt:file na pasta attachmentPath. O restante do código é repetido por meio dos resultados da pesquisa para criar um objeto de Documento e salvá-lo no sistema de arquivos


>[!NOTE]
>
>Como estamos usando um objeto de Documento específico para o AEM Forms, é necessário incluir a dependência aemfd-client-sdk em seu projeto maven. A ID do grupo é com.adobe.aemfd e a ID do artefato é aemfd-client-sdk.

#### Criar e implantar

[Crie o pacote conforme descrito ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[aqui. Verifique se ele foi implantado e está no estado ativo](http://localhost:4502/system/console/bundles)

Crie um modelo de fluxo de trabalho. Arraste e solte a etapa do processo no modelo de fluxo de trabalho. Associe a etapa do processo a &quot;Salvar anexos do formulário adaptável no sistema de arquivos&quot;.

Forneça os argumentos do processo necessários separados por vírgula. Por exemplo, Anexos, c:\\scrappp\\. O primeiro argumento é a pasta em que os anexos do Formulário adaptativo serão armazenados em relação à carga do fluxo de trabalho. Esse deve ser o mesmo valor especificado ao configurar a ação de envio do Formulário adaptável. O segundo argumento é o local em que você deseja que os anexos sejam armazenados.

Crie um formulário adaptável. Arraste e solte o componente Anexos de arquivo no formulário. Configure a ação Enviar do formulário para invocar o fluxo de trabalho criado nas etapas anteriores. Forneça o caminho de anexo apropriado.

Salve as configurações.

Visualizar o formulário. Adicione alguns anexos e envie o formulário. Os anexos devem ser salvos no sistema de arquivos no local especificado por você no fluxo de trabalho.

