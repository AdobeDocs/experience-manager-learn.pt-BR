---
title: Utilização do relatório de transações no AEM Forms
description: Os relatórios de transações no AEM Forms permitem manter uma contagem de todas as transações ocorridas desde uma data especificada na implantação do AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 1%

---

# Utilização do relatório de transações no AEM Forms{#using-transaction-reporting-in-aem-forms}

O relatório de transações para capturar o número de envios de formulários, a renderização de documentos usando serviços de documento e a renderização de comunicações interativas (canais da Web e de impressão) foi introduzido com o AEM Forms 6.4.1. Esse recurso destina-se principalmente aos clientes que desejam licenciar o software com base no número de envios de formulários e/ou documentos renderizados. No momento, esse recurso está disponível somente na pilha OSGI do AEM Forms.

## Ativação do Relatório de Transações {#enabling-transaction-reporting}

Por padrão, a gravação de transação está desativada. Para habilitar a gravação de transação, siga as etapas mencionadas abaixo:

* [Abra o configMgr](http://localhost:4502/system/console/configMgr)
* Procure por &quot;Relatórios de transações do Forms&quot;
* Marque a caixa de seleção &quot;Registrar transações&quot;
* Salve as alterações

Quando o relatório de transação estiver ativado, você poderá enviar o Adaptive Forms, gerar documentos usando serviços de documento ou renderizar documentos da Comunicação interativa para ver o relatório de transação em ação.

## Exibindo Relatório de Transação {#viewing-transaction-report}

Para exibir o relatório de transações, faça logon no AEM Forms como administrador. Somente os membros do grupo fd-Administrator podem exibir o relatório de transações.

Selecionar ferramentas | FORMS | Visualizar relatório de transações

ou exiba o relatório de transações clicando em [aqui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransactionReporting](assets/transactionreporting.gif)

Na captura de tela acima de Documento processado é o número de documentos gerados usando os serviços de documento. Documentos renderizados é o número de documentos de Comunicação interativa (Web e Impressão) renderizados. Forms Enviados é o número de Envios de Formulários adaptáveis.

Uma transação permanece no buffer por um período especificado (Tempo do Buffer de Liberação + Tempo de replicação reversa). Por padrão, leva aproximadamente 90 segundos para a contagem de transações ser refletida no relatório de transações.

Ações como enviar um Formulário PDF, usar a interface do usuário do agente para visualizar uma comunicação interativa ou usar métodos de envio de formulário não padrão não são contabilizadas como transações. O AEM Forms fornece uma API para registrar essas transações. Chame a API a partir das implementações personalizadas para registrar uma transação.

Se você estiver visualizando o relatório de transações na instância do autor, verifique se a replicação inversa está configurada em todas as instâncias de publicação.

Para saber mais sobre relatórios de transação [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
