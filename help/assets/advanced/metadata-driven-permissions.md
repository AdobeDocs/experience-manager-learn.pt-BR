---
title: Permissões orientadas por metadados no AEM Assets
description: As Permissões orientadas por metadados são um recurso usado para restringir o acesso com base nas propriedades dos metadados do ativo, em vez da estrutura de pastas.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 98b26eb15c2fe7d1cf73fe028b2db24087c813a5
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 0%

---

# Permissões orientadas por metadados{#metadata-driven-permissions}

As Permissões orientadas por metadados são um recurso usado para permitir que as decisões de controle de acesso no AEM Assets Author sejam baseadas nas propriedades dos metadados do ativo, em vez da estrutura de pastas. Com esse recurso, é possível definir políticas de controle de acesso que avaliem atributos como status, tipo ou qualquer propriedade de metadados personalizada que você definir.

Vamos ver um exemplo. Os criadores fazem o upload do trabalho deles para o AEM Assets na pasta relacionada à campanha. Pode ser um ativo de trabalho em andamento que não foi aprovado para uso. Queremos garantir que os profissionais de marketing vejam apenas os ativos aprovados para essa campanha. Podemos utilizar a propriedade de metadados para indicar que um ativo foi aprovado e pode ser usado pelos profissionais de marketing.

## Como funciona

A ativação de permissões orientadas por metadados envolve a definição de quais propriedades de metadados de ativos direcionarão as restrições de acesso, como &quot;status&quot; ou &quot;marca&quot;. Essas propriedades podem ser usadas para criar entradas de controle de acesso que especificam quais grupos de usuários têm acesso aos ativos com valores de propriedade específicos.

## Pré-requisitos

O acesso a um ambiente do AEM as a Cloud Service atualizado para a versão mais recente é necessário para configurar permissões orientadas por metadados.

## Configuração OSGi {#configure-permissionable-properties}

Para implementar Permissões orientadas por metadados, um desenvolvedor deve implantar uma configuração OSGi no AEM as a Cloud Service, que permite que propriedades específicas de metadados de ativos alimentem permissões orientadas por metadados.

1. Determine quais propriedades de metadados de ativos serão usadas para controle de acesso. Os nomes de propriedade são os nomes de propriedade do JCR no `jcr:content/metadata` recurso. No nosso caso, será uma propriedade chamada `status`.
1. Criar uma configuração OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` no seu projeto AEM Maven.
1. Cole o seguinte JSON no arquivo criado:

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

## Redefinir permissões do ativo base

Antes de adicionar Entradas de controle de acesso com base em restrições, uma nova entrada de nível superior deve ser adicionada para primeiro negar acesso de leitura a todos os grupos que estão sujeitos à avaliação de permissão para Ativos (por exemplo, &quot;colaboradores&quot; ou semelhante):

1. Navegue até a __Ferramentas → Segurança → Permissões__ tela
1. Selecione o __Colaboradores__ grupo (ou outro grupo personalizado ao qual todos os grupos de usuários pertencem)
1. Clique em __Adicionar ACE__ no canto superior direito da tela
1. Selecionar `/content/dam` para __Caminho__
1. Enter `jcr:read` para __Privilégios__
1. Selecionar `Deny` para __Tipo de permissão__
1. Em Restrições, selecione `rep:ntNames` e insira `dam:Asset` como o __Valor de restrição__
1. Clique em __Salvar__

![Negar acesso](./assets/metadata-driven-permissions/deny-access.png)

## Conceder acesso a ativos por metadados

As entradas de controle de acesso agora podem ser adicionadas para conceder acesso de leitura a grupos de usuários com base no [valores configurados da propriedade de metadados de ativos](#configure-permissionable-properties).

1. Navegue até a __Ferramentas → Segurança → Permissões__ tela
1. Selecione os grupos de usuários que devem ter acesso aos ativos
1. Clique em __Adicionar ACE__ no canto superior direito da tela
1. Selecionar `/content/dam` (ou uma subpasta) para __Caminho__
1. Enter `jcr:read` para __Privilégios__
1. Selecionar `Allow` para __Tipo de permissão__
1. Em __Restrições__, selecione uma das opções [nomes de propriedade dos metadados do ativo configurados na configuração do OSGi](#configure-permissionable-properties)
1. Insira o valor da propriedade de metadados necessário nas __Valor de restrição__ campo
1. Clique em __+__ ícone para adicionar a Restrição à Entrada de Controle de Acesso
1. Clique em __Salvar__

![Permitir acesso](./assets/metadata-driven-permissions/allow-access.png)

## Permissões orientadas por metadados em vigor

A pasta de exemplo contém alguns ativos.

![Exibição do administrador](./assets/metadata-driven-permissions/admin-view.png)

Depois de configurar as permissões e definir as propriedades dos metadados do ativo de acordo, os usuários (usuário do Marketeer no nosso caso) verão somente o ativo aprovado.

![Exibição do profissional de marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Benefícios e considerações

Os benefícios das permissões orientadas por metadados incluem:

- Controle refinado do acesso a ativos com base em atributos específicos.
- Dissociação das políticas de controle de acesso da estrutura de pastas, permitindo uma organização de ativos mais flexível.
- Capacidade de definir regras complexas de controle de acesso com base em várias propriedades de metadados.

>[!NOTE]
>
> É importante observar:
> 
> - As propriedades de metadados são avaliadas em relação às restrições que usam __Igualdade de string__ (`=`) (outros tipos de dados ou operadores ainda não são compatíveis, para mais de (`>`) ou Propriedades de data)
> - Para permitir vários valores para uma propriedade de restrição, restrições adicionais podem ser adicionadas à Entrada de controle de acesso selecionando a mesma propriedade na lista suspensa &quot;Selecionar tipo&quot; e inserindo um novo Valor de restrição (por exemplo, `status=approved`, `status=wip`) e clicando em &quot;+&quot; para adicionar a restrição à entrada
> ![Permitir vários valores](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __E restrições__ são suportados, por meio de várias restrições em uma única Entrada de controle de acesso com nomes de propriedades diferentes (por exemplo, `status=approved`, `brand=Adobe`) será avaliada como uma condição AND, ou seja, o grupo de usuários selecionado receberá acesso de leitura a ativos com `status=approved AND brand=Adobe`
> ![Permitir várias restrições](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __OU restrições__ são suportados pela adição de uma nova Entrada de controle de acesso com uma restrição de propriedade de metadados estabelecerá uma condição OR para as entradas, por exemplo, uma única entrada com restrição `status=approved` e uma entrada única com `brand=Adobe` será avaliado como `status=approved OR brand=Adobe`
> ![Permitir várias restrições](./assets/metadata-driven-permissions/allow-multiple-aces.png)
