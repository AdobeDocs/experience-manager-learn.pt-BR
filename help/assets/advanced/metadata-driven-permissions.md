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
duration: 158
source-git-commit: 6e08e6830c4e2ab27e813d262f4f51c6aae2909b
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 0%

---

# Permissões orientadas por metadados{#metadata-driven-permissions}

As Permissões orientadas por metadados são um recurso usado para permitir que as decisões de controle de acesso no AEM Assets Author sejam baseadas no conteúdo do ativo ou nas propriedades dos metadados, em vez da estrutura de pastas. Com esse recurso, você pode definir políticas de controle de acesso que avaliem atributos como status, tipo ou qualquer propriedade personalizada que você definir.

Vamos ver um exemplo. Os criadores fazem o upload do trabalho deles para o AEM Assets na pasta relacionada à campanha. Pode ser um ativo de trabalho em andamento que não foi aprovado para uso. Queremos garantir que os profissionais de marketing vejam apenas os ativos aprovados para essa campanha. Podemos utilizar uma propriedade de metadados para indicar que um ativo foi aprovado e pode ser usado pelos profissionais de marketing.

## Como funciona

A ativação de permissões orientadas por metadados envolve a definição de qual conteúdo de ativos ou propriedades de metadados direcionará as restrições de acesso, como &quot;status&quot; ou &quot;marca&quot;. Essas propriedades podem ser usadas para criar entradas de controle de acesso que especificam quais grupos de usuários têm acesso aos ativos com valores de propriedade específicos.

## Pré-requisitos

O acesso a um ambiente do AEM as a Cloud Service atualizado para a versão mais recente é necessário para configurar permissões orientadas por metadados.

## Configuração OSGi {#configure-permissionable-properties}

Para implementar Permissões orientadas por metadados, um desenvolvedor deve implantar uma configuração OSGi no AEM as a Cloud Service, que permite que propriedades específicas de conteúdo de ativos ou metadados possibilitem permissões orientadas por metadados.

1. Determine qual conteúdo de ativo ou propriedades de metadados serão usados para controle de acesso. Os nomes de propriedades são os nomes de propriedades JCR no recurso `jcr:content` ou `jcr:content/metadata` do ativo. No nosso caso, será uma propriedade chamada `status`.
1. Crie uma configuração OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` em seu projeto AEM Maven.
1. Cole o seguinte JSON no arquivo criado:

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "restrictionContentPropertyNames":[],
     "enabled":true
   }
   ```

1. Substitua os nomes de propriedade pelos valores necessários.  A propriedade de configuração `restrictionContentPropertyNames` é usada para habilitar permissões nas propriedades de recurso `jcr:content`, enquanto a propriedade de configuração `restrictionPropertyNames` habilita permissões nas propriedades de recurso `jcr:content/metadata` para ativos.

## Redefinir permissões do ativo base

Antes de adicionar Entradas de controle de acesso com base em restrição, uma nova entrada de nível superior deve ser adicionada para primeiro negar acesso de leitura a todos os grupos que estão sujeitos à avaliação de permissão para o Assets (por exemplo, &quot;colaboradores&quot; ou semelhante):

1. Navegue até a tela __Ferramentas → Segurança → Permissões__
1. Selecione o grupo __Colaboradores__ (ou outro grupo personalizado ao qual todos os grupos de usuários pertencem)
1. Clique em __Adicionar ACE__ no canto superior direito da tela
1. Selecionar `/content/dam` para __Caminho__
1. Digite `jcr:read` para __Privilégios__
1. Selecione `Deny` para __Tipo de permissão__
1. Em Restrições, selecione `rep:ntNames` e insira `dam:Asset` como o __Valor de Restrição__
1. Clique em __Salvar__

![Negar acesso](./assets/metadata-driven-permissions/deny-access.png)

## Conceder acesso a ativos por metadados

As entradas de controle de acesso agora podem ser adicionadas para conceder acesso de leitura aos grupos de usuários com base nos [valores configurados da propriedade de metadados do ativo](#configure-permissionable-properties).

1. Navegue até a tela __Ferramentas → Segurança → Permissões__
1. Selecione os grupos de usuários que devem ter acesso aos ativos
1. Clique em __Adicionar ACE__ no canto superior direito da tela
1. Selecione `/content/dam` (ou uma subpasta) para __Caminho__
1. Digite `jcr:read` para __Privilégios__
1. Selecione `Allow` para __Tipo de permissão__
1. Em __Restrições__, selecione um dos [nomes de propriedades de metadados de ativos configurados na configuração OSGi](#configure-permissionable-properties)
1. Insira o valor da propriedade de metadados necessário no campo __Valor de restrição__
1. Clique no ícone __+__ para adicionar a Restrição à Entrada de Controle de Acesso
1. Clique em __Salvar__

![Permitir acesso](./assets/metadata-driven-permissions/allow-access.png)

## Permissões orientadas por metadados em vigor

A pasta de exemplo contém alguns ativos.

![Exibição do administrador](./assets/metadata-driven-permissions/admin-view.png)

Depois de configurar as permissões e definir as propriedades de metadados do ativo de acordo, os usuários (usuário do Marketeer no nosso caso) verão somente o ativo aprovado.

![Exibição do profissional de marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Benefícios e considerações

Os benefícios das permissões orientadas por metadados incluem:

- Controle refinado do acesso a ativos com base em atributos específicos.
- Dissociação das políticas de controle de acesso da estrutura de pastas, permitindo uma organização de ativos mais flexível.
- Capacidade de definir regras complexas de controle de acesso com base em várias propriedades de conteúdo ou metadados.

>[!NOTE]
>
> É importante observar:
> 
> - As propriedades são avaliadas em relação às restrições usando __Igualdade de cadeia de caracteres__ (`=`) (outros tipos de dados ou operadores ainda não têm suporte, para propriedades maiores que (`>`) ou de Data)
> - Para permitir vários valores para uma propriedade de restrição, restrições adicionais podem ser adicionadas à Entrada de Controle de Acesso selecionando a mesma propriedade na lista suspensa &quot;Selecionar Tipo&quot; e inserindo um novo Valor de Restrição (por exemplo, `status=approved`, `status=wip`) e clicando em &quot;+&quot; para adicionar a restrição à entrada
> ![Permitir Valores Múltiplos](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Há suporte para __restrições AND__, por meio de várias restrições em uma única Entrada de Controle de Acesso com diferentes nomes de propriedade (por exemplo, `status=approved`, `brand=Adobe`) que será avaliada como uma condição AND, ou seja, o grupo de usuários selecionado receberá acesso de leitura aos ativos com `status=approved AND brand=Adobe`
> ![Permitir Várias Restrições](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - Há suporte para __restrições OR__ ao adicionar uma nova Entrada de Controle de Acesso com uma restrição de propriedade de metadados que estabelecerá uma condição OR para as entradas. Por exemplo, uma única entrada com restrição `status=approved` e uma única entrada com `brand=Adobe` serão avaliadas como `status=approved OR brand=Adobe`
> ![Permitir Várias Restrições](./assets/metadata-driven-permissions/allow-multiple-aces.png)
