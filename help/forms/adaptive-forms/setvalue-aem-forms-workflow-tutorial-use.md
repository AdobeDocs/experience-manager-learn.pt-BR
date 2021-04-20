---
title: Uso de setvalue no fluxo de trabalho do AEM Forms
seo-title: Uso de setvalue no fluxo de trabalho do AEM Forms
description: Definir valor do elemento em formulários adaptáveis e dados enviados no AEM Forms OSGI
seo-description: Definir valor do elemento em formulários adaptáveis e dados enviados no AEM Forms OSGI
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 1%

---


# Uso de setvalue no fluxo de trabalho do AEM Forms

Defina o valor de um elemento XML nos dados enviados do Adaptive Forms no fluxo de trabalho OSGI do AEM Forms.

![SetValue](assets/setvalue.png)

O LiveCycle costumava ter um componente de valor definido que permitia definir o valor de um elemento XML.

Com base nesse valor, quando o formulário é preenchido com o XML, é possível ocultar/desativar determinados campos ou painéis do formulário.

No AEM Forms OSGI - teremos que gravar um pacote OSGi personalizado para definir valor no XML. O pacote é fornecido como parte deste tutorial.
Usamos a Etapa do processo no fluxo de trabalho do AEM. Associamos o pacote OSGi &quot;Definir valor do elemento em XML&quot; a esta etapa do processo.
Precisamos passar dois argumentos para o pacote de valores definido. O primeiro argumento é o XPath do elemento XML cujo valor precisa ser definido. O segundo argumento é o valor que precisa ser definido.
Por exemplo, na captura de tela acima, estamos definindo o valor do elemento da etapa inicial como &quot;N&quot;.
Com base nesse valor, determinados painéis nos Formulários adaptáveis estão ocultos ou são exibidos.
No nosso exemplo, temos um Formulário de solicitação de tempo de desativação simples. O iniciador deste formulário preenche seu nome e o tempo limite. No envio, este formulário é enviado para &quot;administrador&quot; e será revisado. Quando o administrador abre o formulário, os campos no primeiro painel são desativados. Isso porque definimos o valor do elemento de etapa inicial no XML como &quot;N&quot;.

Com base no valor dos campos de etapa inicial, mostramos o segundo painel no qual o &quot;administrador&quot; pode aprovar ou rejeitar a solicitação

Consulte as regras definidas em relação ao campo &quot;Tempo desligado solicitado por&quot; usando o editor de regras.

Para implantar os ativos em seu sistema local, siga as etapas abaixo:

* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implante o pacote](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) de amostra. Este é o pacote OSGI personalizado que permite definir os valores de um elemento nos dados xml enviados

* [Baixe e extraia o conteúdo do arquivo zip](assets/setvalueassets.zip)
* Aponte seu navegador para [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe e instale o setValueWorkflow.zip. Este tem o modelo de fluxo de trabalho de amostra.
* Aponte seu navegador para [Formulários e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar | Upload de arquivo
* Fazer upload do TimeOfRequestForm.zip
* Abra o [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Preencha os 3 campos obrigatórios e envie
* Faça logon como &quot;administrador&quot; no AEM (caso ainda não o tenha feito)
* Vá para [&quot;Caixa de entrada do AEM&quot;](http://localhost:4502/aem/inbox)
* Abra o formulário &quot;Tempo de revisão da solicitação&quot;
* Observe que os campos no primeiro painel estão desativados. Isso ocorre porque o formulário está sendo aberto pelo revisor. Além disso, observe que o painel para aprovar ou recusar a solicitação agora está visível

>[!NOTE]
>
>Você pode ativar o log de depuração ativando o logger para
>com.aemforms.setvalue.core.SetValueinXml
>apontando seu navegador para http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Verifique se o caminho do Arquivo de dados nas opções de envio do Formulário adaptável está definido como &quot;Data.xml&quot;. Isso ocorre porque a etapa do processo procura um arquivo chamado Data.xml na pasta de carga útil
