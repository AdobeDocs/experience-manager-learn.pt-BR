---
title: Usando o Relatórios de transação no AEM Forms
seo-title: Usando o Relatórios de transação no AEM Forms
description: Os relatórios de transação no AEM Forms permitem que você mantenha uma contagem de todas as transações realizadas desde uma data especificada na implantação do AEM Forms.
seo-description: Os relatórios de transação no AEM Forms permitem que você mantenha uma contagem de todas as transações realizadas desde uma data especificada na implantação do AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: adaptive-forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---


# Usando o Relatórios de transação no AEM Forms{#using-transaction-reporting-in-aem-forms}

O relatórios de transação para capturar o número de envios de Formulário, a renderização de documentos usando serviços de documento e a renderização de comunicações interativas (canais de Web e Impressão) foi introduzido com o AEM Forms 6.4.1. Esse recurso destina-se principalmente aos clientes que desejam licenciar o software com base no número de envios de formulário e/ou documentos renderizados. Este recurso está disponível apenas na pilha OSGI AEM Forms.

## Ativando o Relatórios de Transação {#enabling-transaction-reporting}

Por padrão, a gravação de transações é desativada. Para ativar a gravação de transações, siga as etapas mencionadas abaixo:

* [Abrir o configMgr](http://localhost:4502/system/console/configMgr)
* Pesquisar por &quot;Relatórios de transação Forms&quot;
* Marque a caixa de seleção &quot;Registrar transações&quot;
* Salvar suas alterações

Quando o relatórios de transação estiver ativado, você poderá enviar a Adaptive Forms, gerar documentos usando os serviços de documento ou renderizar documentos de Comunicação Interativa para ver o relatórios de transação em ação.

## Exibindo Relatório de Transação {#viewing-transaction-report}

Para visualização do relatório de transação, faça logon no AEM Forms como administrador. Somente os membros do grupo fd-Administrator podem visualização no relatório de transação.

Selecionar ferramentas | Forms | Relatório de transação de Visualização

ou visualização o relatório de transação clicando [aqui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

Na captura de tela acima do Documento Processado está o número de documentos gerados pelos serviços de documento. Documentos renderizados é o número de documentos de comunicação interativa (Web e Impressão) renderizados. O Forms enviado é o número de envios de formulário adaptável.

Uma transação permanece no buffer por um período especificado (tempo de buffer de liberação + tempo de replicação inversa). Por padrão, leva aproximadamente 90 segundos para a contagem de transações refletir no relatório de transações.

Ações como enviar um formulário PDF, usar a interface do usuário do agente para pré-visualização de uma comunicação interativa ou usar métodos de envio de formulário não padrão não são contabilizadas como transações. A AEM Forms fornece uma API para registrar tais transações. Chame a API de suas implementações personalizadas para registrar uma transação.

Se você estiver exibindo o relatório de transação na instância do autor, verifique se a replicação reversa está configurada em todas as instâncias de publicação.

Para saber mais sobre o relatórios de transação, [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

