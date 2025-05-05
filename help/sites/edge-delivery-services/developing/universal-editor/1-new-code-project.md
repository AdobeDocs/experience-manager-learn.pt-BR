---
title: Criar um projeto de código
description: Crie um projeto de código para o Edge Delivery Services, editável usando o Editor universal.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# Criar um projeto de código Edge Delivery Services

Para criar sites do AEM para o Edge Delivery Services e o Universal Editor, use o [modelo de projeto XWalk padronizado do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk) da Adobe. Este modelo cria um novo projeto de código que contém o CSS e o JavaScript usados para criar a experiência do site. Esse modelo cria um novo repositório GitHub e o carrega com o código e a configuração padronizados da Adobe, fornecendo uma base sólida para o projeto do site da AEM.

Lembre-se, [os sites do AEM entregues pelo Edge Delivery Services](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/sites/edge-delivery-services/overview) têm apenas o código do lado do cliente (navegador). O código do site não é executado nos serviços de Autor ou Publicação do AEM.

![Novo projeto do Edge Delivery Services](./assets/1-new-project/new-project.png)

Siga as [etapas detalhadas descritas na documentação](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) para criar um projeto de código Edge Delivery Services cujo conteúdo é editável no Universal Editor.  Abaixo está uma lista resumida das etapas, incluindo os valores usados neste tutorial.

1. **Configurar uma conta do GitHub.** Se estiver criando um projeto para sua organização, verifique se a organização tem uma conta GitHub e se você é membro.
2. **Crie um novo projeto de código** usando o [modelo de projeto AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Instale o aplicativo GitHub AEM Code Sync** e conceda acesso ao repositório. Você pode encontrar o [aplicativo aqui](https://github.com/apps/aem-code-sync).
4. **Configure o`fstab.yaml`** do seu novo projeto para apontar para o serviço de Autor correto do AEM.

   * Para experimentar, você pode usar ambientes AEM as a Cloud Service mais baixos (Preparo ou Desenvolvimento), no entanto, implementações de sites reais devem ser configuradas para usar um serviço de produção do AEM.

5. **Edite o`paths.json`** do seu novo projeto para mapear o caminho de serviço do Autor do AEM para a raiz do seu site.

Este repositório Git é clonado no capítulo [ambiente de desenvolvimento local](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) e onde o código é desenvolvido.
