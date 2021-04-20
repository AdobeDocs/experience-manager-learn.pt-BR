---
title: Abrindo a interface do usuário do agente no envio do POST
seo-title: Abrindo a interface do usuário do agente no envio do POST
description: Esta é a parte 11 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas para o canal de impressão. Nessa parte, iniciaremos a interface da interface do agente para criar uma correspondência ad-hoc no envio do formulário.
seo-description: Esta é a parte 11 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas para o canal de impressão. Nessa parte, iniciaremos a interface da interface do agente para criar uma correspondência ad-hoc no envio do formulário.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 1%

---


# Abrindo a interface do usuário do agente no envio do POST

Nessa parte, iniciaremos a interface da interface do agente para criar uma correspondência ad-hoc no envio do formulário.

Este artigo o guiará pelas etapas envolvidas na abertura da interface da interface do agente ao enviar um formulário. Um caso de uso típico é o de o agente de serviço ao cliente preencher um formulário com alguns parâmetros de entrada e, na interface do usuário do agente de envio de formulário, ele é aberto com dados pré-preenchidos do serviço de preenchimento prévio do modelo de dados de formulário. Os parâmetros de entrada para o serviço de preenchimento prévio do modelo de dados de formulário são extraídos do envio do formulário.

O vídeo a seguir mostra o caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Linha 1 : Obter o número de conta do parâmetro de solicitação

Linha 2-8: Crie o mapa de parâmetros e defina as chaves e os valores apropriados para refletir documentId, Random.

Linha 9-10: Crie outro objeto de Mapa para manter o parâmetro de entrada definido no Modelo de dados de formulário.

Linha 11: Defina o atributo slingRequest &quot;paramMap&quot;

Linha 12-13: Encaminhar a solicitação para o servlet

Para testar esse recurso no servidor

* [Importe e instale os ativos relacionados a este artigo usando o gerenciador de pacotes.](assets/launch-agent-ui.zip)
* [Logon no configMgr](http://localhost:4502/system/console/configMgr)
* Procure por _Filtro CSRF do Adobe Granite_
* Adicione _/content/getprintchannel_ nos Caminhos excluídos
* Salve as alterações.
* [Abra POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Certifique-se de que a cadeia de caracteres passada para FormFieldRequestParameter seja documentId válida.(Linha 19).
* [Abra a página da ](http://localhost:4502/content/OpenPrintChannel.html) Web, insira o número da conta e envie o formulário.
* A interface da interface do usuário do agente deve abrir com os dados pré-preenchidos específicos para o número da conta inserido no formulário.

>[!NOTE]
>
>Verifique se o parâmetro de entrada da operação Get do Modelo de dados de formulário está vinculado ao Atributo de solicitação chamado &quot;accountnumber&quot; para que isso funcione. Se você alterar o nome do valor de vínculo para qualquer outro nome, verifique se a alteração é refletida na linha 25 do POST.jsp

