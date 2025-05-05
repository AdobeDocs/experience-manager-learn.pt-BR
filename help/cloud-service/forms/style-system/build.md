---
title: Uso do sistema de estilos no AEM Forms
description: Criar o projeto de tema
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 4a02f494-ca0e-42d4-bbb9-6223ff8685e3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Testar as alterações

Crie um formulário adaptável com base no modelo **&quot;Em branco com Componentes principais&quot;**. Arraste e solte três botões no formulário e rotule-os como &quot;Corporativo&quot;, &quot;Marketing&quot; e &quot;Padrão&quot;.
Atribua as variantes de estilo apropriadas aos botões Corporativo e Marketing, selecionando o pincel de pintura, conforme mostrado abaixo.

![estilos](assets/marketing-variation.png)

O terceiro botão terá o estilo padrão aplicado.

## Criar o projeto de tema

A próxima etapa é criar o projeto de tema. Navegue até a pasta raiz do projeto de tema e execute o comando _&#x200B;**npm run build**&#x200B;_, conforme mostrado na captura de tela abaixo.

![tema de compilação](assets/build-theme.png)

Depois que o projeto de tema for criado com sucesso, você estará pronto para testar as alterações.

## Maneira rápida e fácil de testar o css

* Abra o arquivo theme.css localizado na pasta dist do projeto do tema. Selecione e copie todo o conteúdo do arquivo.
* Visualize o formulário criado na etapa anterior.
* Clique com o botão direito em um dos botões e selecione Inspecionar para abrir o console do desenvolvedor.
* No console do desenvolvedor, clique em theme.css para abrir o theme.css
* Selecione e exclua todo o conteúdo de theme.css usando o CTR-A e pressione o botão Delete.
* Copie e cole o conteúdo de theme.css que você criou na etapa anterior.
* Os botões devem ser atualizados com os estilos apropriados, conforme mostrado abaixo.

![botões-finais](assets/final-state-buttons.png)

## Enviar as alterações

Se você estiver satisfeito com as alterações, poderá enviar as alterações para sua instância da nuvem usando o [pipeline de front-end](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/enable-frontend-pipeline-devops/create-frontend-pipeline)
