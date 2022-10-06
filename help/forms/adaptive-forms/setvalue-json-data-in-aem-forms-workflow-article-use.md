---
title: Configuração do valor do elemento de dados Json no fluxo de trabalho do AEM Forms
description: Como um Formulário adaptável é roteado para usuários diferentes AEM fluxo de trabalho, há requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para atender a esses casos de uso, normalmente definimos um valor de um campo oculto. Com base nas regras comerciais de valor desse campo oculto, é possível criar para ocultar/desativar painéis ou campos apropriados.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 1%

---

# Configuração do valor do elemento de dados JSON no fluxo de trabalho do AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Como um Formulário adaptável é roteado para usuários diferentes AEM fluxo de trabalho, há requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para atender a esses casos de uso, normalmente definimos um valor de um campo oculto. Com base nas regras comerciais de valor desse campo oculto, é possível criar para ocultar/desativar painéis ou campos apropriados.

![Configuração do valor de um elemento em dados json](assets/capture-3.gif)

No AEM Forms OSGi - devemos criar um pacote OSGi personalizado para definir o valor do elemento de dados JSON. O pacote é fornecido como parte deste tutorial.

Usamos a Etapa do processo AEM fluxo de trabalho. Associamos o pacote OSGi &quot;Definir valor do elemento no Json&quot; a esta etapa do processo.

Precisamos passar dois argumentos para o pacote de valores definido. O primeiro argumento é o caminho para o elemento cujo valor precisa ser definido. O segundo argumento é o valor que precisa ser definido.

Por exemplo, na captura de tela acima, estamos definindo o valor do elemento intialStep como &quot;N&quot;

afData.afUnboundData.data.initialStep,N

No nosso exemplo, temos um Formulário de solicitação de tempo de desativação simples. O iniciador deste formulário preenche seu nome e o tempo limite. No envio, este formulário é enviado para o &quot;gerente&quot; para revisão. Quando o gerente abre o formulário, os campos no primeiro painel são desativados. Isso porque definimos o valor do elemento da etapa inicial nos dados JSON como N.

Com base no valor dos campos de etapa inicial, mostramos o painel do aprovador, onde o &quot;gerente&quot; pode aprovar ou rejeitar a solicitação.

Consulte as regras definidas em &quot;Etapa inicial&quot;. Com base no valor do campo initialStep , obtemos os detalhes do usuário usando o Modelo de dados de formulário e preenchemos os campos apropriados e ocultamos/desativamos os painéis apropriados.

Para implantar os ativos em seu sistema local:

* [Baixe e implante o DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e implante o pacote setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este é o pacote OSGI personalizado que permite definir os valores de um elemento nos dados json enviados.

* [Baixe e extraia o conteúdo do arquivo zip](assets/set-value-jsondata.zip)
   * Aponte seu navegador para [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
      * Importe e instale o SetValueOfElementInJSONDataWorkflow.zip. Este pacote tem o modelo de fluxo de trabalho de amostra e o Modelo de dados de formulário associados ao formulário.

* Aponte seu navegador para [Forms e documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar | Upload de arquivo
* Carregar arquivo TimeOffRequestForm.zip
   **Este formulário foi criado usando o AEM Forms 6.4. Verifique se você está no AEM Forms 6.4 ou superior**
* Abra o [formulário](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Preencha as Datas inicial e final e envie o formulário.
* Ir para [&quot;Caixa de entrada&quot;](http://localhost:4502/aem/inbox)
* Abra o formulário associado à tarefa.
* Observe que os campos no primeiro painel estão desativados.
* Observe que o painel para aprovar ou recusar a solicitação agora está visível.

>[!NOTE]
>
>Como estamos preenchendo previamente o Formulário adaptável usando o perfil do usuário, verifique se o administrador [informações do perfil do usuário ](http://localhost:4502/security/users.html). No mínimo, certifique-se de ter definido os valores dos campos FirstName, LastName e Email.
>Você pode ativar o log de depuração ativando o logger para com.aemforms.setvalue.core.SetValueInJson [daqui](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>O pacote OSGi para definir o valor dos elementos de dados em dados JSON suporta atualmente a capacidade de definir um valor de elemento de uma vez. Se quiser definir vários valores de elemento, será necessário usar a etapa do processo várias vezes.
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque o código na etapa do processo procura um arquivo chamado Data.xml na pasta de carga útil.
