---
title: Criar um projeto de código
description: Crie um projeto de código para o Edge Delivery Services, editável por meio do editor universal.
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
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# Criar um projeto de código do Edge Delivery Services

Para criar sites do AEM para o Edge Delivery Services e o editor universal, use o [modelo de projeto XWalk padrão do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk), fornecido pela Adobe. Esse modelo cria um novo projeto de código que contém o CSS e o JavaScript usados para criar a experiência do site. Esse modelo cria um novo repositório do GitHub e carrega-o com o código e a configuração padronizados da Adobe, fornecendo uma base sólida para o projeto de site do AEM.

Observe que [os sites do AEM entregues pelo Edge Delivery Services](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/sites/edge-delivery-services/overview) contam apenas com o código do lado do cliente (navegador). O código do site não é executado nos serviços de criação e publicação do AEM.

![Novo projeto do Edge Delivery Services](./assets/1-new-project/new-project.png)

Siga as [etapas detalhadas descritas na documentação](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) para criar um projeto de código do Edge Delivery Services com conteúdo editável por meio do editor universal.  Confira abaixo uma lista resumida das etapas, incluindo os valores usados neste tutorial.

1. **Configure uma conta do GitHub.** Se estiver criando um projeto para a sua organização, verifique se a organização tem uma conta do GitHub e se você é membro.
2. **Crie um novo projeto de código** usando o [modelo de projeto XWalk padrão do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Instale o aplicativo AEM Code Sync do GitHub** e conceda acesso ao repositório. Você pode encontrar o [aplicativo aqui](https://github.com/apps/aem-code-sync).
4. **Configure o`fstab.yaml`** do seu novo projeto para apontar para o serviço correto do ambiente de criação do AEM.

   * Para realizar um experimento, é possível usar ambientes inferiores do AEM as a Cloud Service (preparo ou desenvolvimento), mas você deverá configurar implementações de sites reais para usar um serviço de produção do AEM.

5. **Edite o`paths.json`** do seu novo projeto para mapear o caminho do serviço de criação do AEM para a raiz do seu site.

Esse repositório do Git é clonado no capítulo [Ambiente de desenvolvimento local](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment), e este também é o local onde o código é desenvolvido.
