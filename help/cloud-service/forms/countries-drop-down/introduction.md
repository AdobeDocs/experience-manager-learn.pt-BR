---
title: Criação do componente de lista suspensa de países
description: Crie um componente de lista suspensa de países com base em um componente principal de lista suspensa do aem forms.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: aef151bc-daf1-4abd-914a-6299f3fb58e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
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

Criar um novo componente principal no Adobe Experience Manager (AEM) requer o cumprimento de vários pré-requisitos para garantir um processo de desenvolvimento tranquilo. Veja o que você precisará antes de começar:

* Ambiente de desenvolvimento do AEM: uma instalação funcional pronta para nuvem em execução localmente
* Acesso às ferramentas de Desenvolvimento do AEM, como Visual Studio Code ou IntelliJ
* Configuração do Maven e projeto do AEM com o Arquétipo mais recente
* Conhecimento básico dos conceitos do AEM
* Ferramentas básicas e configuração, como repositório Git, versão correta do JDK


## Próximas etapas

[Criar estrutura de componente](./component.md)
