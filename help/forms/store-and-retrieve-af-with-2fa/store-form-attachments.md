---
title: Armazenar anexos de formulário
description: Extraia os anexos do formulário e armazene em um novo local no repositório CRX.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---

# Armazenar anexos de formulário

Ao adicionar anexos a um formulário adaptável, os anexos são armazenados em um local temporário no repositório CRX. Para que nosso caso de uso funcione, precisamos armazenar os anexos de formulário em um novo local no repositório CRX.

O serviço OSGi é criado para armazenar os anexos do formulário em um novo local no repositório CRX. Um novo mapa de arquivos é criado com o novo local dos anexos no CRX e retornado ao aplicativo de chamada.
Veja a seguir o FileMap que é enviado para o servlet. A chave é o campo de formulário adaptável e o valor é o local temporário do anexo. Em nosso servlet, extrairemos o anexo e o armazenaremos em um novo local no repositório AEM e atualizaremos o FileMap com o novo local

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

A seguir está o código que extrai os anexos da solicitação e os armazena em **/content/afattachments** pasta

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

Este é o novo FileMap com o local atualizado dos anexos de formulário

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## Próximas etapas

[Salvar os dados do formulário](./store-form-data.md)