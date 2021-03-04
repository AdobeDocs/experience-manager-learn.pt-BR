---
title: Etapa de processo personalizada
seo-title: Etapa de processo personalizada
description: Gravação de anexos de formulário adaptável no sistema de arquivos usando a etapa do processo personalizado
seo-description: Gravação de anexos de formulário adaptável no sistema de arquivos usando a etapa do processo personalizado
feature: Fluxo de trabalho
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 0%

---


# Etapa de processo personalizada

Este tutorial é destinado aos clientes do AEM Forms que precisam implementar a etapa de processo personalizada. Uma etapa do processo pode executar o script ECMA ou chamar o código java personalizado para executar operações. Este tutorial explicará as etapas necessárias para implementar o WorkflowProcess que é executado pela etapa do processo.

O principal motivo para implementar a etapa do processo personalizado é estender o fluxo de trabalho do AEM. Por exemplo, se você estiver usando componentes do AEM Forms no seu modelo de fluxo de trabalho, talvez queira executar as seguintes operações

* Salve os anexos do formulário adaptável no sistema de arquivos
* Manipular os dados enviados

Para realizar o caso de uso acima, você normalmente gravará um serviço OSGi que é executado pela etapa do processo.

## Criar projeto Maven

A primeira etapa é criar um projeto maven usando o Arquétipo do Adobe Maven apropriado. As etapas detalhadas são listadas neste [artigo](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Depois que o projeto maven for importado para o eclipse, você estará pronto para começar a gravar seu primeiro componente OSGi que pode ser usado na etapa do processo.


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

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
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
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

Linha 1 - define as propriedades do seu componente. A propriedade process.label é o que você verá ao associar o componente OSGi à etapa do processo, conforme mostrado em uma das capturas de tela abaixo.

Linhas 13-15 - Os argumentos do processo transmitidos para este componente OSGi são divididos usando o separador &quot;,&quot;. Os valores para attachmentPath e saveToLocation são então extraídos da matriz de sequências de caracteres.

* attachmentPath - Esse é o mesmo local especificado no formulário adaptável quando você configura a ação de envio do formulário adaptável para chamar o fluxo de trabalho do AEM. Este é um nome da pasta que você deseja que os anexos sejam salvos no AEM em relação à carga do fluxo de trabalho.

* saveToLocation - esse é o local em que você deseja que os anexos sejam salvos no sistema de arquivos do servidor AEM.

Esses dois valores são passados como argumentos de processo, conforme mostrado na captura de tela abaixo.

![ProcessStep](assets/implement-process-step.gif)


Linha 19: em seguida, construímos o attachmentFilePath. O caminho do arquivo anexo é como

    /var/fd/dashboard/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/attachments

* Os &quot;Anexos&quot; são o nome da pasta relativo à carga útil do fluxo de trabalho que foi especificada quando você configurou a opção de envio do Formulário Adaptativo.

   ![submitoptions](assets/af-submit-options.gif)

Linhas 24-26 - Obtenha ResourceResolver e, em seguida, o recurso apontando para attachmentFilePath.

O restante do código cria objetos Document ao iterar pelo objeto filho do recurso apontando para attachmentFilePath usando a API. Esse objeto de documento é específico para o AEM Forms. Em seguida, usamos o método copyToFile do objeto do documento para salvar o objeto do documento.

>[!NOTE]
>
>Como estamos usando o objeto de Documento específico do AEM Forms, é necessário incluir a dependência aemfd-client-sdk em seu projeto maven. A ID do grupo é com.adobe.aemfd e a ID do artefato é aemfd-client-sdk.

#### Criar e implantar

[Crie o pacote conforme descrito ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[aqui. Verifique se ele foi implantado e está no estado ativo](http://localhost:4502/system/console/bundles)

Crie um modelo de fluxo de trabalho. Arraste e solte a etapa do processo no modelo de fluxo de trabalho. Associe a etapa do processo a &quot;Salvar anexos do formulário adaptável no sistema de arquivos&quot;.

Forneça os argumentos do processo necessários separados por vírgula. Por exemplo, Anexos, c:\\scrappp\\. O primeiro argumento é a pasta em que os anexos do Formulário adaptativo serão armazenados em relação à carga do fluxo de trabalho. Esse deve ser o mesmo valor especificado ao configurar a ação de envio do Formulário adaptável. O segundo argumento é o local em que você deseja que os anexos sejam armazenados.

Crie um formulário adaptável. Arraste e solte o componente Anexos de arquivo no formulário. Configure a ação Enviar do formulário para invocar o fluxo de trabalho criado nas etapas anteriores. Forneça o caminho de anexo apropriado.

Salve as configurações.

Visualizar o formulário. Adicione alguns anexos e envie o formulário. Os anexos devem ser salvos no sistema de arquivos no local especificado por você no fluxo de trabalho.

