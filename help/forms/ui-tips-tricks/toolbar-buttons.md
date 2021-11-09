---
title: Espaça os botões seguinte e anterior da barra de ferramentas
description: Espaçar os botões da barra de ferramentas
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
source-git-commit: 84a0c78f89f78e161b460574b5927fc4aba2fe3a
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Espaço do botão da barra de ferramentas

Quando os botões Next e Prev são adicionados à barra de ferramentas no AEM Forms, por padrão, os botões são colocados próximos um do outro. Talvez você queira pressionar o botão Next para a extremidade direita na barra de ferramentas, mantendo o botão prev/back à esquerda

![espaçamento entre barras de ferramentas](assets/toolbar-spacing.png)


## Estilo da barra de ferramentas

O caso de uso acima pode ser facilmente realizado usando o editor de estilo. Depois de adicionar o botão Anterior/Próximo à barra de ferramentas, verifique se você selecionou a camada Estilo no menu de edição. Com o modo de estilo selecionado, selecione a barra de ferramentas para abrir sua folha de propriedades de estilo. Expanda a seção Dimension e Posição e verifique se você está vendo todas as propriedades. Defina as seguintes propriedades
* Dimension e posição
   * Largura: 100%
   * Posição: relation

Salve as alterações

## Estilo do botão Avançar

Selecione o botão Next e certifique-se de abrir a folha de propriedades de estilo do botão next (não o texto do próximo botão). Defina as seguintes propriedades
* Dimension e posição
   * posição: 1 px Superior absoluto Direito 1px
* Borda
   * Raio da borda: 4px(Superior,Direita,Inferior,Esquerda)

Salve as alterações
