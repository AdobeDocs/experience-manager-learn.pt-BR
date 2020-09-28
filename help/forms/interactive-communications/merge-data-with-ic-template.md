---
title: Gerando Documento de Canal de impressão mesclando dados
seo-title: Gerando Documento de Canal de impressão mesclando dados
description: Saiba como gerar documentos de canal de impressão mesclando dados contidos no fluxo de entrada
seo-description: Saiba como gerar documentos de canal de impressão mesclando dados contidos no fluxo de entrada
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Gerar Documentos de Canal de impressão usando dados enviados

Documentos de canal de impressão geralmente são gerados pela busca de dados de uma fonte de dados de backend pelo serviço de obtenção do modelo de dados de formulário. Em alguns casos, pode ser necessário gerar documentos de canal de impressão com os dados fornecidos. Por exemplo: o Cliente preenche a alteração do formulário beneficiário e você pode gerar o documento do canal de impressão com os dados do formulário enviado. Para realizar esse caso de uso, siga as etapas a seguir

## Criar serviço de preenchimento prévio

O nome do serviço &quot;ccm-print-test&quot; será usado para acessar este serviço. Depois que esse serviço de pré-preenchimento for definido, você poderá acessar esse serviço na implementação da etapa do servidor ou do processo de fluxo de trabalho para gerar o documento do canal de impressão.

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### Criar implementação de WorkflowProcess

O trecho de código de implementação workflowProcess é mostrado abaixo. Esse código é executado quando a etapa do processo no Fluxo de trabalho AEM está associada a essa implementação. Essa implementação espera três argumentos do processo descritos abaixo:

* Nome do caminho DataFile especificado ao configurar o Formulário adaptável
* Nome do modelo de canal de impressão
* Nome do documento de canal de impressão gerado

Linha 98 - Como o Formulário adaptável se baseia no Modelo de dados de formulário, os dados que residem no nó de dados do afBoundData são extraídos.
Linha 128 - O nome do serviço Opções de dados está definido. Anote o nome do serviço. Ele deve corresponder ao nome retornado na Linha 45 da lista de códigos anterior.
Linha 135 - O Documento é gerado usando o método de renderização do objeto PrintChannel


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

Para testar isso em seu servidor, siga as seguintes etapas:

* [Configure o serviço de e-mail Day CQ.](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) Isso é necessário para enviar emails com o documento gerado como anexo.
* [Implantar o pacote de desenvolvedores com usuários de serviços](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Verifique se você adicionou a seguinte entrada na Configuração do serviço Mapeador de Usuário do Apache Sling Service
* **DevelopingWithServiceUser.core:getformsresouresolver=fd-service**
* [Baixe e descompacte os ativos relacionados a este artigo no seu sistema de arquivos](assets/prefillservice.zip)
* [Importe os seguintes pacotes usando o Gerenciador de pacote AEM](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Implantar o seguinte usando o AEM Felix Web Console](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Este pacote contém o código mencionado neste artigo.

* [Abrir ChangeOfBeneficiaryForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Verifique se o formulário adaptável está configurado para enviar para AEM fluxo de trabalho, como mostrado abaixo
   ![imagem](assets/generateic.PNG)
* [Configure o modelo de fluxo de trabalho.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Verifique se a etapa do processo e os componentes de e-mail de envio estão configurados de acordo com o seu ambiente
* [Pré-visualização ChangeOfBenaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) Preencha alguns detalhes e envie
* O fluxo de trabalho deve ser chamado e o documento de canal de impressão IC deve ser enviado para o recipient especificado no componente de email de envio como um anexo
