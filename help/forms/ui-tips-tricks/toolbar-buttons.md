---
title: Espaçar os botões anterior e seguinte da barra de ferramentas
description: Espaçar os botões da barra de ferramentas
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 39
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# Espaço no botão da barra de ferramentas

Ao adicionar os botões Próximo e Anterior à barra de ferramentas no AEM Forms, os botões por padrão são colocados próximos um do outro. Você pode pressionar o botão Avançar para a extrema direita na barra de ferramentas enquanto mantém o botão Voltar/Anterior à esquerda

![espaçamento da barra de ferramentas](assets/toolbar-spacing.png)


## Estilo da barra de ferramentas

O caso de uso acima pode ser facilmente realizado usando o editor de estilos. Depois de adicionar o botão Anterior/Próximo à barra de ferramentas, verifique se você selecionou a camada Estilo no menu de edição. Com o modo de estilo selecionado, selecione a barra de ferramentas para abrir sua folha de propriedades de estilo. Expanda a seção Dimensões e Posição e verifique se todas as propriedades estão sendo exibidas. Definir as seguintes propriedades
* Dimensões e Posição
   * Largura: 100%
   * Posição: relativa

Salve as alterações

## Estilo do botão Próximo

Selecione o botão Avançar e abra a folha de propriedades de estilo do botão Avançar (não o texto do botão Avançar). Definir as seguintes propriedades
* Dimensões e Posição
   * posição: 1 px superior absoluto à direita 1 px
* Borda
   * Raio da borda: 4px(superior,direita,inferior,esquerda)

Salve as alterações
