---
title: Implantar os ativos de amostra no servidor
description: Fazer com que o caso de uso funcione no servidor local
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Implantar os ativos

Os ativos/configurações a seguir foram implantados em um servidor de publicação do AEM Forms.

* [Pacote do Adobe Sign Wrapper](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Modelo de comunicação interativa de amostra](assets/waiver-interactive-communication.zip)
* [Implantar o pacote DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=pt-BR)
* Adicione a seguinte entrada no serviço Mapeador de usuários do Apache Sling Service usando o OSGi configMgr
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## Implantar o aplicativo de amostra do react

* [Baixe o aplicativo de amostra do react](assets/mult-step-form1.zip)
* Descompacte o conteúdo do aplicativo react em uma nova pasta
* Navegue até a pasta e execute os seguintes comandos

```java
npm install
npm start
```

Abra o arquivo EmergencyContact.js e altere o URL no método de busca para corresponder ao seu ambiente.


```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

Para permitir fazer chamadas POST para o endpoint do AEM a partir do aplicativo REACT, será necessário especificar as entradas apropriadas no campo Origens permitidas na configuração da Política de compartilhamento de recursos entre origens do Adobe Granite.

![configuração de cors](assets/cors-settings.png)

Consulte [noções básicas sobre o CORS com o AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=pt-BR) para obter mais detalhes sobre as opções de configuração do CORS.
