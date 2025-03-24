---
title: Instalação do IntelliJ, edição da comunidade
description: Instalar e importar o projeto do AEM para o IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# Instalação do IntelliJ

Instale a [edição da comunidade IntelliJ](https://www.jetbrains.com/idea/download/#section=windows). Você pode aceitar as configurações padrão enquanto sugerido durante a instalação.

## Importar o projeto do AEM

* Iniciar IntelliJ
* Importe o projeto do AEM criado na etapa anterior. Depois que o projeto for importado, sua tela deverá ficar parecida com este ![aem-banking-app](assets/aem-banking-app.png). Normalmente, você trabalhará com subprojetos core,ui.apps,ui.config e ui.content.
* Se você não vir a janela do Maven e do terminal, vá para Exibir -> Janela de ferramentas e selecione Maven e Terminal

## Adicionar o módulo de fontes

Se você quiser usar fontes personalizadas no arquivo do PDF, será necessário enviar as fontes personalizadas para a instância do AEM Forms CS. Siga as etapas a seguir

* Crie uma pasta chamada **fontes** em C:\CloudManager\aem-banking-application
* Extrair o conteúdo de [font.zip](assets/fonts.zip) para a pasta de fontes recém-criada
* Estão incluídas no módulo de fontes algumas fontes personalizadas.Você pode adicionar as fontes personalizadas de sua organização à pasta C:\CloudManager\aem-banking-application\fonts\src\main\resources do módulo de fontes
* Abra o arquivo C:\CloudManager\aem-banking-application\pom.xml
* Adicione a seguinte linha ```<module>fonts</module>``` na seção de módulos do pom.xml
* Salve seu pom.xml
* Atualizar o projeto aem-banking-application no IntelliJ

Estrutura de projeto com módulo de fontes
![módulo-fontes](assets/fonts-module.png)

Módulo Fonts incluído no POM de projetos
![fonts-pom](assets/fonts-module-pom.png)

## Próximas etapas

[Configurar Git](./setup-git.md)
