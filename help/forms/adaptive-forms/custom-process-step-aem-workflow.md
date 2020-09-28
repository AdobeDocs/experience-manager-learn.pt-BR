---
title: Implementação da Etapa do Processo Personalizado
seo-title: Implementação da Etapa do Processo Personalizado
description: Gravação de anexos de formulário adaptável no sistema de arquivos usando a etapa do processo personalizado
seo-description: Gravação de anexos de formulário adaptável no sistema de arquivos usando a etapa do processo personalizado
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '895'
ht-degree: 0%

---


# Etapa do processo personalizado

Este tutorial é destinado aos clientes da AEM Forms que precisam implementar a etapa do processo personalizado. Uma etapa do processo pode executar o script ECMA ou chamar o código java personalizado para executar operações. Este tutorial explicará as etapas necessárias para implementar WorkflowProcess que são executadas pela etapa do processo.

A principal razão para implementar a etapa do processo personalizado é estender o Fluxo de trabalho AEM. Por exemplo, se você estiver usando componentes do AEM Forms em seu modelo de fluxo de trabalho, talvez você queira executar as seguintes operações

* Salve os anexos do formulário adaptativo no sistema de arquivos
* Manipular os dados enviados

Para realizar o caso de uso acima, você normalmente escreverá um serviço OSGi que é executado pela etapa do processo.

## Criar projeto Maven

A primeira etapa é criar um projeto maven usando o Adobe Maven Archetype apropriado. As etapas detalhadas são listadas neste [artigo](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Depois que seu projeto de maven for importado para o eclipse, você estará pronto para o start gravando seu primeiro componente OSGi que pode ser usado na etapa do processo.


### Criar classe que implementa WorkflowProcess

Abra o projeto maven em seu IDE eclipse. Expanda **project name** > pasta **principal** . Expanda a pasta src/main/java. Você deve ver um pacote que termina com &quot;núcleo&quot;. Crie uma classe Java que implemente WorkflowProcess neste pacote. Será necessário substituir o método execute. A assinatura do método execute é a seguinte:public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)lança WorkflowExceptionO método execute dá acesso às três variáveis a seguir

**ItemTrabalho**: A variável workItem dará acesso aos dados relacionados ao fluxo de trabalho. A documentação da API pública está disponível [aqui.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Essa variável workflowSession lhe dará a capacidade de controlar o fluxo de trabalho. A documentação da API pública está disponível [aqui](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Todos os metadados associados ao fluxo de trabalho. Quaisquer argumentos de processo passados para a etapa do processo estão disponíveis usando o objeto MetaDataMap.[Documentação da API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

Neste tutorial, gravaremos os anexos adicionados ao Formulário adaptável no sistema de arquivos como parte do Fluxo de trabalho do AEM.

Para realizar esse caso de uso, a seguinte classe java foi escrita

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

Linha 1 - define as propriedades do nosso componente. A propriedade process.label é o que você verá ao associar o componente OSGi à etapa do processo, conforme mostrado em uma das capturas de tela abaixo.

Linhas 13-15 - Os argumentos do processo passados para esse componente OSGi são divididos usando o separador &quot;,&quot;. Os valores para attachmentPath e saveToLocation são então extraídos da matriz da string.

* attachmentPath - Este é o mesmo local especificado no Formulário adaptável quando você configurou a ação de envio do Formulário adaptável para chamar AEM fluxo de trabalho. Este é o nome da pasta na qual você deseja que os anexos sejam salvos AEM em relação à carga do fluxo de trabalho.

* saveToLocation - este é o local no qual você deseja que os anexos sejam salvos no sistema de arquivos do servidor AEM.

Esses dois valores são passados como argumentos de processo, conforme mostrado na captura de tela abaixo.

![ProcessStep](assets/implement-process-step.gif)


Linha 19: em seguida, construímos o attachmentFilePath. O caminho do arquivo anexo é como

    /var/fd/painel/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/attachments

* Os &quot;Anexos&quot; são o nome da pasta em relação à carga do fluxo de trabalho que foi especificada quando você configurou a opção de envio do Formulário Adaptável.

   ![submitoptions](assets/af-submit-options.gif)

Linhas 24-26 - Obtenha o ResourceResolver e o recurso apontando para o attachmentFilePath.

O restante do código cria objetos de Documento ao interagir pelo objeto filho do recurso que aponta para attachmentFilePath usando a API. Esse objeto de documento é específico do AEM Forms. Em seguida, usamos o método copyToFile do objeto de documento para salvar o objeto de documento.

>[!NOTE]
Como estamos usando um objeto de Documento específico para o AEM Forms, é necessário incluir a dependência aemfd-client-sdk no seu projeto maven. A ID do grupo é com.adobe.aemfd e a ID do artefato é aemfd-client-sdk.

#### Criar e implantar

[Crie o pacote conforme descrito aqui](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)[Verifique se ele está implantado e no estado ativo](http://localhost:4502/system/console/bundles)

Crie um modelo de fluxo de trabalho. Arraste e solte a etapa do processo no modelo de fluxo de trabalho. Associe a etapa do processo a &quot;Salvar anexos de formulário adaptável no sistema de arquivos&quot;.

Forneça os argumentos do processo necessários separados por uma vírgula. Por exemplo, Anexos,c:\\scrappp\\. O primeiro argumento é a pasta na qual os anexos do Formulário adaptativo serão armazenados em relação à carga do fluxo de trabalho. Esse deve ser o mesmo valor especificado ao configurar a ação de envio do Formulário adaptável. O segundo argumento é o local em que você deseja que os anexos sejam armazenados.

Criar um formulário adaptável. Arraste e solte o componente Anexos de arquivo no formulário. Configure a ação de envio do formulário para chamar o fluxo de trabalho criado nas etapas anteriores. Forneça o caminho de anexo apropriado.

Salve as configurações.

Visualizar o formulário. Adicione alguns anexos e envie o formulário. Os anexos devem ser salvos no sistema de arquivos no local especificado por você no fluxo de trabalho.

