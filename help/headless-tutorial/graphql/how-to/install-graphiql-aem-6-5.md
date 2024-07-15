---
title: Instalar o GraphiQL IDE no AEM 6.5
description: Saiba como instalar e configurar o GraphiQL IDE no AEM 6.5
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 14%

---

# Instalar o GraphiQL IDE no AEM 6.5

No AEM 6.5, a ferramenta GraphiQL IDE deve ser instalada manualmente.

1. Navegue até o **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Pesquise por &quot;GraphiQL&quot; (certifique-se de incluir o **i** em **GraphiQL**).
1. Baixe o **Pacote de Conteúdo GraphiQL v.x.x.x** mais recente.

   ![Baixar Pacote GraphiQL](assets/graphiql/software-distribution.png)

   O arquivo zip é um pacote AEM que pode ser instalado diretamente.

1. No menu Iniciar do AEM, navegue até **Ferramentas** > **Implantação** > **Pacotes**.
1. Clique em **Fazer upload do pacote** e escolha o pacote baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.

   ![Instalar Pacote GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Navegue até **CRXDE Lite** > **Painel do Repositório** > selecione o nó `/content/graphiql` (por exemplo, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. Na guia **Propriedades**, altere o valor da propriedade `endpoint` para `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Alteração do Valor da Propriedade do Ponto de Extremidade](assets/graphiql/endpoint-prop-value-change.png)

1. Navegue até a **Configuração do Console da Web** > Pesquisar por **Configuração do Filtro CSRF** (por exemplo,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. Na atualização do campo de nome de propriedade `Excluded Paths`, o caminho do ponto de extremidade WKND GraphQL para `/content/cq:graphql/wknd-shared/endpoint`.

![Excluir Alteração do Valor da Propriedade dos Caminhos](assets/graphiql/exclude-paths-value-change.png)

1. Acesse o editor de GraphiQL usando `//HOST:PORT/content/graphiql.html` e verifique se é possível construir uma nova consulta ou executar uma consulta existente. (ex.: <http://localhost:4502/content/graphiql.html>)

![Editor de GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Para suportar o esquema GraphQL específico do seu projeto e a execução da consulta, você deve fazer as alterações correspondentes para os valores `endpoint` e `Excluded Paths` nas etapas acima.
