---
title: Fluxo de trabalho de solicitação de folga paga simples
description: Ocultar e mostrar painéis de formulário adaptáveis no fluxo de trabalho do AEM
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 592
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# Fluxo de trabalho de solicitação de folga paga simples

Neste artigo, analisamos um fluxo de trabalho simples usado para solicitar o Tempo de folga pago. Os requisitos de negócios são os seguintes:

* O usuário A solicita folga preenchendo um formulário adaptável.
* O formulário é roteado para o usuário administrador do AEM (na vida real, é roteado para o gerente do remetente)
* O administrador abre o formulário. O administrador não deve ser capaz de editar qualquer informação preenchida pelo remetente.
* A seção Aprovador deve estar visível para o aprovador (nesse caso, é o usuário administrador do AEM).

Para atender ao requisito acima, usamos um campo oculto chamado **initialstep** no formulário e seu valor padrão estiver definido como Sim.Quando o formulário for enviado, a primeira etapa do fluxo de trabalho definirá o valor de initialstep como Não. O formulário tem regras de negócios para ocultar e mostrar as seções apropriadas com base no valor da etapa inicial.

**Configurar o formulário para acionar o fluxo de trabalho do AEM**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**Apresentação do fluxo de trabalho**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**Visualização do emissor do formulário de Solicitação de folga**

![initialstep](assets/initialstep.gif)

**Visualização do aprovador do formulário**

![aproverview](assets/approversview.gif)

Na visualização do aprovador, o aprovador não consegue editar os dados enviados. Também há uma nova seção que se destina somente aos Aprovadores.

Para testar esse workflow em seu sistema, siga as etapas mencionadas abaixo:
* [Baixe e implante DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Baixe e implante o pacote OSGI personalizado SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importar os ativos relacionados a este artigo para o AEM](assets/helpxworkflow.zip)
* Abra o [Formulário de solicitação de folga](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie
* Abra o [caixa de entrada](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Você deverá ver uma nova tarefa atribuída. Abra o formulário. Os dados do remetente devem ser somente leitura e uma nova seção do aprovador deve estar visível.
* Explore o [modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explore a etapa do processo. Esta é a etapa que define o valor de initialstep como No.
