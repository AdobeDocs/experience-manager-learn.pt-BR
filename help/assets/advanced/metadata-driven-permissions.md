---
title: Permissões orientadas por metadados no AEM Assets
description: As Permissões orientadas por metadados são um recurso usado para restringir o acesso com base nas propriedades dos metadados do ativo, em vez da estrutura de pastas.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 03cb7ef0cf79a21ec1b96caf6c11e6f5119f777c
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 0%

---

# Permissões orientadas por metadados{#metadata-driven-permissions}

As Permissões orientadas por metadados são um recurso usado para permitir que as decisões de controle de acesso no AEM Assets Author sejam baseadas nas propriedades dos metadados do ativo, em vez da estrutura de pastas. Com esse recurso, é possível definir políticas de controle de acesso que avaliem atributos como status, tipo ou qualquer propriedade de metadados personalizada que você definir.

Vamos ver um exemplo. Os criadores fazem o upload do trabalho deles para o AEM Assets na pasta relacionada à campanha. Pode ser um ativo de trabalho em andamento que não foi aprovado para uso. Queremos garantir que os profissionais de marketing vejam apenas os ativos aprovados para essa campanha. Podemos utilizar a propriedade de metadados para indicar que um ativo foi aprovado e pode ser usado pelos profissionais de marketing.

## Como funciona

A ativação de permissões orientadas por metadados envolve a definição de quais propriedades de metadados de ativos direcionarão as restrições de acesso, como &quot;status&quot; ou &quot;marca&quot;. Essas propriedades podem ser usadas para criar entradas de controle de acesso que especificam quais grupos de usuários têm acesso aos ativos com valores de propriedade específicos.

## Pré-requisitos

O acesso a um ambiente do AEM as a Cloud Service atualizado para a versão mais recente é necessário para configurar permissões orientadas por metadados.


## Etapas de desenvolvimento

Para implementar permissões orientadas por metadados:

1. Determine quais propriedades de metadados de ativos serão usadas para controle de acesso. No nosso caso, será uma propriedade chamada `status`.
1. Criar uma configuração OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` no seu projeto.
1. Cole o seguinte JSON no arquivo criado

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. Substitua os nomes de propriedade pelos valores necessários.


Antes de adicionar Entradas de controle de acesso com base em restrições, uma nova entrada de nível superior deve ser adicionada para primeiro negar acesso de leitura a todos os grupos que estão sujeitos à avaliação de permissão para Ativos (por exemplo, &quot;colaboradores&quot; ou semelhante):

1. Navegue até a tela Ferramentas → Segurança → Permissões
1. Selecione o grupo &quot;Colaboradores&quot; (ou outro grupo personalizado ao qual todos os grupos de usuários pertencem)
1. Clique em &quot;Adicionar ACE&quot; no canto superior direito da tela
1. Selecione /content/dam para Caminho
1. Digite jcr:read em Privilégios
1. Selecionar Negar para Tipo de Permissão
1. Em Restrições, selecione rep:ntNames e informe dam:Asset como o Valor de Restrição
1. Clique em Salvar

![Negar acesso](./assets/metadata-driven-permissions/deny-access.png)

As entradas de controle de acesso agora podem ser adicionadas para conceder acesso de leitura aos grupos de usuários com base nos valores da propriedade de metadados do ativo.

1. Navegue até a tela Ferramentas → Segurança → Permissões
1. Selecione o grupo desejado
1. Clique em &quot;Adicionar ACE&quot; no canto superior direito da tela
1. Selecione /content/dam (ou uma subpasta) para Caminho
1. Digite jcr:read em Privilégios
1. Selecione Permitir para tipo de permissão
1. Em Restrições, selecione um dos nomes de propriedade dos metadados de ativos configurados (as propriedades definidas na configuração do OSGi serão incluídas aqui)
1. Insira o valor da propriedade de metadados necessário no campo Valor de restrição
1. Clique no ícone &quot;+&quot; para adicionar a Restrição à Entrada de controle de acesso
1. Clique em Salvar

![Permitir acesso](./assets/metadata-driven-permissions/allow-access.png)

A pasta de exemplo contém alguns ativos.

![Exibição do administrador](./assets/metadata-driven-permissions/admin-view.png)

Depois de configurar as permissões e definir as propriedades dos metadados do ativo de acordo, os usuários (usuário do Marketeer no nosso caso) verão somente o ativo aprovado.

![Exibição do profissional de marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Benefícios e considerações

Os benefícios das Permissões orientadas por metadados incluem:

- Controle refinado do acesso a ativos com base em atributos específicos.
- Dissociação das políticas de controle de acesso da estrutura de pastas, permitindo uma organização de ativos mais flexível.
- Capacidade de definir regras complexas de controle de acesso com base em várias propriedades de metadados.

>[!NOTE]
>
> É importante observar:
> 
> - As propriedades de metadados são avaliadas em relação às restrições usando a igualdade de string (outros tipos de dados ainda não suportados, por exemplo, data)
> - Para permitir vários valores para uma propriedade de restrição, restrições adicionais podem ser adicionadas à Entrada de controle de acesso selecionando a mesma propriedade na lista suspensa &quot;Selecionar tipo&quot; e inserindo um novo Valor de restrição (por exemplo, `status=approved`, `status=wip`) e clicando em &quot;+&quot; para adicionar a restrição à entrada
> ![Permitir vários valores](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Várias restrições em uma única Entrada de controle de acesso com nomes de propriedades diferentes (por exemplo, `status=approved`, `brand=Adobe`) será avaliada como uma condição AND, ou seja, o grupo de usuários selecionado receberá acesso de leitura a ativos com `status=approved AND brand=Adobe`
> ![Permitir várias restrições](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - Adicionar uma nova Entrada de controle de acesso com uma restrição de propriedade de metadados estabelecerá uma condição OR para as entradas, por exemplo, uma única entrada com restrição `status=approved` e uma entrada única com `brand=Adobe` será avaliado como `status=approved OR brand=Adobe`
> ![Permitir várias restrições](./assets/metadata-driven-permissions/allow-multiple-aces.png)
