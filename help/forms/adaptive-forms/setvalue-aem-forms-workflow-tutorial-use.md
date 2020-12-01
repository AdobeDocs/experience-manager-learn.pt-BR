---
title: Usar setvalue no fluxo de trabalho do AEM Forms
seo-title: Uso de setvalue no AEM Forms Workflow
description: Definir valor do elemento nos dados submetidos pela Forms adaptável no AEM Forms OSGI
seo-description: Definir valor do elemento nos dados submetidos pela Forms adaptável no AEM Forms OSGI
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---


# Usar setvalue no fluxo de trabalho do AEM Forms

Defina o valor de um elemento XML em dados enviados pela Forms Adaptive no fluxo de trabalho OSGI da AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle usado para ter um componente de valor definido que permitisse definir o valor de um elemento XML.

Com base nesse valor, quando o formulário é preenchido com o XML, é possível ocultar/desativar determinados campos ou painéis do formulário.

No AEM Forms OSGI - será necessário gravar um pacote OSGi personalizado para definir o valor no XML. O pacote é fornecido como parte deste tutorial.
Usamos a Etapa do processo AEM fluxo de trabalho. Associamos o pacote OSGi &quot;Definir valor do elemento em XML&quot; a essa etapa do processo.
Precisamos passar dois argumentos para o conjunto de valores definido. O primeiro argumento é o XPath do elemento XML cujo valor precisa ser definido. O segundo argumento é o valor que precisa ser definido.
Por exemplo, na captura de tela acima, estamos definindo o valor do elemento intialstep como &quot;N&quot;.
Com base nesse valor, alguns painéis no Adaptador Forms estão ocultos ou são exibidos.
Em nosso exemplo, temos um formulário de solicitação de tempo limite. O iniciador deste formulário preenche seu nome e as datas de folga. No envio, este formulário é enviado para &quot;admin&quot; para revisão. Quando o administrador abre o formulário, os campos no primeiro painel são desativados. Isso porque definimos o valor do elemento de etapa inicial no XML como &quot;N&quot;.

Com base no valor dos campos de etapa iniciais, mostramos o segundo painel no qual o &quot;administrador&quot; pode aprovar ou rejeitar a solicitação

Observe as regras definidas no campo &quot;Tempo de folga solicitado por&quot; usando o editor de regras.

Para implantar os ativos no sistema local, siga as etapas abaixo:

* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implante o pacote](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) de amostras. Este é o pacote OSGI personalizado que permite definir os valores de um elemento nos dados xml enviados

* [Baixe e extraia o conteúdo do arquivo zip](assets/setvalueassets.zip)
* Aponte seu navegador para [gerenciador de pacote](http://localhost:4502/crx/packmgr/index.jsp)
* Importe e instale o setValueWorkflow.zip. Este tem o modelo de fluxo de trabalho de exemplo.
* Aponte seu navegador para [Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar | Upload de arquivo
* Carregar TimeOfRequestForm.zip
* Abra o [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Preencha os 3 campos obrigatórios e envie
* Faça logon como &quot;admin&quot; no AEM (caso ainda não tenha feito isso)
* Ir para [&quot;Caixa de entrada AEM&quot;](http://localhost:4502/aem/inbox)
* Abra o formulário &quot;Tempo de revisão da solicitação&quot;
* Observe que os campos no primeiro painel estão desativados. Isso ocorre porque o formulário está sendo aberto pelo revisor. Além disso, observe que o painel para aprovar ou recusar a solicitação agora está visível

>[!NOTE]
>
>Você pode ativar o registro de depuração ativando o agente de log para
>com.aemforms.setvalue.core.SetValueinXml
>apontando seu navegador para http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque a etapa do processo procura um arquivo chamado Data.xml na pasta payload
