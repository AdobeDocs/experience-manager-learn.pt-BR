---
title: Testar a solução
description: Execute o Main.java para testar a solução
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---


# Importar projeto do Eclipse

Baixe e descompacte o [arquivo zip](./assets/aem-forms-doc-gen.zip)

Inicie o Eclipse e importe o projeto para o Eclipse
O projeto inclui os seguintes arquivos na pasta de recursos:

* DataFile1 e DataFile2 - Arquivos de dados xml de amostra a serem unidos ao modelo para gerar o arquivo PDF final
* address.xdp - modelo XDP
* service_token.json - Você terá que substituir o conteúdo deste arquivo por suas credenciais específicas da conta
* options.json - As opções especificadas nesse arquivo são usadas para definir as propriedades do arquivo PDF gerado pela API

![resources-file](./assets/resource-files.JPG)

## Testar a solução

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo DocumentGeneration.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados
* Abra Main.java. Defina o valor da variável postURL para apontar para sua instância.
* Execute o Main.java como aplicativo java

>[!NOTE]
> Na primeira vez que você executar o programa java, aparecerá o erro HTTP 403. Para superar isso, forneça as permissões [apropriadas ao usuário da conta técnica no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**O** usuário do AEM Forms é a função que usei para este curso.

