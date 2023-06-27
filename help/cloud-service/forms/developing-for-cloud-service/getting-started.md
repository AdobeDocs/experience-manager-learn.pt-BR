---
title: Instalar os pré-requisitos
description: Instalar o software necessário para configurar o ambiente de desenvolvimento
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# Instalar o software necessário

Este tutorial guiará você pelas etapas necessárias para criar um projeto do AEM Forms, sincronizar o projeto do AEM Forms com sua instância do AEM local usando o IntelliJ e a ferramenta de repositório. Você também aprenderá a adicionar seu projeto ao repositório Git local e enviar o repositório Git local para o repositório do cloud manager.





Este tutorial se referirá a esta estrutura de pastas no futuro.

* [Instalar JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). Baixei o jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)Por exemplo, se você tiver instalado o Maven na pasta c:\maven, será necessário criar uma variável de ambiente chamada M2_HOME com o valor C:\maven\apache-maven-3.6.0. Em seguida, adicione M2_HOME\bin ao caminho e salve sua configuração.

## Criar projeto Maven usando o arquétipo de projeto AEM

* Crie uma pasta chamada **cloudmanager**(você pode dar qualquer nome a ele) na unidade c
* Abra a janela do prompt de comando e navegue até **c:\cloudmanager**
* Copie e cole o conteúdo do [arquivo de texto](assets/creating-maven-project.txt) na janela do prompt de comando. Talvez seja necessário alterar DarchetypeVersion=30 dependendo da variável [versão mais recente](https://github.com/adobe/aem-project-archetype/releases). No momento em que este artigo foi escrito, a última versão tinha 30 anos.
* Execute o comando pressionando a tecla Enter. Se tudo correr corretamente, você verá a mensagem de sucesso da build.

## Próximas etapas

[Instalação do IntelliJ](./intellij-set-up.md)