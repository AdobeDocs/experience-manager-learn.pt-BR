---
title: Configuração do valor do elemento de dados Json no AEM Forms Workflow
seo-title: Configuração do valor do elemento de dados Json no AEM Forms Workflow
description: Como um Formulário adaptável é roteado para diferentes usuários AEM fluxo de trabalho, haverá requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para satisfazer esses casos de uso, normalmente definimos um valor de um campo oculto. Com base nas regras de negócios de valor desse campo oculto, é possível criar para ocultar/desativar os painéis ou campos apropriados.
seo-description: Como um Formulário adaptável é roteado para diferentes usuários AEM fluxo de trabalho, haverá requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para satisfazer esses casos de uso, normalmente definimos um valor de um campo oculto. Com base nas regras de negócios de valor desse campo oculto, é possível criar para ocultar/desativar os painéis ou campos apropriados.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: 233ad7184cb48098253a78c07a3913356ac9e774
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# Configuração do valor do elemento de dados JSON no AEM Forms Workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Como um Formulário adaptável é roteado para diferentes usuários AEM fluxo de trabalho, haverá requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para satisfazer esses casos de uso, normalmente definimos um valor de um campo oculto. Com base nas regras de negócios de valor desse campo oculto, é possível criar para ocultar/desativar os painéis ou campos apropriados.

![Configuração do valor de um elemento em dados json](assets/capture-3.gif)

No AEM Forms OSGI - será necessário gravar um pacote OSGi personalizado para definir o valor do elemento de dados JSON. O pacote é fornecido como parte deste tutorial.

Usamos a Etapa do processo AEM fluxo de trabalho. Associamos o pacote OSGi &quot;Definir valor do elemento em Json&quot; a essa etapa do processo.

Precisamos passar dois argumentos para o conjunto de valores definido. O primeiro argumento é o caminho para o elemento cujo valor precisa ser definido. O segundo argumento é o valor que precisa ser definido.

Por exemplo, na captura de tela acima, estamos definindo o valor do elemento intialStep como &quot;N&quot;

afData.afUnboundData.data.initialStep,N

Em nosso exemplo, temos um formulário de solicitação de tempo limite. O iniciador deste formulário preenche seu nome e as datas de folga. No envio, este formulário é enviado para o &quot;gerente&quot; para revisão. Quando o gerente abre o formulário, os campos no primeiro painel são desativados. Isso porque definimos o valor do elemento de etapa inicial nos dados JSON como N.

Com base no valor dos campos de etapa iniciais, mostramos o painel do aprovador no qual o &quot;gerente&quot; pode aprovar ou rejeitar a solicitação.

Consulte as regras definidas em &quot;Etapa inicial&quot;. Com base no valor do campo initialStep, buscamos os detalhes do usuário usando o Modelo de dados do formulário e preenchemos os campos apropriados e ocultamos/desativamos os painéis apropriados.

Para implantar os ativos no sistema local:

* [Baixe e implante DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e implante o conjunto](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Este é o pacote OSGI personalizado que permite definir os valores de um elemento nos dados json enviados.

* [Baixe e extraia o conteúdo do arquivo zip](assets/set-value-jsondata.zip)
   * Aponte seu navegador para [gerenciador de pacote](http://localhost:4502/crx/packmgr/index.jsp)
      * Importe e instale o SetValueOfElementInJSONDataWorkflow.zip.Este pacote tem o modelo de fluxo de trabalho de amostra e o Modelo de dados de formulário associados ao formulário.

* Aponte seu navegador para [Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar | Upload de arquivo
* Carregar arquivo TimeOffRequestForm.zip
   **Este formulário foi criado com o AEM Forms 6.4. Certifique-se de estar no AEM Forms 6.4 ou superior**
* Abra o [formulário](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Preencha o Start e as Datas finais e envie o formulário.
* Ir para [&quot;Caixa de entrada&quot;](http://localhost:4502/aem/inbox)
* Abra o formulário associado à tarefa.
* Observe que os campos no primeiro painel estão desativados.
* Observe que o painel para aprovar ou recusar a solicitação agora está visível.

>[!NOTE]
>
>Como estamos preenchendo previamente o Formulário adaptativo usando o perfil do usuário, certifique-se de que as [informações do perfil do usuário do administrador ](http://localhost:4502/security/users.html) estejam preenchidas. No mínimo, certifique-se de ter definido os valores dos campos Nome, Sobrenome e Email.
>Você pode ativar o registro de depuração ativando o agente de log para com.aemforms.setvalue.core.SetValueInJson [daqui](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>O pacote OSGi para definir o valor dos elementos de dados em Dados JSON suporta atualmente a capacidade de definir um valor de elemento de uma vez. Se você quiser definir vários valores de elemento, será necessário usar a etapa do processo várias vezes.
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque o código na etapa do processo procura um arquivo chamado Data.xml na pasta payload.
