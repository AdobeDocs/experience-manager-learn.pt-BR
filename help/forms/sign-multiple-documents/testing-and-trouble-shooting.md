---
title: Solução de problemas de assinatura de vários Documentos
description: Testar e solucionar problemas da solução
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Teste e solução de problemas


## Pré-visualização do formulário de refinanciamento

O caso de uso é acionado quando o agente de serviço ao cliente preenche e envia [formulário de refinanciamento](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

O fluxo de trabalho Assinar vários Forms recebe acionadores no envio do formulário e o cliente recebe uma notificação por email com um link para start do processo de preenchimento e assinatura do formulário.

## Preencher formulários no pacote

O cliente é apresentado para preencher e assinar o primeiro formulário no pacote. Ao assinar com êxito o formulário, o cliente pode navegar até o próximo formulário no pacote. Depois que todos os formulários forem preenchidos e assinados, o cliente receberá o formulário &quot;**AllDone**&quot;.

## Solução de problemas

### A notificação por email não está sendo gerada

A notificação por email é enviada pelo componente Enviar email no fluxo de trabalho Assinar vários formulários. Se alguma das etapas deste fluxo de trabalho falhar, a notificação por email será enviada. Certifique-se de que a etapa do processo personalizado no fluxo de trabalho esteja criando linhas no banco de dados MySQL. Se as linhas estiverem sendo criadas, verifique as configurações do Serviço de e-mail do Day CQ

### O link na notificação por email não está funcionando

Os links nas notificações por email são gerados dinamicamente. Se o servidor AEM não estiver sendo executado em localhost:4502, forneça o nome e a porta corretos do servidor nos argumentos da etapa Loja Forms To Sign do fluxo de trabalho Assinar vários Forms

### Não é possível assinar o formulário

Isso pode ocorrer se o formulário não for preenchido corretamente, buscando os dados da fonte de dados. Verifique os registros stdout do seu servidor. O fetchformdata.jsp grava algumas mensagens úteis no stdout.

### Não é possível navegar para o próximo formulário no pacote

Ao assinar com êxito um formulário no pacote, o fluxo de trabalho Atualizar status de assinatura é acionado. A primeira etapa do fluxo de trabalho atualiza o status de assinatura do formulário no banco de dados. Verifique se o status do formulário foi atualizado de 0 para 1.

### Não vendo o formulário AllDone

Quando não houver mais formulários para fazer logon no pacote, o formulário AllDone será apresentado ao usuário.Se você não estiver vendo o formulário AllDone, verifique o URL usado na linha 33 do arquivo GetNextFormToSign.js que faz parte da biblioteca de clientes **getnextform**.











