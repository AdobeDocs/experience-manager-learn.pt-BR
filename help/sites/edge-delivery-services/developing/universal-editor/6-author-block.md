---
title: Criar um bloco
description: Crie um bloco do Edge Delivery Services com o Universal Editor.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 77beb9f543bc6dc8c1ab4993c969375ce3e238e8
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# Criar um bloco

Depois de enviar o [JSON do bloco de teaser](./5-new-block.md) para a ramificação `teaser`, o bloco torna-se editável no AEM Universal Editor.

A criação de um bloco em desenvolvimento é importante por vários motivos:

1. Ele verifica se a definição e o modelo do bloco são precisos.
1. Ele permite que os desenvolvedores revisem o HTML semântico do bloco, que serve como base para o desenvolvimento.
1. Ela permite a implantação do conteúdo e do HTML semântico no ambiente de visualização, oferecendo suporte ao desenvolvimento de blocos mais rápido.

## Abrir Editor Universal usando código da ramificação `teaser`

1. Faça logon no AEM Author.
2. Navegue até **Sites** e selecione o site (WKND (Universal Editor)) criado no [capítulo anterior](./2-new-aem-site.md).

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. Crie ou edite uma página para adicionar o novo bloco, garantindo que o contexto esteja disponível para oferecer suporte ao desenvolvimento local. Embora as páginas possam ser criadas em qualquer lugar no site, geralmente é melhor criar páginas separadas para cada novo corpo de trabalho. Crie uma nova página de &quot;pasta&quot; chamada **Ramificações**. Cada subpágina é usada para suportar o desenvolvimento da ramificação Git de mesmo nome.

   ![AEM Sites - Criar página de ramificações](./assets/6-author-block/branches-page-3.png)

4. Na página **Ramificações**, crie uma nova página denominada **Teaser**, que corresponda ao nome da ramificação de desenvolvimento, e clique em **Abrir** para editar a página.

   ![AEM Sites - Criar página de teaser](./assets/6-author-block/teaser-page-3.png)

5. Atualize o Editor Universal para carregar o código da ramificação `teaser` adicionando `?ref=teaser` à URL. Certifique-se de adicionar o parâmetro de consulta **ANTES** o símbolo `#`.

   ![Editor Universal - Selecionar ramificação do teaser](./assets/6-author-block/select-branch.png)

6. Selecione a primeira seção em **Principal**, clique no botão **adicionar** e escolha o bloco **Teaser**.

   ![Editor Universal - Adicionar Bloco](./assets/6-author-block/add-teaser-2.png)

7. Na tela, selecione o teaser recém-adicionado e crie os campos à direita ou por meio do recurso de edição em linha.

   ![Editor Universal - Bloco do Autor](./assets/6-author-block/author-block.png)

8. Após concluir a criação, clique no botão **Publicar** na parte superior direita do Universal Editor, escolha Publicar na **Visualização** e publique as alterações no ambiente de visualização. As alterações são publicadas no domínio `aem.page` do site.
   ![AEM Sites - Publicar ou Visualizar](./assets/6-author-block/publish-to-preview.png)

9. Aguarde as alterações para publicar para visualização e, em seguida, abra a página da Web pela [CLI do AEM](./3-local-development-environment.md#install-the-aem-cli) em [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser).

   ![Site Local - Atualizar](./assets/6-author-block/preview.png)

Agora, o conteúdo do bloco de teaser criado e o HTML semântico estão disponíveis no site de visualização, prontos para desenvolvimento usando a CLI do AEM no ambiente de desenvolvimento local.
