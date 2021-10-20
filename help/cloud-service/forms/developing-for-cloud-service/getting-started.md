---
title: Instalação dos pré-requisitos
description: Instale o software necessário para configurar seu ambiente de desenvolvimento
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 2%

---


# Instalação do software necessário

Este tutorial o guiará pelas etapas necessárias para criar um projeto do AEM Forms, sincronizar o projeto do AEM Forms com sua instância de AEM local usando a ferramenta IntelliJ e repo. Você também aprenderá a adicionar seu projeto ao repositório Git local e enviar o repositório Git local para o repositório do cloud manager.

Crie a seguinte estrutura de pastas na unidade c
**c:\cloudmanager\adoberepo**

Este tutorial se referirá a essa estrutura de pastas a partir de agora.

* [Instalar o JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). Baixei o jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)Por exemplo, se tiver instalado o Maven em c:\maven folder, será necessário criar uma variável de ambiente chamada M2_HOME com o valor C:\maven\apache-maven-3.6.0. Em seguida, adicione M2_HOME\bin ao caminho e salve sua configuração

## Criar projeto Maven usando AEM Arquétipo de projeto

* Crie uma pasta chamada **cloudmanager**(você pode lhe dar qualquer nome) na unidade c
* Abra a janela do prompt de comando e navegue até **c:\cloudmanager**
* Copie e cole o conteúdo do arquivo de texto (assets/creating-maven-project.txt) na janela da tela de comandos. Talvez seja necessário alterar DarchetypeVersion=30 dependendo do [versão mais recente](https://github.com/adobe/aem-project-archetype/releases). A versão mais recente era 30 no momento da redação deste artigo.

* Execute o comando pressionando Enter key.  Se tudo correr corretamente, você deverá ver a mensagem de sucesso de criação




