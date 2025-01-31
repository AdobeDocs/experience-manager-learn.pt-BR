---
title: Criar um projeto de código
description: Crie um projeto de código para o Edge Delivery Services, editável usando o Editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48b402642738abf512edab68b6074935cb7dd444
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# Criar um projeto Edge Delivery Services

Para criar sites AEM para o Edge Delivery Services e o Universal Editor, use o [modelo de projeto XWalk Boilerplate do Adobe AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Este modelo cria um novo projeto de código que contém o CSS e o JavaScript usados para criar a experiência do site. Esse modelo cria um novo repositório GitHub e o carrega com o código e a configuração da placa de bloco do Adobe, fornecendo uma base sólida para o projeto do site AEM.

Lembre-se, [os sites do AEM entregues pelo Edge Delivery Services](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) têm apenas o código do lado do cliente (navegador). O código do site não é executado nos serviços AEM Author ou Publish.

![Novo projeto do Edge Delivery Services](./assets/1-new-project/new-project.png)

Siga as [etapas detalhadas descritas na documentação](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) um projeto de código Edge Delivery Services cujo conteúdo é editável no Universal Editor.  Abaixo está uma lista resumida das etapas, incluindo os valores usados neste tutorial.

1. **Configurar uma conta do GitHub.** Se estiver criando um projeto para sua organização, verifique se a organização tem uma conta GitHub e se você é membro.
2. **Crie um novo projeto de código** usando o [modelo de projeto XWalk com Boilerplate de AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Instale o aplicativo GitHub AEM Code Sync** e conceda acesso ao repositório. Você pode encontrar o [aplicativo aqui](https://github.com/apps/aem-code-sync).
4. **Configure o`fstab.yaml`** do seu novo projeto para apontar para o serviço de Autor de AEM correto.

   * Para experimentar, você pode usar ambientes AEM as a Cloud Service mais baixos (Stage ou Dev), no entanto, implementações de sites reais devem ser configuradas para usar um serviço de AEM de produção.

5. **Edite o`paths.json`** do seu novo projeto para mapear o caminho do serviço de Autor de AEM para a raiz do seu site.

Este repositório Git é clonado no capítulo [ambiente de desenvolvimento local](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) e onde o código é desenvolvido.
