---
title: Configurar contas e serviços para a extensibilidade da Computação de ativos
description: Desenvolvendo funcionários da Asset Compute exigem acesso a contas e serviços, incluindo AEM como Cloud Service, Adobe Project Firefly e armazenamento em nuvem fornecidos pela Microsoft ou Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# Configurar contas e serviços

Este tutorial requer que os seguintes serviços sejam provisionados e acessíveis via Adobe ID do aluno.

Todos os serviços de Adobe devem estar acessíveis por meio da mesma Organização de Adobe, usando seu Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
   + O provisionamento pode levar de 2 a 10 dias
+ Armazenamento em nuvem
   + [Armazenamento Blob do Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + ou [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Verifique se você tem acesso a todos os serviços mencionados antes de continuar com este tutorial.
> 
> Analise as seções abaixo sobre como definir e fornecer os serviços necessários.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

O acesso a um AEM como ambiente Cloud Service é necessário para configurar Perfis de processamento AEM Assets para chamar o aplicativo personalizado Asset Compute.

O ideal é que um programa sandbox ou um ambiente de Desenvolvimento que não seja sandbox esteja disponível para uso.

Observe que um SDK de AEM local é insuficiente para concluir este tutorial, já que o SDK de AEM local não pode se comunicar com os microserviços do Asset Compute, em vez disso, é necessário um AEM verdadeiro como ambiente.

## Adobe Project Firefly{#adobe-project-firefly}

A estrutura [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) é usada para criar e implantar aplicativos personalizados na plataforma Adobe I/O Runtime, Adobe Server Fireverless. AEM os aplicativos de computação de ativos são aplicativos Firefly especialmente criados para integração com a AEM Assets por meio de Perfis de processamento, e oferecem a capacidade de acessar e processar binários de ativos.

Para obter acesso ao Project Firefly, inscreva-se para a pré-visualização.

1. [Inscreva-se para a pré-visualização](https://adobeio.typeform.com/to/obqgRm)do Project Firefly.
1. Aguarde aproximadamente de 2 a 10 dias até ser notificado por email de que você foi provisionado antes de continuar com o tutorial.
   + Se você não tiver certeza se foi provisionado, continue com as próximas etapas e se você não conseguir criar um projeto do __Project Firefly__ no Console [do desenvolvedor do](https://console.adobe.io) Adobe, você ainda não foi provisionado.

## Armazenamento em nuvem

O armazenamento Cloud é necessário para o desenvolvimento local de aplicativos de Computação de ativos.

Quando os aplicativos Asset Compute são implantados na Adobe I/O Runtime para uso direto por AEM como Cloud Service, esse armazenamento em nuvem não é estritamente necessário, pois AEM fornece o armazenamento em nuvem do qual o ativo é lido e renderizado.

### Armazenamento Blob do Microsoft Azure{#azure-blob-storage}

Se você ainda não tiver acesso ao Armazenamento Blob do Microsoft Azure, inscreva-se para obter uma conta [de 12 meses](https://azure.microsoft.com/en-us/free/)gratuita.

Este tutorial usará o Armazenamento Blob do Azure, no entanto, o [Amazon S3](#amazon-s3) também pode ser usado somente para variações secundárias do tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)
_Click-through de provisionamento do Armazenamento Blob do Azure (Sem áudio)_


1. Efetue logon na sua conta [do](https://azure.microsoft.com/en-us/account/)Microsoft Azure.
1. Navegue até a seção de serviços Contas __de__ Armazenamento do Azure
1. Toque em __+ Adicionar__ para criar uma nova conta de Armazenamento Blob
1. Crie um novo grupo __de__ Recursos conforme necessário, por exemplo: `aem-as-a-cloud-service`
1. Forneça um nome __de conta de__ Armazenamento, por exemplo: `aemguideswkndassetcomput`
   + O nome __da conta do__ Armazenamento será usado para [configurar o armazenamento](../develop/environment-variables.md) da nuvem para a Ferramenta de Desenvolvimento de Computação de Ativos local
   + As chaves __de__ acesso associadas à conta do armazenamento também são necessárias ao [configurar o armazenamento](../develop/environment-variables.md)em nuvem.
1. Deixe tudo o resto como padrão e toque no botão __Revisar + criar__
   + Como opção, selecione o __local__ próximo a você.
1. Revise a solicitação de provisionamento para corrigir e toque no botão __Criar__ , se satisfeito

### Amazon S3{#amazon-s3}

O uso do Armazenamento [Blob do](#azure-blob-storage) Microsoft Azure é recomendado para concluir este tutorial, no entanto o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) também pode ser usado.

Se estiver usando o armazenamento Amazon S3, especifique as credenciais do armazenamento em nuvem do Amazon S3 ao [configurar as variáveis](../develop/environment-variables.md#amazon-s3)do ambiente do projeto.

Se você precisar provisionar armazenamento em nuvem especialmente para este tutorial, recomendamos o uso do Armazenamento [Blob do](#azure-blob-storage)Azure.
