---
title: Solução de problemas de assinatura de vários documentos
description: Testar e solucionar problemas da solução
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Testar e solucionar problemas


## Visualizar o formulário de refinamento

O caso de uso é acionado quando o agente de atendimento ao cliente preenche e envia [formulário de refinanciamento](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

O fluxo de trabalho Assinar vários Forms recebe acionadores neste envio de formulário e o cliente recebe uma notificação por email com um link para iniciar o processo de preenchimento e assinatura do formulário.

## Preencher formulários no pacote

O cliente é apresentado para preencher e assinar o primeiro formulário no pacote. Ao assinar o formulário com êxito, o cliente pode navegar até o próximo formulário no pacote. Depois que todos os formulários forem preenchidos e assinados, o cliente será apresentado ao &quot;**TudoConcluído**&quot;.

## Solução de problemas

### A notificação por email não está sendo gerada

As notificações por email são enviadas pelo componente Enviar email no fluxo de trabalho Assinar vários formulários. Se qualquer uma das etapas deste workflow falhar, a notificação por e-mail será enviada. Verifique se a etapa de processo personalizada no fluxo de trabalho está criando linhas no banco de dados MySQL. Se as linhas estiverem sendo criadas, verifique as configurações do Day CQ Mail Service

### O link na notificação por email não está funcionando

Os links nas notificações por email são gerados dinamicamente. Se o servidor AEM não estiver em execução no localhost:4502, forneça o nome do servidor e a porta corretos nos argumentos da etapa Armazenar Forms para assinar do fluxo de trabalho Assinar vários Forms

### Não é possível assinar o formulário

Isso pode acontecer se o formulário não tiver sido preenchido corretamente ao buscar os dados da fonte de dados. Verifique os logs stdout do servidor. O fetchformdata.jsp escreve algumas mensagens úteis para o stdout.

### Não é possível navegar para o próximo formulário no pacote

Ao assinar com êxito um formulário no pacote, o fluxo de trabalho Atualizar status da assinatura é acionado. A primeira etapa do fluxo de trabalho atualiza o status da assinatura do formulário no banco de dados. Verifique se o status do formulário foi atualizado de 0 para 1.

### Não vendo o formulário AllDone

Quando não houver mais formulários para fazer logon no pacote, o formulário AllDone é apresentado ao usuário.Se você não estiver vendo o formulário AllDone, verifique o URL usado na linha 33 do arquivo GetNextFormToSign.js que faz parte do **getnextform** biblioteca do cliente.
