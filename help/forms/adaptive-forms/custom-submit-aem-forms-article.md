---
title: Escrever um envio personalizado no AEM Forms
description: Forma rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável
feature: Formulários adaptáveis
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 2%

---


# Escrever um envio personalizado no AEM Forms {#writing-a-custom-submit-in-aem-forms}

Forma rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável

Este artigo o guiará pelas etapas necessárias para criar uma ação de envio personalizada para lidar com o envio do Adaptive Forms.

* Logon no crx
* Crie um nó do tipo &quot;sling :folder &quot; em aplicativos. Vamos chamar este nó CustomSubmitHelpx.
* Salve o nó recém-criado.
* Adicione as duas propriedades a seguir ao nó recém-criado
* PropertyName       | Valor de propriedade
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,básico
* jcr:description   | CustomSubmitHelpx
* Salve as alterações
* Crie um novo arquivo chamado post.POST.jsp no nó CustomSubmitHelpx.Quando um formulário adaptável é enviado, esse JSP é chamado. Você pode gravar o código JSP de acordo com seu requisito neste arquivo. O código a seguir encaminha a solicitação para o servlet.

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

* Crie um arquivo chamado addfields .jsp sob o nó CustomSubmitHelpx . Esse arquivo permitirá que você acesse o documento assinado.
* Adicione o seguinte código a este arquivo

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Salve as alterações

Agora você verá &quot;CustomSubmitHelpx&quot; nas ações de envio do formulário adaptável, conforme mostrado nesta imagem.

![Formulário adaptável com envio personalizado](assets/capture-2.gif)

