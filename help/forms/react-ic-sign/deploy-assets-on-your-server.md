---
title: Implante os ativos de amostra no servidor
description: Obtenha o caso de uso que está funcionando no servidor local
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Implantar os ativos

Os seguintes ativos/configurações foram implantados em um servidor de publicação do AEM Forms.

* [Pacote do Wrapper Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Exemplo de modelo de comunicação interativa](assets/waiver-interactive-communication.zip)
* [Implante o pacote DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Adicione a seguinte entrada no serviço Mapeador de Usuário do Apache Sling Service usando o OSGi configMgr
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [É possível baixar o código de exemplo do aplicativo React aqui](assets/src.zip)



O aplicativo de reação de amostra precisa ser implantado no ambiente local

Você terá que alterar o URL do ponto de extremidade para corresponder ao seu ambiente. Abra o arquivo EmergencyContact.js e altere o URL no método de busca

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

Para habilitar fazer chamadas de POST para o terminal de AEM do aplicativo REACT, será necessário especificar as entradas apropriadas no campo Origens permitidas na configuração da Política de Compartilhamento de Recursos entre Origens do Adobe Granite

![definição de cors](assets/cors-settings.png)



