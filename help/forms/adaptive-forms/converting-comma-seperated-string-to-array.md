---
title: Conversão de string separada por vírgulas em matriz de strings no AEM Forms Workflow
description: quando o modelo de dados de formulário tiver uma matriz de strings como um dos parâmetros de entrada, será necessário massajar os dados gerados a partir da ação de envio de um formulário adaptável antes de invocar a ação de envio do modelo de dados de formulário.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
kt: 8507
source-git-commit: e01d93591d1c00b2abec3430fdfa695b32165e54
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---


# Conversão de sequência separada por vírgulas em matriz de sequência {#setting-value-of-json-data-element-in-aem-forms-workflow}

Quando o formulário for baseado em um modelo de dados de formulário que tenha uma matriz de strings como parâmetro de entrada, será necessário manipular os dados de formulário adaptáveis enviados para inserir uma matriz de strings. Como exemplo, se você tiver vinculado um campo de caixa de seleção a um elemento de modelo de dados de formulário do tipo matriz de sequência de caracteres, os dados do campo de caixa de seleção estarão em um formato de sequência de caracteres separado por vírgulas. O código de amostra listado abaixo mostra como substituir a string separada por vírgulas por uma matriz de strings.

## Criar uma etapa do processo

Uma etapa do processo é usada em um fluxo de trabalho AEM quando queremos que nosso fluxo de trabalho execute uma determinada lógica. A etapa do processo pode ser associada a um script ECMA ou a um serviço OSGi. Nossa etapa do processo personalizado executará o serviço OSGi

Os dados enviados estão no seguinte formato. O valor do elemento businessUnits é uma string separada por vírgulas, que precisa ser convertida em uma matriz de string.

![enviar dados](assets/submitted-data-string.png)

Os dados de entrada do terminal restante associado ao modelo de dados de formulário esperam uma matriz de sequências de caracteres como mostrado nesta captura de tela. O código personalizado na etapa do processo converterá os dados enviados no formato correto.

![fdm-string-array](assets/string-array-fdm.png)

Passamos o caminho do objeto JSON e o nome do elemento para a etapa do processo. O código na etapa do processo substituirá os valores separados por vírgulas do elemento em uma matriz de sequências de caracteres.
![etapa do processo](assets/create-string-array.png)

>[!NOTE]
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque o código na etapa do processo procura um arquivo chamado Data.xml na pasta de carga útil.

## Código de etapa do processo

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

@Component(property = {
    Constants.SERVICE_DESCRIPTION + "=Create String Array",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Replace comma seperated string with string array"
})

public class CreateStringArray implements WorkflowProcess {
    private static final Logger log = LoggerFactory.getLogger(CreateStringArray.class);
    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
        log.debug("The string I got was ..." + arg2.get("PROCESS_ARGS", "string").toString());
        String[] arguments = arg2.get("PROCESS_ARGS", "string").toString().split(",");
        String objectName = arguments[0];
        String propertyName = arguments[1];

        String objects[] = objectName.split("\\.");
        System.out.println("The params is " + propertyName);
        log.debug("The params string is " + objectName);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload  in set Elmement Value in Json is  " + workItem.getWorkflowData().getPayload().toString());
        String dataFilePath = payloadPath + "/Data.xml/jcr:content";
        Session session = workflowSession.adaptTo(Session.class);
        Node submittedDataNode = null;
        try {
            submittedDataNode = session.getNode(dataFilePath);

            InputStream submittedDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(submittedDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();

            String inputStr;
            while ((inputStr = streamReader.readLine()) != null)
                stringBuilder.append(inputStr);
            JsonParser jsonParser = new JsonParser();
            JsonObject jsonObject = jsonParser.parse(stringBuilder.toString()).getAsJsonObject();
            System.out.println("The json object that I got was " + jsonObject);
            JsonObject targetObject = null;

            for (int i = 0; i < objects.length - 1; i++) {
                System.out.println("The object name is " + objects[i]);
                if (i == 0) {
                    targetObject = jsonObject.get(objects[i]).getAsJsonObject();
                } else {
                    targetObject = targetObject.get(objects[i]).getAsJsonObject();

                }

            }

            System.out.println("The final object is " + targetObject.toString());
            String businessUnits = targetObject.get(propertyName).getAsString();
            System.out.println("The values of " + propertyName + " are " + businessUnits);

            JsonArray jsonArray = new JsonArray();

            String[] businessUnitsArray = businessUnits.split(",");
            for (String name: businessUnitsArray) {
                jsonArray.add(name);
            }

            targetObject.add(propertyName, jsonArray);
            System.out.println(" After updating the property " + targetObject.toString());
            InputStream is = new ByteArrayInputStream(jsonObject.toString().getBytes());
            System.out.println("The changed json data  is " + jsonObject.toString());
            Binary binary = session.getValueFactory().createBinary(is);
            submittedDataNode.setProperty("jcr:data", binary);
            session.save();

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

    }
}
```

O pacote de amostra pode ser [baixado aqui](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)