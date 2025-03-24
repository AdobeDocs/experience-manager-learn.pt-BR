---
title: Uso do sistema de estilos no AEM Forms
description: Definir a política de estilo para o componente de botão
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 52205a93-d03c-430c-a707-b351ab333939
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 3%

---

# Definir o estilo na política para o componente

* Faça logon na instância do AEM pronta para nuvem local e navegue até Ferramentas | Geral | Modelos | o nome do seu projeto.

* Selecione e abra o modelo **Em branco com Componentes principais** no modo de edição.
* Clique no ícone de política do componente Botão para abrir o editor de política.

* ![política-botão](assets/button-policy.png)

Defina a política conforme mostrado abaixo

![detalhes-da-política-de-botão](assets/styling-policy.png)

Definimos 2 variações de estilo chamadas Marketing e Corporativo. Essas variações estão associadas às classes CSS correspondentes.**Verifique se não há espaço antes e depois dos nomes de classe CSS**.
Salve as alterações.

| Estilo | Classe CSS |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button — marketing |
| Corporativo | cmp-adaptiveform-button — corporativo |

Essas classes CSS serão definidas no arquivo scss do componente (_button.scss).

## Próximas etapas

[Definir classes CSS](./create-variations.md)
