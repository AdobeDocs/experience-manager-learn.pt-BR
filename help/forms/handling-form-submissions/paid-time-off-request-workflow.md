---
title: Fluxo de Trabalho de Solicitação de Tempo Pago Simples
description: Ocultar e mostrar painéis de formulário adaptáveis no fluxo de trabalho AEM
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integrations
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---


# Fluxo de Trabalho de Solicitação de Tempo Pago Simples

Neste artigo, observaremos um fluxo de trabalho simples usado para solicitar Tempo de espera pago. Os requisitos de negócio são os seguintes:

* O usuário A solicita o tempo limite ao preencher um formulário adaptável.
* O formulário é roteado para AEM usuário administrador (na vida real, ele será roteado para o gerente do remetente)
* O administrador abre o formulário. O administrador não deve ser capaz de editar nenhuma informação preenchida pelo remetente.
* A seção Aprovador deve estar visível para o aprovador (nesse caso, é o usuário administrador AEM).

Para cumprir o requisito acima, usamos um campo oculto chamado **initialstep** no formulário e seu valor padrão é definido como Sim.Quando o formulário é submetido, a primeira etapa do fluxo de trabalho define o valor da etapa inicial como Não. O formulário tem regras de negócios para ocultar e mostrar as seções apropriadas com base no valor da etapa inicial.

**Configurar formulário para acionar AEM fluxo de trabalho**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Caminhada do fluxo de trabalho**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Visualização do formulário Solicitação de tempo limite do remetente**

![initialstep](assets/initialstep.gif)

**Visualização do aprovador do formulário**

![audiview](assets/approversview.gif)

Na visualização do aprovador, o aprovador não pode editar os dados enviados. Há também uma nova seção destinada apenas aos Aprovadores.

Para testar este fluxo de trabalho no seu sistema, siga as etapas mencionadas abaixo:
* [Baixe e implante DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Baixe e implante o pacote OSGI personalizado SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importar os ativos relacionados a este artigo para AEM](assets/helpxworkflow.zip)
* Abra o formulário [Solicitação de tempo limite](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie
* Abra a caixa de entrada [a1/>. ](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html) Você deve ver uma nova tarefa atribuída. Abra o formulário. Os dados do remetente devem ser somente leitura e uma nova seção do aprovador deve estar visível.
* Explore o [modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explore a etapa do processo. Esta é a etapa que define o valor da etapa inicial como Não.
