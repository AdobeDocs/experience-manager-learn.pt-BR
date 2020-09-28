---
title: Uso do ldap com o Aem Forms Workflow
seo-title: Uso do ldap com o Aem Forms Workflow
description: Atribuição da tarefa de fluxo de trabalho da AEM Forms ao gerente do remetente
seo-description: Atribuição da tarefa de fluxo de trabalho da AEM Forms ao gerente do remetente
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---


# Uso do LDAP com o AEM Forms Workflow

Atribuindo a tarefa de fluxo de trabalho da AEM Forms ao gerente do remetente.

Ao usar o Formulário adaptável no fluxo de trabalho AEM, você deve atribuir dinamicamente uma tarefa ao gerente do remetente do formulário. Para realizar esse caso de uso, teremos que configurar o AEM com o Ldap.

As etapas necessárias para configurar o AEM com LDAP são explicadas em [detalhes aqui.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Para a finalidade deste artigo, estou anexando os arquivos de configuração usados na configuração do AEM com o Adobe Ldap. Esses arquivos estão incluídos no pacote que pode ser importado usando o gerenciador de pacotes.

Na captura de tela abaixo, estamos buscando todos os usuários que pertencem a um centro de custo específico. Se você quiser buscar todos os usuários em seu LDAP, talvez não use o filtro extra.

![Configuração LDAP](assets/costcenterldap.gif)

Na captura de tela abaixo, atribuímos os grupos aos usuários obtidos do LDAP para o AEM. Observe o grupo de usuários de formulários atribuído aos usuários importados. O usuário precisa ser membro desse grupo para interagir com o AEM Forms. Também armazenamos a propriedade manager no nó perfil/gerente no AEM.

![Synchandler](assets/synchandler.gif)

Depois que você tiver configurado o LDAP e os usuários importados no AEM, poderemos criar um fluxo de trabalho que atribuirá a tarefa ao gerente dos enviantes. Para a finalidade deste artigo, desenvolvemos um fluxo de trabalho de aprovação simples de uma etapa.

A primeira etapa do fluxo de trabalho define o valor da etapa inicial como Não. A regra de negócios no formulário adaptável desativará o painel &quot;Detalhes do remetente&quot; e mostrará o painel &quot;Aprovado por&quot; com base no valor da etapa inicial.

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

O trecho de código é responsável por buscar a ID dos gerentes e atribuir a tarefa ao gerente.

Temos contato com a pessoa que iniciou o fluxo de trabalho. Em seguida, obtemos o valor da propriedade manager.

Dependendo de como a propriedade manager é armazenada no LDAP, talvez seja necessário fazer alguma manipulação de sequência de caracteres para obter a ID do gerente.

Leia este artigo para implementar seu próprio [ ParticipantChooser.](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Para testar isso no sistema (para funcionários do Adobe, você pode usar essa amostra na caixa)

* [Baixe e implante o conjunto](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)setvalue. Este é o pacote OSGI personalizado para definir a propriedade do gerente.
* [Baixe e instale o DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importe os Ativos associados a este artigo para AEM usando o gerenciador](assets/aem-forms-ldap.zip)de pacotes.Incluídos como parte deste pacote estão os arquivos de configuração LDAP, o fluxo de trabalho e um formulário adaptável.
* Configure o AEM com seu LDAP usando as credenciais LDAP apropriadas.
* Faça logon para AEM usando suas credenciais LDAP.
* Abrir o [formulário timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha o formulário e envie.
* O gerente do remetente deve obter o formulário para revisão.

>[!NOTE]
>
>Este código personalizado para extrair o nome do gerente foi testado em relação ao LDAP do Adobe. Se você estiver executando esse código em um LDAP diferente, será necessário modificar ou gravar sua própria implementação getParticipant para obter o nome do gerente.
