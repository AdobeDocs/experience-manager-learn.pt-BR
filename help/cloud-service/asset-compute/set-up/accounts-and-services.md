---
title: Configurar contas e serviços para extensibilidade do Asset compute
description: O desenvolvimento de trabalhadores do Asset compute exige acesso a contas e serviços, incluindo AEM as a Cloud Service, App Builder e armazenamento em nuvem fornecido pela Microsoft ou Amazon.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%

---

# Configurar contas e serviços

Este tutorial requer que os seguintes serviços sejam provisionados e acessíveis pela Adobe ID do aluno.

Todos os serviços da Adobe devem ser acessíveis por meio da mesma Adobe Org, usando sua Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + O provisionamento pode levar de 2 a 10 dias
+ armazenamento na nuvem
   + [Armazenamento Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Certifique-se de ter acesso a todos os serviços mencionados anteriormente, antes de continuar com este tutorial.
> 
> Analise as seções abaixo sobre como definir e provisionar os serviços necessários.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

O acesso a um ambiente as a Cloud Service AEM é necessário para configurar os Perfis de processamento AEM Assets para chamar o trabalhador do Asset compute personalizado.

Idealmente, um programa de sandbox ou um ambiente de desenvolvimento que não seja de sandbox está disponível para uso.

Observe que um SDK de AEM local é insuficiente para concluir este tutorial, pois o SDK de AEM local não pode se comunicar com os microsserviços do Asset compute, em vez disso, é necessário um ambiente AEM verdadeiro.

## App Builder{#app-builder}

O [App Builder](https://developer.adobe.com/app-builder/) Essa estrutura é usada para criar e implantar ações personalizadas na plataforma Adobe sem servidor. AEM projetos Asset compute são projetos especialmente desenvolvidos do App Builder que se integram ao AEM Assets por meio de Perfis de processamento e fornecem a capacidade de acessar e processar binários de ativos.

Para obter acesso ao App Builder, cadastre-se na visualização.

1. [Inscreva-se para a avaliação do App Builder](https://developer.adobe.com/app-builder/trial/).
1. Aguarde aproximadamente 2 a 10 dias até ser notificado por email de que você está provisionado antes de continuar com o tutorial.
   + Se não tiver certeza se foi provisionado, continue com as próximas etapas e se não conseguir criar um __App Builder__ projeto em [Console do Adobe Developer](https://developer.adobe.com/console/) você ainda não foi provisionado.

## armazenamento na nuvem

O armazenamento na nuvem é necessário para o desenvolvimento local de projetos do Asset compute.

Quando os trabalhadores do Asset compute são implantados na Adobe I/O Runtime para uso direto por AEM as a Cloud Service, esse armazenamento em nuvem não é estritamente necessário, pois AEM fornece o armazenamento em nuvem do qual o ativo é lido e renderizado.

### Armazenamento de blobs do Microsoft Azure{#azure-blob-storage}

Se você ainda não tiver acesso ao Microsoft Azure Blob Storage, cadastre-se em um [conta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

No entanto, este tutorial usará o Armazenamento Azure Blob [Amazon S3](#amazon-s3) também pode ser usada somente uma pequena variação do tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Click-through do provisionamento do Armazenamento Azure Blob (Sem áudio)_

1. Faça logon no [Conta do Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Navegue até o __Contas de armazenamento__ Seção de serviços do Azure
1. Toque __+ Adicionar__ para criar uma nova conta de armazenamento de blob
1. Crie um novo __Grupo de recursos__ conforme necessário, por exemplo: `aem-as-a-cloud-service`
1. Forneça uma __Nome da conta de armazenamento__, por exemplo: `aemguideswkndassetcomput`
   + O __Nome da conta de armazenamento__  usado para [configuração do armazenamento em nuvem](../develop/environment-variables.md) na Ferramenta de desenvolvimento de Assets compute local
   + O __Chaves de acesso__ associados à conta de armazenamento também são necessários quando [configuração do armazenamento em nuvem](../develop/environment-variables.md).
1. Deixe tudo o resto como padrão e toque no __Revisar + criar__ botão
   + Como opção, selecione a __localização__ perto de você.
1. Revise a solicitação de provisionamento para corrigir e toque em __Criar__ botão se estiver satisfeito

### Amazon S3{#amazon-s3}

Usando [Armazenamento de blobs do Microsoft Azure](#azure-blob-storage) é recomendado para concluir este tutorial, no entanto [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) também pode ser usado.

Se estiver usando o armazenamento Amazon S3, especifique as credenciais de armazenamento em nuvem do Amazon S3 ao [configuração das variáveis de ambiente do projeto](../develop/environment-variables.md#amazon-s3).

Se você precisar provisionar o armazenamento em nuvem especialmente para este tutorial, recomendamos usar [Armazenamento Azure Blob](#azure-blob-storage).
