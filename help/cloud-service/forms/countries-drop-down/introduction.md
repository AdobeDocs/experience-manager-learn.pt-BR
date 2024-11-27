---
title: Criação do componente de lista suspensa de países
description: Crie um componente de lista suspensa de países com base em um componente principal de lista suspensa do aem forms.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Criar um componente de lista suspensa de países com base em um componente de lista suspensa

A criação de um novo componente principal no Adobe Experience Manager (AEM) é um processo interessante que envolve várias etapas, incluindo a definição da estrutura do componente, a personalização da caixa de diálogo e a implementação de um modelo Sling para funcionalidade dinâmica.

No final deste tutorial, você terá aprendido a:

* Crie e use um modelo Sling para buscar dados dinamicamente.
* Personalize o cq-dialog adicionando novos campos e ocultando outros.
* Defina uma estrutura de componentes robusta e personalizada para reutilização.

O componente, chamado de &quot;Países&quot;, permitirá que os usuários selecionem um continente e preencham dinamicamente uma lista suspensa com países correspondentes ao continente escolhido. Ele será construído com base no componente de lista suspensa pronto para uso, aprimorado para este caso de uso específico.

Vamos nos aprofundar e criar esse componente dinâmico e poderoso.

## Pré-requisitos

Criar um novo componente principal no Adobe Experience Manager (AEM) requer atender a vários pré-requisitos para garantir um processo de desenvolvimento tranquilo. Veja o que você precisará antes de começar:

* Ambiente de desenvolvimento do AEM: uma instalação funcional pronta para nuvem em execução localmente
* Acesso a ferramentas de Desenvolvimento do AEM, como Visual Studio Code ou IntelliJ
* Configuração do MAven e projeto AEM com o Arquétipo mais recente
* Conhecimento básico dos conceitos de AEM
* Ferramentas básicas e configuração, como repositório Git, versão correta do JDK


## Próximas etapas

[Criar estrutura de componente](./component.md)
