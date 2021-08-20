---
title: Uso de relatórios de transação no AEM Forms
description: Relatórios de transação no AEM Forms permitem que você mantenha uma contagem de todas as transações realizadas desde uma data especificada na implantação do AEM Forms.
feature: Formulários adaptáveis
version: 6.4.1,6.5
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---


# Uso de relatórios de transação no AEM Forms{#using-transaction-reporting-in-aem-forms}

O relatório de transação para capturar o número de Envio de formulário, a renderização de documentos usando serviços de documento e a renderização de comunicações interativas (canais Web e Impressão) foi introduzido no AEM Forms 6.4.1. Esse recurso destina-se principalmente aos clientes que desejam licenciar o software com base no número de envios de formulário e/ou documentos renderizados. Esse recurso está disponível atualmente apenas na pilha OSGI do AEM Forms.

## Ativação do Relatório de Transações {#enabling-transaction-reporting}

Por padrão, a gravação de transações é desativada. Para habilitar o registro de transações, siga as etapas mencionadas abaixo:

* [Abra o configMgr](http://localhost:4502/system/console/configMgr)
* Pesquise por &quot;Relatório de transação do Forms&quot;
* Marque a caixa de seleção &quot;Registrar Transações&quot;
* Salve as alterações

Quando o relatório de transações estiver ativado, você poderá enviar o Adaptive Forms, gerar documentos usando serviços de documento ou renderizar documentos do Interative Communication para ver o relatório de transações em ação.

## Exibindo Relatório de Transação {#viewing-transaction-report}

Para exibir o relatório de transação, faça logon no AEM Forms como administrador. Somente os membros do grupo fd-Administrator podem exibir o relatório de transação.

Selecionar ferramentas | Forms | Exibir Relatório de Transação

ou exiba o relatório de transação clicando [aqui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransactionReporting](assets/transactionreporting.gif)

Na captura de tela acima do Documento processado está o número de documentos gerados usando serviços de documento. Documentos renderizados é o número de documentos de comunicação interativa (Web e Impressão) renderizados. Forms Enviado é o número de Envio de formulário adaptável.

Uma transação permanece no buffer por um período especificado (Tempo do buffer de liberação + Tempo de replicação inversa). Por padrão, leva aproximadamente 90 segundos para a contagem de transações ser refletida no relatório de transações.

Ações como enviar um formulário PDF, usar a interface do usuário do agente para visualizar uma comunicação interativa ou usar métodos de envio de formulário não padrão não são contabilizadas como transações. A AEM Forms fornece uma API para registrar essas transações. Chame a API das implementações personalizadas para registrar uma transação.

Se você estiver exibindo o relatório de transação na instância do autor, verifique se a replicação inversa está configurada em todas as instâncias de publicação.

Para saber mais sobre o relatório de transações [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

