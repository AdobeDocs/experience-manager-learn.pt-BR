---
title: Instale o GraphiQL IDE no AEM 6.5
description: Saiba como instalar e configurar o GraphiQL IDE no AEM 6.5
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 25%

---


# Instale o GraphiQL IDE no AEM 6.5

No AEM 6.5, a ferramenta GraphiQL IDE deve ser instalada manualmente.

1. Navegue até o **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Pesquise por &quot;GraphiQL&quot; (não deixe de incluir o **i** em **GraphiQL**).
1. Baixe a versão mais recente do **pacote de conteúdo GraphiQL v.x.x.x**.

   ![Download do pacote GraphiQL](assets/graphiql/software-distribution.png)

   O arquivo zip é um pacote AEM que pode ser instalado diretamente.

1. No menu Iniciar do AEM, navegue até **Ferramentas** > **Implantação** > **Pacotes**.
1. Clique em **Fazer upload do pacote** e escolha o pacote baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.

   ![Instalar o pacote GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Navegar para **CRXDE Lite** > **Painel do repositório** > selecionar `/content/graphiql` node (por exemplo, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. No **Propriedades** valor de alteração de guia de `endpoint` propriedade para `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Alteração de Valor da Propriedade do Ponto de Extremidade](assets/graphiql/endpoint-prop-value-change.png)

1. Navegue até o **Configuração do Console da Web** Interface do usuário > Pesquisar por **Filtro CSRF** configuração (por exemplo,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. No `Excluded Paths` atualização do campo de nome da propriedade, o caminho do terminal WKND GraphQL para `/content/cq:graphql/wknd-shared/endpoint`.

![Excluir caminhos Alteração de valor da propriedade](assets/graphiql/exclude-paths-value-change.png)

1. Acesse o editor GraphiQL usando `//HOST:PORT/content/graphiql.html`e verifique se é possível criar uma nova consulta ou executar uma existente. (por exemplo, <http://localhost:4502/content/graphiql.html>)

![Editor GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Para dar suporte ao esquema GraphQL específico do projeto e à execução da consulta, é necessário fazer as alterações correspondentes na variável `endpoint` e `Excluded Paths` em etapas acima.
