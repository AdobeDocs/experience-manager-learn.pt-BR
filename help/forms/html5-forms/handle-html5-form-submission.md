---
title: Lidar com o envio de um formulário HTML5
description: Criar manipulador de envio de formulário HTML5.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# Lidar com o envio de formulário do HTML5

Os formulários do HTML5 podem ser enviados a um servlet hospedado no AEM. Os dados enviados podem ser acessados no servlet como um fluxo de entrada. Para enviar o formulário HTML5, adicione um &quot;Botão de envio HTTP&quot; ao modelo de formulário usando o AEM Forms Designer.

## Criar seu manipulador de envio

Um servlet simples pode lidar com o envio do formulário HTML5. Extraia os dados enviados usando o seguinte trecho de código. Baixe o [servlet](assets/html5-submit-handler.zip) fornecido neste tutorial. Instale o [servlet](assets/html5-submit-handler.zip) usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp).

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

Verifique se você configurou a [Configuração do SDK do Adobe LiveCycle Client](https://helpx.adobe.com/br/aem-forms/6/submit-form-data-livecycle-process.html) se planeja usar o código para invocar um processo J2EE.

## Configurar a URL de envio do formulário HTML5

![Enviar URL](assets/submit-url.PNG)

- Abra o xdp e navegue até _Propriedades_->_Avançadas_.
- Copie o http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html e cole-o no campo de texto Enviar URL.
- Clique no botão _SalvarEFechar_.

### Adicionar entrada nos Caminhos de exclusão

- Vá para [configMgr](http://localhost:4502/system/console/configMgr).
- Pesquise por _Filtro CSRF do Adobe Granite_.
- Adicione a seguinte entrada na seção Caminhos Excluídos: _/content/AemFormsSamples/handlehml5formsubmit_.
- Salve as alterações.

### Testar o formulário

- Abra o modelo xdp.
- Clique em _Visualizar_->Visualizar como HTML.
- Insira os dados no formulário e clique em enviar.
- Verifique os dados enviados no arquivo stdout.log do servidor.

### Leitura adicional

Para obter mais informações sobre como gerar PDFs a partir de envios de formulários do HTML5, consulte este [artigo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=pt-BR).

