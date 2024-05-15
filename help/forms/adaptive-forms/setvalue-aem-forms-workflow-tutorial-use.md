---
title: Uso de setvalue no workflow do AEM Forms
description: Definir o valor do elemento nos dados enviados do Adaptive Forms no AEM Forms OSGI
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 105
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# Uso de setvalue no workflow do AEM Forms

Definir o valor de um elemento XML nos dados enviados pelo Forms adaptável no fluxo de trabalho OSGI do AEM Forms.

![DefinirValor](assets/setvalue.png)

O LiveCycle costumava ter um componente de valor definido que permitia definir o valor de um elemento XML.

Com base nesse valor, quando o formulário é preenchido com o XML, você pode ocultar/desativar determinados campos ou painéis do formulário.

No AEM Forms OSGi, teremos que gravar um pacote OSGi personalizado para definir o valor no XML. O pacote é fornecido como parte deste tutorial.
Usamos a Etapa do processo no fluxo de trabalho do AEM. Associamos o pacote OSGi &quot;Definir valor do elemento em XML&quot; a esta etapa do processo.
Precisamos passar dois argumentos para o conjunto de valores definido. O primeiro argumento é o XPath do elemento XML cujo valor precisa ser definido. O segundo argumento é o valor que precisa ser definido.
Por exemplo, na captura de tela acima, estamos definindo o valor do elemento initialstep como &quot;N&quot;.
Com base nesse valor, determinados painéis no Forms adaptável ficam ocultos ou são exibidos.
Em nosso exemplo, temos um Formulário de solicitação de folga simples. O iniciador deste formulário preenche seu nome e as datas de folga. No envio, este formulário é enviado ao &quot;administrador&quot; para revisão. Quando o administrador abre o formulário, os campos no primeiro painel são desativados. Isso porque definimos o valor do elemento da etapa inicial no XML como &quot;N&quot;.

Com base no valor dos campos de etapa inicial, mostramos o segundo painel em que o &quot;administrador&quot; pode aprovar ou rejeitar a solicitação

Consulte as regras definidas no campo &quot;Folga solicitada por&quot; usando o editor de regras.

Para implantar os ativos no sistema local, siga as etapas abaixo:

* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implante o pacote de amostra](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este é o pacote OSGI personalizado que permite definir os valores de um elemento nos dados xml enviados

* [Baixe e extraia o conteúdo do arquivo zip](assets/setvalueassets.zip)
* Aponte seu navegador para [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe e instale o setValueWorkflow.zip. Ele tem o modelo de fluxo de trabalho de amostra.
* Aponte seu navegador para [Forms e documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar | Upload de arquivo
* Fazer upload do arquivo TimeOfRequestForm.zip
* Abra o [FormuláriodeSolicitaçãodeFolga](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Preencha os 3 campos obrigatórios e envie
* Fazer logon como &quot;administrador&quot; no AEM (se você ainda não tiver feito isso)
* Ir para [&quot;Caixa de entrada AEM&quot;](http://localhost:4502/aem/inbox)
* Abra o formulário &quot;Revisar solicitação de folga&quot;
* Observe que os campos no primeiro painel estão desativados. Isso ocorre porque o formulário está sendo aberto pelo revisor. Além disso, observe que o painel para aprovar ou recusar a solicitação agora está visível

>[!NOTE]
>
>Você pode ativar o log de depuração ativando o logger para
>com.aemforms.setvalue.core.SetValueinXml
>apontando seu navegador para http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque a etapa do processo procura um arquivo chamado Data.xml na pasta de carga útil
