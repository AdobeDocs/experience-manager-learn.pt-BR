---
title: Escrever um envio personalizado no AEM Forms
seo-title: Escrever um envio personalizado no AEM Forms
description: Forma rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável
seo-description: Forma rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável
feature: formulários adaptáveis
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---


# Escrever um envio personalizado no AEM Forms {#writing-a-custom-submit-in-aem-forms}

Forma rápida e fácil de criar sua própria ação de envio personalizada para o Formulário adaptável

Este artigo o guiará pelas etapas necessárias para criar uma ação de envio personalizada para lidar com o envio de formulários adaptativos.

* Logon no crx
* Crie um nó do tipo &quot;sling :folder &quot; em aplicativos. Vamos chamar este nó CustomSubmitHelpx.
* Salve o nó recém-criado.
* Adicione as duas propriedades a seguir ao nó recém-criado
* PropertyName       | Valor de propriedade
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,básico
* jcr:description   | CustomSubmitHelpx
* Salve as alterações
* Crie um novo arquivo chamado post.POST.jsp sob o nó CustomSubmitHelpx.Quando um formulário adaptável é enviado, esse JSP é chamado. Você pode gravar o código JSP de acordo com seu requisito neste arquivo. O código a seguir encaminha a solicitação para o servlet.

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

