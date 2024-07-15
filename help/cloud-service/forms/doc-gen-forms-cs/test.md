---
title: Testar a solução
description: Execute o Main.java para testar a solução
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Importar projeto Eclipse

Baixe e descompacte o [arquivo zip](./assets/aem-forms-cs-doc-gen.zip)

Iniciar o Eclipse e importar o projeto para o Eclipse
O projeto inclui os seguintes arquivos na pasta de recursos:

* DataFile1, DataFile2 e DataFile3 - Arquivos de dados xml de amostra a serem mesclados com o modelo para gerar o arquivo de PDF final
* custom_fonts.xdp - modelo XDP.
* service_token.json - Será necessário substituir o conteúdo desse arquivo pelas credenciais específicas da conta
* options.json - As opções especificadas neste arquivo são usadas para definir as propriedades do arquivo de PDF gerado pela API

![arquivo-recursos](./assets/resource-files.png)

## Testar a solução

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo DocumentGeneration.java e especifique a pasta na qual deseja salvar os arquivos de PDF gerados
* Abra Main.java. Defina o valor da variável postURL para apontar para sua instância.
* Execute o aplicativo Main.java as java

>[!NOTE]
> Na primeira vez que você executa o programa java, receberá o erro HTTP 403. Para superar isso, forneça as [permissões apropriadas ao usuário técnico da conta no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Usuários do AEM Forms** é a função que usei neste curso.
