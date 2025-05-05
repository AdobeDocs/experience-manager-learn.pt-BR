---
title: Projeto de exemplo
description: Projeto de amostra que pode ser importado e executado em seu ambiente
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: f1fcc4bb-cc31-45e8-b7bb-688ef6a236bb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Testar em seu ambiente local

* Importar o projeto

   * Baixe e extraia o [projeto de amostra](./assets/formsdocumentservices.zip)
   * Abra seu **Ambiente de desenvolvimento Java** preferido (IntelliJ IDEA, Eclipse ou Código VS) e importe o projeto como um projeto Maven
* Configurar credenciais

   * Localizar o arquivo `resources/credentials/server_credentials.json`
   * Abra-o e **atualize as credenciais** específicas do seu ambiente.
   * Certifique-se de que inclua valores válidos para:

     `clientId`, `clientSecret`,`adobeIMSV3TokenEndpointURL` e
     `scopes`

* Executar a classe principal

   * Navegue até `src/main/java/Main.java` e execute o método principal

* Verificar execução
   * Verifique a saída na janela do terminal
