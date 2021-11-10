---
title: Navegar pelos painéis aninhados
description: Navegar pelos painéis aninhados
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
source-git-commit: 20cae7a327131927f831ae9c49fb5eebfb00f5c4
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Guias Navegação com vários painéis

Quando o formulário tiver guias de navegação esquerdas e uma das guias tiver vários painéis, talvez você queira ocultar o título dos painéis filhos e ainda conseguir navegar entre as guias e os painéis filhos dessa guia

[Esse recurso está ativo neste formulário](https://forms.enablementadobe.com/content/forms/af/testnav1.html)




## Criar formulário adaptável

Crie um formulário adaptável com a seguinte estrutura. O painel raiz tem painéis filhos que são exibidos como guias à esquerda. Alguns deles &quot;**guias**&quot; tem painéis filhos adicionais. Por exemplo, a guia Família tem dois painéis filhos chamados Cônjuge e Filhos.

Uma barra de ferramentas também é adicionada em FormContainer com os botões Anterior e Próximo

![espaçamento entre barras de ferramentas](assets/multiple-panels.png)



O comportamento padrão desse formulário seria exibir todos os painéis à esquerda e, em seguida, navegar de uma guia para outra ao clicar no botão seguinte.

Para alterar esse comportamento padrão, precisamos fazer o seguinte

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


Adicione o seguinte código ao evento click da variável **Próximo** botão usando o editor de código

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Adicione o seguinte código ao evento click da variável **Anterior** botão usando o editor de código

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

O código acima ajudará você a navegar entre as guias e os painéis filhos de cada guia.

## Ocultar o título dos painéis filhos

Use o editor de estilos para ocultar o título dos painéis filho de guias.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
>
>O recurso descrito neste artigo não funciona na última guia. Por exemplo, se a guia Endereço tivesse painéis filhos, essa funcionalidade não funcionaria.
