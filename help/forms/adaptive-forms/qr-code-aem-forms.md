---
title: Exibição do código QR no Formulário adaptável
description: Exibir código QR em um formulário adaptável
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
exl-id: 0c6079f4-601e-4a82-976c-71dbb2faa671
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Componente de amostra do código QR

A incorporação de um código QR em um Formulário adaptável pode melhorar muito a conveniência e a eficiência para que os usuários acessem informações adicionais relacionadas ao formulário.

O componente de exemplo usa [QRCode.js](https://davidshimjs.github.io/qrcodejs/).

O QRCode.js é uma biblioteca javascript para fazer QRCode, ele suporta Cross-browser com HTML5 Canvas e tag de tabela no DOM.

O componente gera o código QR com base no valor especificado na propriedade de configuração do componente.
![imagem](assets/qr-code-url.png)

O código a seguir foi usado no body.jsp do componente qr-code-generator.

O &quot;url&quot; é o url que precisa ser incorporado no código qr. Esta url é especificada nas propriedades de configuração do componente de código QR.

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



O código a seguir usa o método makeCode da biblioteca QRCode.js na biblioteca do cliente do componente qr-code-generator. O código QR gerado é anexado ao div identificado pela id **&quot;qrcode&quot;**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## Implantar os ativos no servidor local

* [Baixe e instale o componente de código QR usando o Gerenciador de pacotes.](assets/qrcode.zip)
* [Baixe e instale o formulário adaptável de exemplo usando o Gerenciador de pacotes.](assets/form-with-qr-code.zip)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). A seção de ajuda do formulário tem o código QR.
