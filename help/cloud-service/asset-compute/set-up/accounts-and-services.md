---
title: Configurar contas e serviços para extensibilidade do Asset compute
description: O desenvolvimento de trabalhadores do Asset compute requer acesso a contas e serviços, incluindo AEM como Cloud Service, Adobe Project Firefly e armazenamento em nuvem fornecido pela Microsoft ou Amazon.
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---

# Configurar contas e serviços

Este tutorial requer que os seguintes serviços sejam provisionados e acessíveis pela Adobe ID do aluno.

Todos os serviços da Adobe devem ser acessíveis por meio da mesma Adobe Org, usando sua Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
   + O provisionamento pode levar de 2 a 10 dias
+ armazenamento na nuvem
   + [Armazenamento Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Certifique-se de ter acesso a todos os serviços mencionados anteriormente, antes de continuar com este tutorial.
> 
> Analise as seções abaixo sobre como definir e provisionar os serviços necessários.

## AEM como Cloud Service{#aem-as-a-cloud-service}

O acesso a um AEM como um ambiente Cloud Service é necessário para configurar os Perfis de processamento AEM Assets para chamar o trabalhador do Asset compute personalizado.

Idealmente, um programa de sandbox ou um ambiente de desenvolvimento que não seja de sandbox está disponível para uso.

Observe que um SDK de AEM local é insuficiente para concluir este tutorial, pois o SDK de AEM local não pode se comunicar com microsserviços do Asset compute, em vez disso, é necessário um AEM verdadeiro como um ambiente de Cloud Service.

## Adobe Project Firefly{#adobe-project-firefly}

A estrutura [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) é usada para criar e implantar ações personalizadas na plataforma sem servidor Adobe, Adobe I/O Runtime. AEM projetos Asset compute são projetos Firefly especialmente criados que se integram ao AEM Assets por meio de Perfis de processamento e fornecem a capacidade de acessar e processar binários de ativos.

Para obter acesso ao Project Firefly, inscreva-se para a pré-visualização.

1. [Inscreva-se para obter a visualização](https://adobeio.typeform.com/to/obqgRm) do Project Firefly.
1. Aguarde aproximadamente 2 a 10 dias até ser notificado por email de que você está provisionado antes de continuar com o tutorial.
   + Se não tiver certeza se foi provisionado, continue com as próximas etapas e se não conseguir criar um projeto __Project Firefly__ no [Adobe Developer Console](https://console.adobe.io) você ainda não foi provisionado.

## armazenamento na nuvem

O armazenamento na nuvem é necessário para o desenvolvimento local de projetos do Asset compute.

Quando os trabalhadores do Asset compute são implantados na Adobe I/O Runtime para uso direto pelo AEM como Cloud Service, esse armazenamento em nuvem não é estritamente necessário, pois AEM fornece o armazenamento em nuvem do qual o ativo é lido e renderizado.

### Armazenamento de blobs do Microsoft Azure{#azure-blob-storage}

Se você ainda não tiver acesso ao Armazenamento de blobs do Microsoft Azure, cadastre-se em uma [conta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Este tutorial usará o Armazenamento de Blobs do Azure. No entanto, [O Amazon S3](#amazon-s3) também pode ser usado somente em pequenas variações para o tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Click-through do provisionamento do Armazenamento Azure Blob (Sem áudio)_


1. Faça logon em sua [conta do Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Navegue até a seção __Contas de Armazenamento__ Serviços do Azure
1. Toque em __+ Adicionar__ para criar uma nova conta de Armazenamento de Blob
1. Crie um novo __Grupo de recursos__ conforme necessário, por exemplo: `aem-as-a-cloud-service`
1. Forneça um __Nome da conta de armazenamento__, por exemplo: `aemguideswkndassetcomput`
   + O __Nome da conta de armazenamento__ será usado para [configurar o armazenamento na nuvem](../develop/environment-variables.md) para a Ferramenta de Desenvolvimento de Assets compute local
   + As __chaves de acesso__ associadas à conta de armazenamento também são necessárias quando [configurar o armazenamento na nuvem](../develop/environment-variables.md).
1. Deixe tudo o resto como padrão e toque no botão __Revisar + criar__
   + Como opção, selecione o __local__ próximo a você.
1. Revise a solicitação de provisionamento para corrigir e toque no botão __Create__ se satisfeito

### Amazon S3{#amazon-s3}

É recomendável usar [Microsoft Azure Blob Storage](#azure-blob-storage) para concluir este tutorial. No entanto, [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) também pode ser usado.

Se estiver usando o armazenamento Amazon S3, especifique as credenciais de armazenamento da nuvem do Amazon S3 quando [configurar as variáveis de ambiente do projeto](../develop/environment-variables.md#amazon-s3).

Se você precisar provisionar o armazenamento em nuvem especialmente para este tutorial, recomendamos usar [Armazenamento Azure Blob](#azure-blob-storage).
