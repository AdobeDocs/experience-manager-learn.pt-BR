---
title: Criar um site de AEM
description: Crie um site no AEM Sites para Edge Delivery Services, editável usando o Editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Criar um site de AEM

O site do AEM é onde o conteúdo do site é editado, gerenciado e publicado. Para criar um site de AEM entregue via Edge Delivery Services e criado usando o Universal Editor, use o [Edge Delivery Services com o modelo de site de criação de AEM AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) para criar um novo site no Author.

O site do AEM é onde o conteúdo do site é armazenado e criado. A experiência final é uma combinação do conteúdo do site AEM com o [código do site](./1-new-code-project.md)

![Novo site AEM para Edge Delivery Services e Universal Editor](./assets/2-new-aem-site/new-site.png)

Siga as etapas abaixo para criar um novo site de AEM:

1. **Criar um novo site** no Autor do AEM. Este tutorial usa a seguinte nomeação de site:
   * Título do site: `WKND (Universal Editor)`
   * Nome do site: `aem-wknd-eds-ue`
2. **Importe o modelo mais recente** do [Edge Delivery Services com modelo de site de criação AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Nomeie o site** para corresponder ao nome do repositório GitHub e defina a URL do GitHub como a URL do repositório.

Para obter instruções detalhadas, consulte a [seção Criar e editar um novo site de AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) no guia de introdução.

## Publish do novo site para visualização

Depois de criar o site no AEM Author, publique-o na pré-visualização do Edge Delivery Services, disponibilizando o conteúdo para o [ambiente de desenvolvimento local](./3-local-development-environment.md).

1. Faça logon no **AEM Author** e navegue até o **Sites**.
2. Selecione o **novo site** (`WKND (Universal Editor)`) e clique em **Gerenciar Publicações**.
3. Escolha **Visualizar** em **Destinos** e clique em **Avançar**.
4. Em **Incluir configurações secundárias**, selecione **Incluir secundárias**, desmarque outras opções e clique em **OK**.
5. Clique em **Publish** para publicar o conteúdo do site para visualização.
