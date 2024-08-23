---
title: Criação de componente de imagem clicável
description: Criação do componente de imagem clicável no AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introdução

O uso de imagens clicáveis no Forms pode criar uma experiência do usuário mais envolvente, intuitiva e visualmente atraente. Para os fins deste artigo, usamos o SVG para imagens clicáveis, pois ele oferece várias vantagens, especialmente em termos de flexibilidade de design, desempenho e experiência do usuário.
O SVG pode ser criado usando o Adobe Illustrator ou qualquer uma das ferramentas online gratuitas. Usei o [Mapa dos EUA de](https://simplemaps.com/resources/svg-us)simpleMaps para demonstrar o caso de uso.

## Caso de uso para usar o mapa clicável dos EUA

O mapa clicável dos EUA permite que os usuários explorem envios de formulários específicos do estado. Quando um usuário clica em um estado, os envios desse estado são listados, com a opção de abrir um envio específico.
