---
title: Criar um site do AEM
description: Crie um site no AEM Sites para Edge Delivery Services, editável usando o Editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: ae3ade0f31846776aa9bdd3a615d6514b626f48d
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# Criar um site do AEM

O site do AEM é onde o conteúdo do site é editado, gerenciado e publicado. Para criar um site do AEM entregue via Edge Delivery Services e criado com o Universal Editor, use o [modelo de site do Edge Delivery Services com AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) para criar um novo site no AEM Author.

O site do AEM é onde o conteúdo do site é armazenado e criado. A experiência final é uma combinação do conteúdo do site do AEM com o [código do site](./1-new-code-project.md).

![Novo site do AEM para Edge Delivery Services e Universal Editor](./assets/2-new-aem-site/new-site.png)

Siga as [etapas detalhadas descritas na documentação](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) para criar um novo site do AEM.  Abaixo está uma lista resumida das etapas, incluindo os valores usados neste tutorial.
1. **Criar um novo site** no AEM Author. Este tutorial usa a seguinte nomeação de site:
   * Título do site: `WKND (Universal Editor)`
   * Nome do site: `aem-wknd-eds-ue`

      * O valor do nome do site deve corresponder ao nome do caminho do site [adicionado a `paths.json`](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping).

2. **Importe o modelo mais recente** do [modelo de site de criação do Edge Delivery Services com AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Nomeie o site** para corresponder ao nome do repositório GitHub e defina a URL do GitHub como a URL do repositório.

## Publicar o novo site para visualização

Depois de criar o site no AEM Author, publique-o na visualização do Edge Delivery Services, disponibilizando o conteúdo para o [ambiente de desenvolvimento local](./3-local-development-environment.md).

1. Faça logon no **AEM Author** e navegue até o **Sites**.
2. Selecione o **novo site** (`WKND (Universal Editor)`) e clique em **Gerenciar Publicações**.
3. Escolha **Visualizar** em **Destinos** e clique em **Avançar**.
4. Em **Incluir configurações secundárias**, selecione **Incluir secundárias**, desmarque outras opções e clique em **OK**.
5. Clique em **Publicar** para publicar o conteúdo do site para visualização.
6. Depois de publicadas para visualização, as páginas ficam disponíveis no ambiente de visualização do Edge Delivery Services (as páginas não aparecerão no serviço de Visualização do AEM).
