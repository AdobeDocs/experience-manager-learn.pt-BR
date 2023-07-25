---
title: Armazenar dados do formulário adaptável no armazenamento do Azure
description: Criar e configurar o Formulário adaptável para armazenar dados no Armazenamento do Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 2%

---

# Tudo junto na prática

Agora temos todas as configurações/integrações necessárias para o caso de uso. A última etapa é criar um formulário adaptável com base no modelo de dados de formulário apoiado pelo Armazenamento do Azure.

Crie um formulário adaptável e verifique se ele se baseia no modelo de formulário adaptável correto. Isso garantirá que nosso código personalizado associado ao modelo seja executado sempre que um formulário adaptável for renderizado.

## Exemplo de formulário adaptável

No formulário, adicionamos dois campos ocultos

* ID do blob - Este campo é preenchido com uma GUID quando o campo é inicializado. O valor deste campo é usado como blobid para identificar exclusivamente o armazenamento de blob dos dados de formulário. Este blobid é usado para identificar os dados de formulário.
* ID do Blob Retornado - Este campo é preenchido com o resultado da chamada de serviço para armazenar dados no Armazenamento do Azure. Esse valor será o mesmo que a ID de blob da etapa anterior.

O formulário tem as seguintes regras de negócio

* O botão Salvar formulário é exibido quando o usuário insere o endereço de email. Com o clique no botão Salvar formulário, os dados do formulário são armazenados no Armazenamento do Azure usando a operação de serviço de chamada do modelo de dados de formulário.
* A BlobID retornada pelo serviço de chamada é armazenada no campo ID de blob. Quando esse valor é alterado, um email é enviado ao candidato usando o SendGrid. O e-mail conterá o link para abrir o formulário parcialmente preenchido identificado pela ID do blob.
* Um texto de confirmação é mostrado ao usuário quando os dados são armazenados com êxito no Armazenamento do Azure

### Próximas etapas

[Implantar os ativos de amostra](./deploy-sample-assets.md)

