---
title: Configuração do valor do elemento de dados Json no fluxo de trabalho do AEM Forms
description: Como um formulário adaptável é roteado para diferentes usuários no fluxo de trabalho do AEM, há requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para atender a esses casos de uso, normalmente definimos um valor de um campo oculto. Com base no valor desse campo oculto, as regras de negócios podem ser criadas para ocultar/desativar painéis ou campos apropriados.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 126
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---

# Configuração do valor do elemento de dados JSON no fluxo de trabalho do AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Como um formulário adaptável é roteado para diferentes usuários no fluxo de trabalho do AEM, há requisitos para ocultar ou desativar determinados campos ou painéis com base na pessoa que revisa o formulário. Para atender a esses casos de uso, normalmente definimos um valor de um campo oculto. Com base no valor desse campo oculto, as regras de negócios podem ser criadas para ocultar/desativar painéis ou campos apropriados.

![Definindo o valor de um elemento em dados json](assets/capture-3.gif)

No AEM Forms OSGi - devemos criar um pacote OSGi personalizado para definir o valor do elemento de dados JSON. O pacote é fornecido como parte deste tutorial.

Usamos a Etapa do processo no fluxo de trabalho do AEM. Associamos o pacote OSGi &quot;Definir valor do elemento no Json&quot; a esta etapa do processo.

Precisamos passar dois argumentos para o conjunto de valores definido. O primeiro argumento é o caminho para o elemento cujo valor precisa ser definido. O segundo argumento é o valor que precisa ser definido.

Por exemplo, na captura de tela acima, estamos definindo o valor do elemento initialStep como &quot;N&quot;

afData.afUnboundData.data.initialStep,N

Em nosso exemplo, temos um Formulário de solicitação de folga simples. O iniciador deste formulário preenche seu nome e as datas de folga. No envio, este formulário é enviado ao &quot;gerente&quot; para revisão. Quando o gerente abre o formulário, os campos no primeiro painel são desativados. Isso ocorre porque definimos o valor do elemento da etapa inicial nos dados JSON como N.

Com base no valor dos campos de etapa inicial, mostramos o painel do aprovador, no qual o &quot;gerente&quot; pode aprovar ou rejeitar a solicitação.

Consulte as regras definidas em relação à &quot;Etapa inicial&quot;. Com base no valor do campo initialStep, buscamos os detalhes do usuário usando o modelo de dados de formulário e preenchemos os campos apropriados e ocultamos/desabilitamos os painéis apropriados.

Para implantar os ativos no sistema local:

* [Baixe e implante DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e implante o conjunto setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este é o pacote OSGI personalizado que permite definir os valores de um elemento nos dados json enviados.

* [Baixe e extraia o conteúdo do arquivo zip](assets/set-value-jsondata.zip)
   * Aponte seu navegador para [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
      * Importe e instale o SetValueOfElementInJSONDataWorkflow.zip. Este pacote tem o modelo de fluxo de trabalho de amostra e o Modelo de dados de formulário associados ao formulário.

* Aponte seu navegador para [Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar | Upload de arquivo
* Fazer upload do arquivo TimeOffRequestForm.zip
  **Este formulário foi criado usando o AEM Forms 6.4. Verifique se você está no AEM Forms 6.4 ou posterior**
* Abra o [formulário](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Preencha as Datas inicial e final e envie o formulário.
* Ir para [&quot;Caixa de entrada&quot;](http://localhost:4502/aem/inbox)
* Abra o formulário associado à tarefa.
* Observe que os campos no primeiro painel estão desativados.
* Observe que o painel para aprovar ou recusar a solicitação agora está visível.

>[!NOTE]
>
>Como estamos preenchendo previamente o formulário adaptável usando o perfil do usuário, verifique as [informações do perfil do usuário](http://localhost:4502/security/users.html) do administrador. No mínimo, verifique se você definiu os valores dos campos FirstName, LastName e Email.
>Você pode habilitar o log de depuração habilitando o agente de log para com.aemforms.setvalue.core.SetValueInJson [daqui](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>O pacote OSGi para definir o valor dos elementos de dados em dados JSON atualmente suporta a capacidade de definir um valor de elemento de cada vez. Se quiser definir vários valores de elemento, será necessário usar a etapa do processo várias vezes.
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque o código na etapa do processo procura um arquivo chamado Data.xml na pasta de carga útil.
