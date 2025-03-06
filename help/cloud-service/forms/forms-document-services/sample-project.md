---
title: Projeto de exemplo
description: Projeto de amostra que pode ser importado e executado em seu ambiente
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
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

