---
title: Testar a solução
description: Execute o Main.java para testar a solução
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Importar projeto Eclipse

Baixe e descompacte o [arquivo zip](./assets/aem-forms-cs-doc-gen.zip)

Iniciar o Eclipse e importar o projeto para o Eclipse
O projeto inclui os seguintes arquivos na pasta de recursos:

* DataFile1, DataFile2 e DataFile3 - Arquivos de dados xml de amostra a serem mesclados com o modelo para gerar o arquivo PDF final
* custom_fonts.xdp - modelo XDP.
* service_token.json - Será necessário substituir o conteúdo desse arquivo pelas credenciais específicas da conta
* options.json - As opções especificadas neste arquivo são usadas para definir as propriedades do arquivo do PDF gerado pela API

![arquivo-recursos](./assets/resource-files.png)

## Testar a solução

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo DocumentGeneration.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados
* Abra Main.java. Defina o valor da variável postURL para apontar para sua instância.
* Execute o aplicativo Main.java as java

>[!NOTE]
> Na primeira vez que você executa o programa java, receberá o erro HTTP 403. Para passar por isso, certifique-se de fornecer as [permissões apropriadas ao usuário da conta técnica no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=pt-BR#configure-access-in-aem).

**Usuários do AEM Forms** é a função que usei neste curso.
