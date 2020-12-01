---
title: Criar o formulário MyAccount
description: Crie o formulário da minha conta para recuperar o formulário parcialmente preenchido na verificação bem-sucedida da ID do aplicativo e do número de telefone.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# Criar o formulário MyAccount

O formulário **MyAccountForm** é usado para recuperar o formulário adaptativo parcialmente preenchido depois que o usuário verifica a ID do aplicativo e o número do dispositivo móvel associado à ID do aplicativo.

![meu formulário de conta](assets/6599.JPG)

Quando o usuário digita a ID do aplicativo e clica no botão **FetchApplication**, o número móvel associado à ID do aplicativo é obtido do banco de dados usando a operação Get do modelo de dados do formulário.

Este formulário usa a invocação POST do Modelo de dados de formulário para verificar o número do dispositivo móvel usando OTP. A ação de envio do formulário é acionada na verificação bem-sucedida do número do dispositivo móvel usando o código a seguir. Estamos acionando o evento click do botão de envio chamado **submitForm**.

>[!NOTE]
> Será necessário fornecer a chave da API e os valores do segredo da API específicos para sua conta [Nexmo](https://dashboard.nexmo.com/) nos campos apropriados de MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Este formulário está associado à ação de envio personalizado que encaminha o envio do formulário para o servlet montado em **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

O código no servlet montado em **/bin/renderaf** encaminha a solicitação para renderizar o formulário adaptável storeafwithattachments pré-preenchido com os dados salvos.


* O MyAccountForm pode ser [baixado daqui](assets/my-account-form.zip)

* Os formulários de amostra são baseados em [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisa ser importado para AEM para que os formulários de amostra sejam renderizados corretamente.

* [O ](assets/custom-submit-my-account-form.zip) manipulador de envio personalizado associado ao envio de MyAccountForm precisa ser importado para o AEM.
