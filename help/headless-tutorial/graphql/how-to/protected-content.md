---
title: Conteúdo protegido no AEM Headless
description: Saiba como proteger o conteúdo no AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-04-01T00:00:00Z
source-git-commit: c498783aceaf3bb389baaeaeefbe9d8d0125a82e
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 0%

---


# Proteção de conteúdo no AEM Headless

Garantir a integridade e a segurança de seus dados ao veicular conteúdo AEM Headless do AEM Publish é fundamental ao veicular conteúdo confidencial. Esta instrução aborda a proteção do conteúdo distribuído pelos endpoints da API do GraphQL sem periféricos do AEM.

Esta instrução não abrange:

- Proteger os endpoints diretamente, mas em vez disso se concentra em proteger o conteúdo que eles fornecem.
- Autenticação para publicação no AEM ou obtenção de tokens de logon. Os métodos de autenticação e a transmissão de credenciais dependem de casos de uso e implementações individuais.

## Grupos de usuários

Primeiro, devemos definir um [grupo de usuários](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) contendo os usuários que devem ter acesso ao conteúdo protegido.

![Grupo de usuários de conteúdo protegido do AEM Headless](./assets/protected-content/user-groups.png){align="center"}

Os grupos de usuários atribuem acesso ao conteúdo AEM Headless, incluindo Fragmentos de conteúdo ou outros ativos referenciados.

1. Faça logon no AEM Author as a **administrador de usuários**.
1. Navegue até **Ferramentas** > **Segurança** > **Grupos**.
1. Selecionar **Criar** no canto superior direito.
1. No **Detalhes** especifique a **ID do grupo** e **Nome do grupo**.
   - A ID do grupo e o Nome do grupo podem ser qualquer coisa, mas neste exemplo usa o nome **Usuários da API AEM Headless**.
1. Selecionar **Salvar e fechar**.
1. Selecione o grupo recém-criado e escolha **Ativar** na barra de ações.

Se vários níveis de acesso forem necessários, crie vários grupos de usuários que podem ser associados a diferentes conteúdos.

### Adicionar usuários aos grupos

Para conceder acesso a solicitações de API AEM Headless do GraphQL a conteúdo protegido, você pode associar a solicitação headless a um usuário pertencente a um grupo de usuários específico. Estas são duas abordagens comuns:

1. **AEM as a Cloud Service [contas técnicas](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Crie uma conta técnica no Console do desenvolvedor as a Cloud Service AEM.
   - Faça logon no AEM Author uma vez com a conta técnica.
   - Adicionar a conta técnica ao grupo de usuários via **Ferramentas > Segurança > Grupos > Usuários da API AEM Headless > Membros**.
   - **Ativar** tanto o usuário da conta técnica quanto o grupo de usuários no AEM Publish.
   - Este método requer que o cliente headless não exponha as Credenciais de serviço ao usuário, pois elas são credenciais de um usuário específico e não devem ser compartilhadas.

   ![Gestão de grupo de contas técnicas AEM](./assets/protected-content/group-membership.png){align="center"}

2. **Usuários nomeados:**
   - Autentique usuários nomeados e adicione-os diretamente ao grupo de usuários na publicação do AEM.
   - Esse método exige que o cliente headless autentique as credenciais do usuário com AEM Publish, obtenha um logon AEM ou token de acesso e use esse token para solicitações subsequentes ao AEM. Os detalhes de como fazer isso não são abordados nesta instrução e dependem da implementação.

## Proteção dos fragmentos de conteúdo

A proteção de fragmentos de conteúdo é essencial para proteger o conteúdo AEM headless e é alcançada ao associar o conteúdo a um Grupo de usuários fechado (CUG). Quando um usuário faz uma solicitação para a API AEM Headless do GraphQL, o conteúdo retornado é filtrado com base nos CUGs do usuário.

![AEM Headless CUGs](./assets/protected-content/cugs.png){align="center"}

Siga estas etapas para fazer isso até [Grupos de usuários fechados (CUGs)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. Faça logon no AEM Author as a **Usuário DAM**.
2. Navegue até **Ativos > Arquivos** e selecione o **pasta** que contém os fragmentos de conteúdo a serem protegidos. Os CUGs são aplicados hierarquicamente e afetam subpastas, a menos que sejam substituídos por um CUG diferente.
   - Certifique-se de que os usuários pertencentes a outros canais que utilizam o conteúdo das pastas estejam incluídos nesse grupo de usuários. Como alternativa, inclua os grupos de usuários associados a esses canais na lista de CUGs. Caso contrário, o conteúdo não estará acessível a esses canais.
3. Selecione a pasta e escolha **Propriedades** na barra de ferramentas.
4. Selecione o **Permissões** guia.
5. Digite o **Nome do grupo** e selecione o **Adicionar** botão para adicionar o novo CUG.
6. **Salvar** para aplicar o CUG.
7. **Selecionar** selecione a pasta de ativos e **Publish** para enviar a pasta com os CUGs aplicados para a publicação do AEM, onde ela será avaliada como uma permissão.

Execute essas mesmas etapas para todas as pastas que contenham Fragmentos de conteúdo que precisam ser protegidos, aplicando os CUGs corretos a cada pasta.

Agora, quando uma solicitação HTTP é feita para o endpoint da API do GraphQL sem periféricos do AEM, somente os Fragmentos de conteúdo acessíveis pelos CUGs especificados do usuário solicitante serão incluídos no resultado. Se o usuário não tiver acesso a qualquer fragmento de conteúdo, o resultado estará vazio, embora ainda retorne um código de status 200 HTTP.

### Proteção do conteúdo referenciado

Os fragmentos de conteúdo geralmente fazem referência a outro conteúdo AEM, como imagens. Para proteger esse conteúdo referenciado, aplique CUGs às pastas de ativos nas quais os ativos referenciados estão armazenados. Observe que os ativos referenciados geralmente são solicitados usando métodos distintos daqueles das APIs AEM Headless GraphQL. Consequentemente, a maneira pela qual os tokens de acesso são passados em solicitações para esses ativos referenciados pode ser diferente.

Dependendo da arquitetura de conteúdo, pode ser necessário aplicar CUGs a várias pastas para garantir que todo o conteúdo referenciado esteja protegido.

## Impedir o armazenamento em cache de conteúdo protegido

AEM as a Cloud Service [armazena em cache respostas HTTP por padrão](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) para melhorar o desempenho. No entanto, isso pode causar problemas com a veiculação de conteúdo protegido. Para impedir o armazenamento em cache desse conteúdo, [remover cabeçalhos de cache para endpoints específicos](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) na configuração do Apache da instância de publicação do AEM.

Adicione a seguinte regra ao arquivo de configuração do Apache do projeto do Dispatcher para remover cabeçalhos de cache de pontos de extremidade específicos:

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Observe que isso incorrerá em uma penalidade de desempenho, pois o conteúdo não será armazenado em cache pelo Dispatcher ou CDN. Este é um compromisso entre desempenho e segurança.

## Proteção de pontos de acesso AEM Headless da API do GraphQL

Este guia não aborda a proteção do [Endpoints de API do GraphQL sem periféricos do AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint) se concentrem na proteção do conteúdo que veiculam. Todos os usuários, incluindo usuários anônimos, podem acessar os endpoints com conteúdo protegido. Somente o conteúdo acessível pelos Grupos fechados de usuários do usuário serão retornados. Se nenhum conteúdo estiver acessível, a resposta da API AEM Headless ainda terá um código de status de resposta 200 HTTP, mas os resultados estarão vazios. Normalmente, proteger o conteúdo é suficiente, pois os próprios endpoints não expõem inerentemente dados confidenciais. Se você precisar proteger os endpoints, aplique ACLs a eles na publicação do AEM via [Scripts de inicialização do repositório Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
