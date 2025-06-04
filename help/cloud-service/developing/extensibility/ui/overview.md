---
title: Extensibilidade da interface do AEM
description: Saiba mais sobre a extensibilidade da interface do AEM, usando o App Builder para criar extensões.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 50
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '275'
ht-degree: 100%

---

# Extensibilidade da interface do AEM {#aem-ui-extensibility}

O Adobe Experience Manager (AEM) oferece uma interface do usuário (IU) avançada para criar experiências digitais. Para personalizar e estender a IU, a Adobe lançou o App Builder. Essa ferramenta permite que os desenvolvedores aprimorem a experiência do usuário sem códigos complexos, usando o JavaScript e o React.

O App Builder fornece uma camada de implementação para criar extensões vinculadas a pontos de extensão bem definidos no AEM. O App Builder integra-se perfeitamente ao AEM, permitindo visualização e testes em tempo real. A implantação de alterações no AEM é rápida e simplificada. Ao usar o App Builder, os desenvolvedores economizam tempo e esforço, permitindo uma prototipagem rápida e colaboração com as partes interessadas.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Introdução ao Adobe Developer App Builder e AEM Headless"
>abstract="Saiba como o AEM App Builder permite que os desenvolvedores personalizem e estendam rapidamente as interfaces do AEM com o JavaScript e o React, oferecendo suporte a uma integração perfeita e uma implantação rápida."

## Desenvolver uma extensão da IU do AEM

As várias IUs do AEM têm pontos de extensão diferentes, mas os conceitos básicos são os mesmos.

Os vídeos e tutoriais fornecidos abaixo mostram o uso de uma extensão do console de fragmentos de conteúdo para ilustrar várias atividades. No entanto, é importante observar que os conceitos abordados podem ser aplicados a todas as extensões da IU do AEM.

1. [Criar um projeto do Adobe Developer Console](./adobe-developer-console-project.md)
1. [Inicializar uma extensão](./app-initialization.md)
1. [Registrar uma extensão](./extension-registration.md)
1. Implementar um ponto de extensão

   Os pontos de extensão e suas implementações variam com base na IU que está sendo estendida.

   + [Desenvolver uma extensão da IU dos fragmentos de conteúdo](./content-fragments/overview.md)

1. [Desenvolver um modal](./modal.md)
1. [Desenvolver uma ação do Adobe I/O Runtime](./runtime-action.md)
1. [Verificar uma extensão](./verify.md)
1. [Implantar uma extensão](./deploy.md)

## Documentação do Adobe Developer

O Adobe Developer contém detalhes para o desenvolvedor sobre a extensibilidade da IU do AEM. Revise o [conteúdo do Adobe Developer para mais detalhes técnicos](https://developer.adobe.com/uix/docs/).
