---
title: Configurar contas e serviços para extensibilidade do Asset Compute
description: O desenvolvimento de funcionários da Asset Compute exige acesso a contas e serviços, incluindo AEM as a Cloud Service, App Builder e armazenamento em nuvem fornecido pela Microsoft ou Amazon.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 1%

---

# Configurar contas e serviços

Este tutorial requer que os seguintes serviços sejam provisionados e acessíveis por meio da Adobe ID do aluno.

Todos os serviços da Adobe devem ser acessíveis pela mesma organização da Adobe, usando sua Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
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

O acesso a um ambiente do AEM as a Cloud Service é necessário para configurar Perfis de processamento do AEM Assets para chamar o trabalhador personalizado do Asset Compute.

Idealmente, um programa de sandbox ou um ambiente de desenvolvimento que não seja de sandbox está disponível para uso.

Observe que um AEM SDK local é insuficiente para concluir este tutorial, pois o AEM SDK local não pode se comunicar com os microsserviços da Asset Compute. Em vez disso, é necessário um ambiente AEM as a Cloud Service verdadeiro.

## App Builder{#app-builder}

A estrutura [App Builder](https://developer.adobe.com/app-builder/) é usada para criar e implantar ações personalizadas na Adobe I/O Runtime, a plataforma sem servidor da Adobe. Os projetos do AEM Asset Compute são projetos do App Builder especialmente criados que se integram ao AEM Assets por meio de Perfis de processamento, e fornecem a capacidade de acessar e processar binários de ativos.

Para obter acesso ao App Builder, inscreva-se para obter a visualização.

1. [Inscreva-se na avaliação do App Builder](https://developer.adobe.com/app-builder/trial/).
1. Aguarde aproximadamente de 2 a 10 dias até que você seja notificado por e-mail de que está provisionado antes de continuar com o tutorial.
   + Se você não tiver certeza se foi provisionado, continue com as próximas etapas e se não conseguir criar um projeto do __App Builder__ no [Adobe Developer Console](https://developer.adobe.com/console/), você ainda não foi provisionado.

## armazenamento na nuvem

O armazenamento na nuvem é necessário para o desenvolvimento local de projetos do Asset Compute.

Quando os funcionários da Asset Compute são implantados na Adobe I/O Runtime para uso direto pela AEM as a Cloud Service, esse armazenamento em nuvem não é estritamente necessário, pois a AEM fornece o armazenamento em nuvem do qual o ativo é lido e a representação gravada.

### Armazenamento de blobs do Microsoft Azure{#azure-blob-storage}

Se você ainda não tiver acesso ao Armazenamento de Blobs do Microsoft Azure, inscreva-se para obter uma [conta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Este tutorial usará o Armazenamento de Blobs do Azure, no entanto, o [Amazon S3](#amazon-s3) também pode ser usado, somente para pequenas variações no tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Click-through do provisionamento do Armazenamento Azure Blob (Sem áudio)_

1. Faça logon em sua [conta do Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Navegue até a __seção Contas de Armazenamento__ dos serviços do Azure
1. Toque em __+ Adicionar__ para criar uma nova conta de Armazenamento de Blobs
1. Crie um novo __Grupo de recursos__ conforme necessário, por exemplo: `aem-as-a-cloud-service`
1. Forneça um __nome da conta de armazenamento__, por exemplo: `aemguideswkndassetcomput`
   + O __nome da conta de armazenamento__ usado para [configurar o armazenamento na nuvem](../develop/environment-variables.md) na Ferramenta de Desenvolvimento Asset Compute local
   + As __chaves de acesso__ associadas à conta de armazenamento também são necessárias ao [configurar o armazenamento na nuvem](../develop/environment-variables.md).
1. Deixe tudo como padrão e toque no botão __Revisar + criar__
   + Opcionalmente, selecione o __local__ próximo a você.
1. Revise se se a solicitação de provisionamento está correta e toque no botão __Criar__, se satisfeito

### Amazon S3{#amazon-s3}

Recomendamos o uso do [Armazenamento de Blob do Microsoft Azure](#azure-blob-storage) para concluir este tutorial, no entanto, o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) também pode ser usado.

Se estiver usando o armazenamento Amazon S3, especifique as credenciais de armazenamento na nuvem do Amazon S3 ao [configurar as variáveis de ambiente do projeto](../develop/environment-variables.md#amazon-s3).

Se você precisar provisionar o armazenamento na nuvem especialmente para este tutorial, recomendamos usar o [Armazenamento Azure Blob](#azure-blob-storage).
