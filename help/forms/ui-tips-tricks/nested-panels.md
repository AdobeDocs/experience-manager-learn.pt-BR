---
title: Navegar em painéis aninhados
description: Navegar em painéis aninhados
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 287
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# Guias de navegação com vários painéis

Quando o formulário tem guias de navegação à esquerda e se uma das guias tiver vários painéis, talvez você queira ocultar o título dos painéis secundários e ainda poder navegar entre as guias e os painéis secundários dessas guias

## Criar formulário adaptável

Crie um formulário adaptável com a seguinte estrutura. O painel raiz tem painéis secundários que são exibidos como guias à esquerda. Alguns desses &quot;**guias**&quot; têm painéis secundários adicionais. Por exemplo, a guia Família tem dois painéis filhos chamados Cônjuge e Filhos.

Uma barra de ferramentas também é adicionada abaixo de FormContainer com os botões Anterior e Próximo

![espaçamento da barra de ferramentas](assets/multiple-panels.png)



O comportamento padrão desse formulário seria exibir todos os painéis à esquerda e, em seguida, navegar de uma guia para outra ao clicar no botão próximo.

Para alterar esse comportamento padrão, é necessário fazer o seguinte

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


Adicione o seguinte código ao evento de clique do **Próxima** botão usando o editor de código

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Adicione o seguinte código ao evento de clique do **Anterior** botão usando o editor de código

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

O código acima ajudará você a navegar entre as guias e os painéis secundários de cada guia.

## Ocultar o título dos painéis secundários

Use o editor de estilos para ocultar o título dos painéis secundários das guias.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>O recurso descrito neste artigo não funciona na última guia. Por exemplo, se a guia Endereço tivesse painéis secundários, essa funcionalidade não funcionaria.
