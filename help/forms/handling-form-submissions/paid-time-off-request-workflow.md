---
title: Fluxo de trabalho de solicitação de tempo de espera paga simples
description: Ocultar e mostrar painéis de formulário adaptável AEM fluxo de trabalho
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Fluxo de trabalho de solicitação de tempo de espera paga simples

Neste artigo, observamos um fluxo de trabalho simples usado para solicitar Tempo de espera pago. Os requisitos de negócios são os seguintes:

* O usuário A solicita tempo de desativação preenchendo um formulário adaptável.
* O formulário é roteado para AEM usuário administrador (na vida real, é roteado para o gerente do remetente)
* O administrador abre o formulário. O administrador não deve poder editar nenhuma informação preenchida pelo remetente.
* A seção Aprovador deve estar visível para o aprovador (nesse caso, é o usuário administrador AEM).

Para realizar o requisito acima, usamos um campo oculto chamado **initialstep** no formulário e seu valor padrão é definido como Sim.Quando o formulário é enviado, a primeira etapa no fluxo de trabalho define o valor da etapa inicial como Não. O formulário tem regras de negócios para ocultar e mostrar as seções apropriadas com base no valor da etapa inicial.

**Configurar formulário para acionar AEM fluxo de trabalho**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Passeio pelo fluxo de trabalho**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Exibição do formulário de solicitação de tempo de espera pelo remetente**

![initialstep](assets/initialstep.gif)

**Visualização do aprovador do formulário**

![aproverview](assets/approversview.gif)

Na visualização do aprovador, o aprovador não pode editar os dados enviados. Também há uma nova seção destinada apenas aos Aprovadores.

Para testar esse fluxo de trabalho em seu sistema, siga as etapas mencionadas abaixo:
* [Baixe e implante o DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Baixe e implante o pacote OSGI personalizado SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importar os ativos relacionados a este artigo para o AEM](assets/helpxworkflow.zip)
* Abra o [Formulário de solicitação de tempo desligado](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie
* Abra o [caixa de entrada](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Você deve ver uma nova tarefa atribuída. Abra o formulário. Os dados do remetente devem ser somente leitura e uma nova seção do aprovador deve estar visível.
* Explore o [modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explore a etapa do processo. Esta é a etapa que define o valor de initialstep como No.
