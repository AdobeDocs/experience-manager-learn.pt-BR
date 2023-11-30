---
title: Configurar contas e serviços para extensibilidade do Asset compute
description: O desenvolvimento de Assets compute requer acesso a contas e serviços, incluindo AEM as a Cloud Service, Construtor de aplicativos e armazenamento em nuvem fornecido pela Microsoft ou Amazon.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%

---

# Configurar contas e serviços

Este tutorial requer que os seguintes serviços sejam provisionados e acessíveis por meio da Adobe ID do aluno.

Todos os serviços da Adobe devem ser acessíveis pela mesma organização de Adobe, usando sua Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Construtor de aplicativos](#app-builder)
   + O provisionamento pode levar de 2 a 10 dias
+ armazenamento na nuvem
   + [Armazenamento Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Certifique-se de ter acesso a todos os serviços mencionados acima antes de continuar com este tutorial.
> 
> Analise as seções abaixo sobre como definir e provisionar os serviços necessários.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

O acesso a um ambiente do AEM as a Cloud Service é necessário para configurar Perfis de processamento do AEM Assets para chamar o trabalhador de Asset compute personalizado.

Idealmente, um programa de sandbox ou um ambiente de desenvolvimento que não seja de sandbox está disponível para uso.

Observe que um SDK do AEM local é insuficiente para concluir este tutorial, pois o SDK do AEM local não pode se comunicar com os microsserviços do Asset compute AEM. Em vez disso, é necessário um ambiente as a Cloud Service verdadeiro do.

## Construtor de aplicativos{#app-builder}

A variável [Construtor de aplicativos](https://developer.adobe.com/app-builder/) A estrutura é usada para criar e implantar ações personalizadas no Adobe I/O Runtime, plataforma sem servidor Adobe. Os projetos do AEM Asset compute são projetos especialmente criados do App Builder que se integram ao AEM Assets por meio de Perfis de processamento e fornecem a capacidade de acessar e processar binários de ativos.

Para obter acesso ao App Builder, inscreva-se para a visualização.

1. [Inscrever-se na avaliação do App Builder](https://developer.adobe.com/app-builder/trial/).
1. Aguarde aproximadamente de 2 a 10 dias até que você seja notificado por e-mail de que está provisionado antes de continuar com o tutorial.
   + Se não tiver certeza se foi provisionado, continue com as próximas etapas e se não conseguir criar um __Construtor de aplicativos__ projeto em [Console do Adobe Developer](https://developer.adobe.com/console/) você ainda não foi provisionado.

## armazenamento na nuvem

O armazenamento na nuvem é necessário para o desenvolvimento local de projetos do Asset compute.

Quando os trabalhadores do Asset compute são implantados na Adobe I/O Runtime para uso direto pelo AEM as a Cloud Service AEM, esse armazenamento em nuvem não é estritamente necessário, pois o fornece o armazenamento em nuvem do qual o ativo é lido e a renderização gravada.

### Armazenamento de blobs do Microsoft Azure{#azure-blob-storage}

Se você ainda não tiver acesso ao Armazenamento de blobs do Microsoft Azure, inscreva-se para um [conta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Entretanto, este tutorial usará o Armazenamento Azure Blob [Amazon S3](#amazon-s3) O também pode ser usado somente para variações secundárias no tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Click-through de provisionamento do Armazenamento Azure Blob (sem áudio)_

1. Faça logon no [Conta do Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Navegue até a __Contas de Armazenamento__ Seção de serviços do Azure
1. Toque __+ Adicionar__ para criar uma nova conta de Armazenamento de blob
1. Criar um novo __Grupo de recursos__ conforme necessário, por exemplo: `aem-as-a-cloud-service`
1. Forneça um __Nome da conta de armazenamento__, por exemplo: `aemguideswkndassetcomput`
   + A variável __Nome da conta de armazenamento__  usado para [configuração do armazenamento na nuvem](../develop/environment-variables.md) na Ferramenta de desenvolvimento de Assets compute local
   + A variável __Chaves de acesso__ associados à conta de armazenamento também são necessários quando [configuração do armazenamento na nuvem](../develop/environment-variables.md).
1. Deixe tudo como padrão e toque no __Revisar + criar__ botão
   + Opcionalmente, selecione o __localização__ perto de você.
1. Revise a solicitação de provisionamento para verificar se está correto e toque em __Criar__ botão se satisfeito

### Amazon S3{#amazon-s3}

Usar [Armazenamento de blobs do Microsoft Azure](#azure-blob-storage) é recomendado para concluir este tutorial, no entanto [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) também pode ser usado.

Se estiver usando o armazenamento do Amazon S3, especifique as credenciais de armazenamento da nuvem do Amazon S3 quando [configuração das variáveis de ambiente do projeto](../develop/environment-variables.md#amazon-s3).

Se você precisar provisionar o armazenamento na nuvem especialmente para este tutorial, recomendamos usar [Armazenamento Azure Blob](#azure-blob-storage).
