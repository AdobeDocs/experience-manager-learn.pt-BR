---
title: Extensibilidade da interface do AEM
description: Saiba mais sobre a extensibilidade da interface do AEM usando o App Builder para criar extensões.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
thumbnail: null
last-substantial-update: 2023-06-02T00:00:00Z
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---


# Extensibilidade da interface do AEM

O Adobe Experience Manager (AEM) oferece uma interface de usuário (UI) avançada para criar experiências digitais. Para personalizar e estender a interface do usuário, o Adobe apresentou o Construtor de aplicativos. Essa ferramenta permite que os desenvolvedores aprimorem a experiência do usuário sem codificação complexa usando JavaScript e React.

O Construtor de aplicativos fornece uma camada de implementação para criar extensões vinculadas a pontos de extensão bem definidos no AEM. O App Builder se integra perfeitamente ao AEM, permitindo pré-visualização e testes em tempo real. A implantação de alterações no AEM é rápida e simplificada. Ao usar o App Builder, os desenvolvedores economizam tempo e esforço, permitindo a prototipagem rápida e a colaboração com as partes interessadas.

## Desenvolver uma extensão da interface do usuário para AEM

AEM, várias interfaces têm pontos de extensão diferentes, no entanto, os conceitos básicos são os mesmos.

Os vídeos e apresentações fornecidos vinculados abaixo mostram o uso de uma extensão do Console do fragmento de conteúdo para ilustrar várias atividades. No entanto, é importante observar que os conceitos abordados podem ser aplicados a todas as extensões da interface do usuário do AEM.

1. [Criar um projeto do Adobe Developer Console](./adobe-developer-console-project.md)
1. [Inicializar uma extensão](./app-initialization.md)
1. [Registrar uma extensão](./extension-registration.md)
1. Implementar um ponto de extensão

   Os pontos de extensão e suas implementações variam com base na interface que está sendo estendida.

   + [Desenvolver uma extensão da interface dos fragmentos de conteúdo](./content-fragments/overview.md)

1. [Desenvolver um modal](./modal.md)
1. [Desenvolver uma ação do Adobe I/O Runtime](./runtime-action.md)
1. [Verificar uma extensão](./verify.md)
1. [Implantar uma extensão](./deploy.md)

## Documentação do Adobe Developer

O Adobe Developer contém detalhes do desenvolvedor sobre a extensibilidade da interface do AEM. Revise o [Conteúdo do Adobe Developer para obter mais detalhes técnicos](https://developer.adobe.com/uix/docs/).