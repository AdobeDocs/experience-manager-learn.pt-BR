---
title: Solução de problemas de assinatura de vários documentos
description: Testar e solucionar problemas da solução
feature: Adaptive Forms
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 1%

---

# Teste e solução de problemas


## Visualizar o formulário de refinanciamento

O caso de uso é acionado quando o agente de serviço do cliente preenche e envia [formulário de refinanciamento](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

O fluxo de trabalho Assinar várias Forms recebe acionadores no envio desse formulário e o cliente recebe uma notificação por email com um link para iniciar o processo de preenchimento e assinatura do formulário.

## Preencha os formulários no pacote

O cliente é apresentado para preencher e assinar o primeiro formulário no pacote. Ao assinar com êxito o formulário, o cliente pode navegar até o próximo formulário do pacote. Depois que todos os formulários forem preenchidos e assinados, o cliente será apresentado com o &quot;**Concluído**&quot;.

## Solução de problemas

### A notificação por email não está sendo gerada

A notificação por email é enviada pelo componente Enviar email no fluxo de trabalho Assinar vários formulários . Se alguma das etapas nesse workflow falhar, a notificação por email será enviada. Certifique-se de que a etapa do processo personalizado no fluxo de trabalho esteja criando linhas no banco de dados MySQL. Se as linhas estiverem sendo criadas, verifique as configurações do Day CQ Mail Service

### O link na notificação por email não está funcionando

Os links nas notificações por email são gerados dinamicamente. Se o servidor de AEM não estiver sendo executado no localhost:4502, forneça o nome e a porta corretos do servidor nos argumentos da etapa Armazenar Forms para assinar do fluxo de trabalho Assinar vários Forms

### Não é possível assinar o formulário

Isso pode acontecer se o formulário não tiver sido preenchido corretamente, buscando os dados da fonte de dados. Verifique os logs de preparo do seu servidor. O fetchformdata.jsp grava algumas mensagens úteis no stdout.

### Não é possível navegar até o próximo formulário no pacote

Ao assinar com êxito um formulário no pacote, o fluxo de trabalho Atualizar status da assinatura é acionado. A primeira etapa no fluxo de trabalho atualiza o status da assinatura do formulário no banco de dados. Verifique se o status do formulário foi atualizado de 0 para 1.

### Não ver o formulário AllComplete

Quando não houver mais formulários para fazer logon no pacote, o formulário AllComplete será apresentado ao usuário.Se você não estiver visualizando o formulário AllComplete , verifique o URL usado na linha 33 do arquivo GetNextFormToSign.js que faz parte do **getnextform** biblioteca do cliente.
