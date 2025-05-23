---
title: Navegar em painéis aninhados
description: Navegar em painéis aninhados
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 264
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# Guias de navegação com vários painéis

Quando o formulário tem guias de navegação à esquerda e se uma das guias tiver vários painéis, talvez você queira ocultar o título dos painéis secundários e ainda poder navegar entre as guias e os painéis secundários dessas guias

## Criar formulário adaptável

Crie um formulário adaptável com a seguinte estrutura. O painel raiz tem painéis secundários que são exibidos como guias à esquerda. Algumas dessas &quot;**guias**&quot; têm painéis secundários adicionais. Por exemplo, a guia Família tem dois painéis filhos chamados Cônjuge e Filhos.

Uma barra de ferramentas também é adicionada abaixo de FormContainer com os botões Anterior e Próximo

![espaçamento da barra de ferramentas](assets/multiple-panels.png)



O comportamento padrão desse formulário seria exibir todos os painéis à esquerda e, em seguida, navegar de uma guia para outra ao clicar no botão próximo.

Para alterar esse comportamento padrão, é necessário fazer o seguinte

>[!VIDEO](https://video.tv.adobe.com/v/3438636?quality=12&learn=on&captions=por_br)


Adicione o seguinte código ao evento de clique do botão **Próximo** usando o editor de código

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Adicione o seguinte código ao evento de clique do botão **Anterior** usando o editor de código

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

O código acima ajudará você a navegar entre as guias e os painéis secundários de cada guia.

## Ocultar o título dos painéis secundários

Use o editor de estilos para ocultar o título dos painéis secundários das guias.

>[!VIDEO](https://video.tv.adobe.com/v/3439132?quality=12&learn=on&captions=por_br)

>[!NOTE]
>
>O recurso descrito neste artigo não funciona na última guia. Por exemplo, se a guia Endereço tivesse painéis secundários, essa funcionalidade não funcionaria.
