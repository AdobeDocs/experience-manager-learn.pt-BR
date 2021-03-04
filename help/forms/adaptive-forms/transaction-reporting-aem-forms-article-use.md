---
title: Uso de relatórios de transação no AEM Forms
seo-title: Uso de relatórios de transação no AEM Forms
description: Relatórios de transação no AEM Forms permitem que você mantenha uma contagem de todas as transações realizadas desde uma data especificada na implantação do AEM Forms.
seo-description: Relatórios de transação no AEM Forms permitem que você mantenha uma contagem de todas as transações realizadas desde uma data especificada na implantação do AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: Formulários adaptáveis
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---


# Uso de relatórios de transação no AEM Forms{#using-transaction-reporting-in-aem-forms}

O relatório de transações para capturar o número de envios de Formulários, a renderização de documentos usando serviços de documentos e a renderização de comunicações interativas (canais Web e Impressão) foi introduzido no AEM Forms 6.4.1. Esse recurso destina-se principalmente aos clientes que desejam licenciar o software com base no número de envios de formulários e/ou documentos renderizados. Esse recurso está disponível atualmente apenas na pilha OSGI do AEM Forms.

## Ativando o Relatório de Transações {#enabling-transaction-reporting}

Por padrão, a gravação de transações é desativada. Para habilitar o registro de transações, siga as etapas mencionadas abaixo:

* [Abra o configMgr](http://localhost:4502/system/console/configMgr)
* Pesquise por &quot;Relatório de transação de formulários&quot;
* Marque a caixa de seleção &quot;Registrar Transações&quot;
* Salve as alterações

Quando o relatório de transações estiver ativado, você poderá enviar Formulários Adaptativos, gerar documentos usando serviços de documento ou renderizar documentos do Interative Communication para ver o relatório de transações em ação.

## Exibindo Relatório de Transação {#viewing-transaction-report}

Para exibir o relatório de transação, faça logon no AEM Forms como administrador. Somente os membros do grupo fd-Administrator podem exibir o relatório de transação.

Selecionar ferramentas | Formulários | Exibir Relatório de Transação

ou exiba o relatório de transação clicando [aqui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransactionReporting](assets/transactionreporting.gif)

Na captura de tela acima do Documento processado está o número de documentos gerados usando serviços de documento. Documentos renderizados é o número de documentos de comunicação interativa (Web e Impressão) renderizados. Formulários enviados é o número de envios de formulário adaptável.

Uma transação permanece no buffer por um período especificado (Tempo do buffer de liberação + Tempo de replicação inversa). Por padrão, leva aproximadamente 90 segundos para a contagem de transações ser refletida no relatório de transações.

Ações como enviar um formulário PDF, usar a interface do usuário do agente para visualizar uma comunicação interativa ou usar métodos de envio de formulário não padrão não são contabilizadas como transações. O AEM Forms fornece uma API para registrar essas transações. Chame a API das implementações personalizadas para registrar uma transação.

Se você estiver exibindo o relatório de transação na instância do autor, verifique se a replicação inversa está configurada em todas as instâncias de publicação.

Para saber mais sobre o relatório de transações [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

