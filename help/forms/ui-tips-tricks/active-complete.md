---
title: Estilizar as guias de navegação à esquerda com ícones
description: Adicionar ícones para indicar guias ativas e concluídas
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
duration: 68
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 21%

---

# Adicionar ícones para indicar guias ativas e concluídas

Quando você tem um formulário adaptável com a navegação da guia esquerda, talvez queira exibir ícones para indicar o status da guia. Por exemplo, você deseja mostrar um ícone para indicar a guia ativa e o ícone para indicar a guia concluída, como mostrado na captura de tela abaixo.

![espaçamento da barra de ferramentas](assets/active-completed.png)

## Criação de um Formulário adaptável

Um formulário adaptável simples baseado no modelo Básico e no tema do Canvas 3.0 foi usado para criar o formulário de amostra.
Os [ícones usados neste artigo](assets/icons.zip) podem ser baixados aqui.


## Estilo do estado padrão

Abrir o formulário no modo de edição
Verifique se você está na camada de estilo e selecione qualquer guia (por exemplo, guia Geral).
Você está no estado padrão ao abrir o editor de estilos da guia, como mostrado na captura de tela abaixo
![guia de navegação](assets/navigation-tab.png)

Defina as propriedades de CSS para o estado padrão conforme mostrado abaixo

| Categoria | Nome de propriedade | Valor de propriedade |
|:---|:---|:---|
| Dimensões e Posição | Largura | 50px |
| Texto | Espessura da Fonte | Negrito |
| Texto | Cor | #FFF |
| Texto | Altura da Linha | 3 |
| Texto | Alinhamento do Texto | Esquerda |
| Fundo | Cor | #056dae |

Salve as alterações

## Estilo do estado ativo

Verifique se você está no estado Ativo e estilize as seguintes propriedades de CSS

| Categoria | Nome de propriedade | Valor de propriedade |
|:---|:---|:---|
| Dimensões e Posição | Largura | 50px |
| Texto | Espessura da Fonte | Negrito |
| Texto | Cor | #FFF |
| Texto | Altura da Linha | 3 |
| Texto | Alinhamento do Texto | Esquerda |
| Fundo | Cor | #056dae |

Estilize a imagem de plano de fundo conforme mostrado na captura de tela abaixo

Salve as alterações.



![estado ativo](assets/active-state.png)

## Estilo do estado visitado

Verifique se você está no estado visitado e estilize as seguintes propriedades

| Categoria | Nome de propriedade | Valor de propriedade |
|:---|:---|:---|
| Dimensões e Posição | Largura | 50px |
| Texto | Espessura da Fonte | Negrito |
| Texto | Cor | #FFF |
| Texto | Altura da Linha | 3 |
| Texto | Alinhamento do Texto | Esquerda |
| Fundo | Cor | #056dae |

Estilize a imagem de plano de fundo conforme mostrado na captura de tela abaixo


![estado visitado](assets/visited-state.png)

Salve as alterações

Pré-visualize o formulário e teste se os ícones estão funcionando como esperado.
