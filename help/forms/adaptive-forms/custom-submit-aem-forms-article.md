---
title: Escrever um envio personalizado no AEM Forms
description: Maneira rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 51
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 1%

---

# Escrever um envio personalizado no AEM Forms {#writing-a-custom-submit-in-aem-forms}

Maneira rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável

>[!NOTE]
>O código deste artigo não funciona com os componentes principais baseados em formulário adaptável.
>[O artigo equivalente para o formulário adaptável baseado em componente principal está disponível aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/custom-submit-headless-forms/custom-submit-service.html?lang=pt-BR)


Este artigo guiará você pelas etapas necessárias para criar uma ação de envio personalizada para lidar com o envio do Adaptive Forms.

* Logon no crx
* Crie um nó do tipo &quot;sling :folder&quot; em aplicativos. Vamos chamar este nó de CustomSubmitHelpx.
* Salve o nó recém-criado.
* Adicione as três propriedades a seguir ao nó recém-criado

| Nome de propriedade | Valor de propriedade |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa,xsd,basic |
| jcr :description | CustomSubmitHelpx |


* Salve as alterações
* Crie um novo arquivo chamado post.POST.jsp no nó CustomSubmitHelpx.Quando um formulário adaptável é enviado, esse JSP é chamado. Você pode gravar o código JSP de acordo com seu requisito nesse arquivo. O código a seguir encaminha a solicitação ao servlet.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* Crie o arquivo chamado addfields .jsp no nó CustomSubmitHelpx. Este arquivo permitirá que você acesse o documento assinado.
* Adicionar o código a seguir a este arquivo

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Salve as alterações

Agora você começará a ver &quot;CustomSubmitHelpx&quot; nas ações de envio do seu Formulário adaptável, como mostrado nesta imagem.

![Formulário adaptável com envio personalizado](assets/capture-2.gif)
