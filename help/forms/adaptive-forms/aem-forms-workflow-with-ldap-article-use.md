---
title: Utilização do LDAP com o fluxo de trabalho do AEM Forms
description: Atribuir a tarefa de fluxo de trabalho AEM Forms ao gerente do remetente
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 111
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Utilização do LDAP com o fluxo de trabalho do AEM Forms

Atribuir a tarefa de fluxo de trabalho AEM Forms ao gerente do remetente.

Ao usar o formulário adaptável no fluxo de trabalho AEM, você atribuirá dinamicamente uma tarefa ao gerente do remetente do formulário. Para realizar esse caso de uso, precisaremos configurar o AEM com Ldap.

As etapas necessárias para configurar o AEM com LDAP são explicadas em [detalhe aqui.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Para os fins deste artigo, estou anexando arquivos de configuração usados na configuração do AEM com Adobe Ldap. Esses arquivos estão incluídos no pacote que pode ser importado usando o gerenciador de pacotes.

Na captura de tela abaixo, buscamos todos os usuários que pertencem a um centro de custo específico. Se quiser buscar todos os usuários no LDAP, você não poderá usar o filtro extra.

![Configuração LDAP](assets/costcenterldap.gif)

Na captura de tela abaixo, atribuímos os grupos aos usuários buscados do LDAP para o AEM. Observe o grupo de formulários-usuários atribuído aos usuários importados. O usuário precisa ser membro desse grupo para interação com o AEM Forms. Também armazenamos a propriedade manager no nó profile/manager no AEM.

![Synchandler](assets/synchandler.gif)

Depois de configurar o LDAP e importar usuários para AEM, podemos criar um workflow que atribuirá a tarefa ao gerente dos remetentes. Para o propósito deste artigo, desenvolvemos um simples fluxo de trabalho de aprovação de uma etapa.

A primeira etapa do fluxo de trabalho definiu o valor de initialstep como No. A regra de negócios no formulário adaptável desativará o painel &quot;Detalhes do remetente&quot; e mostrará o painel &quot;Aprovado por&quot; com base no valor da etapa inicial.

A segunda etapa atribui a tarefa ao gerente do remetente. Obtemos o gerente do remetente usando o código personalizado.

![Atribuir tarefa](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

O trecho de código é responsável por buscar a id de gerentes e atribuir a tarefa ao gerente.

Pegamos a pessoa que iniciou o workflow. Em seguida, obtemos o valor da propriedade do gerenciador.

Dependendo de como a propriedade do gerenciador é armazenada no LDAP, talvez seja necessário fazer alguma manipulação de string para obter a ID do gerenciador.

Leia este artigo para implementar o seu próprio [ParticipantChooser.](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Para testá-lo em seu sistema (para os funcionários da Adobe, você pode usar essa amostra imediatamente)

* [Baixe e implante o pacote setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este é o pacote OSGI personalizado para definir a propriedade do gerenciador.
* [Baixe e instale o DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importe os Ativos associados a este artigo para o AEM usando o gerenciador de pacotes](assets/aem-forms-ldap.zip).Incluídos como parte desse pacote estão os arquivos de configuração LDAP, fluxo de trabalho e um formulário adaptável.
* Configure o AEM com seu LDAP usando as credenciais LDAP apropriadas.
* Faça logon no AEM usando suas credenciais LDAP.
* Abra o [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha o formulário e envie.
* O gerente do remetente deve obter o formulário para revisão.

>[!NOTE]
>
>Este código personalizado para extrair o nome do gerenciador foi testado em relação ao LDAP do Adobe. Se você estiver executando esse código em um LDAP diferente, será necessário modificar ou gravar sua própria implementação getParticipant para obter o nome do gerente.
