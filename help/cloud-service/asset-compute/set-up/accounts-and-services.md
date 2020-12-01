---
title: Configurar contas e serviços para extensibilidade do Asset compute
description: Os trabalhadores do Asset compute em desenvolvimento precisam de acesso a contas e serviços, inclusive AEM como Cloud Service, Adobe Project Firefly e armazenamento em nuvem fornecidos pela Microsoft ou Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
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

O acesso a um AEM como ambiente Cloud Service é necessário para configurar Perfis de processamento AEM Assets para chamar o funcionário do Asset compute personalizado.

O ideal é que um programa sandbox ou um ambiente de Desenvolvimento que não seja sandbox esteja disponível para uso.

Observe que um SDK de AEM local é insuficiente para concluir este tutorial, já que o SDK de AEM local não pode se comunicar com microserviços de Asset compute, em vez disso, é necessário um AEM verdadeiro como ambiente.

## Adobe Project Firefly{#adobe-project-firefly}

A estrutura &lt;a0>Adobe Project Firefly[ é usada para criar e implantar ações personalizadas na plataforma Adobe I/O Runtime, Adobe Server/Wireless. ](https://www.adobe.io/apis/experienceplatform/project-firefly.html) AEM projetos de Asset computes são projetos Firefly especialmente construídos que se integram à AEM Assets por meio de Perfis de processamento e oferecem a capacidade de acessar e processar binários de ativos.

Para obter acesso ao Project Firefly, inscreva-se para a pré-visualização.

1. [Inscreva-se para a pré-visualização](https://adobeio.typeform.com/to/obqgRm) do Project Firefly.
1. Aguarde aproximadamente de 2 a 10 dias até ser notificado por email de que você foi provisionado antes de continuar com o tutorial.
   + Se você não tiver certeza se foi provisionado, continue com as próximas etapas e se você não conseguir criar um projeto __Project Firefly__ no Console do desenvolvedor do Adobe[você ainda não foi provisionado.](https://console.adobe.io)

## Armazenamento em nuvem

O armazenamento em nuvem é necessário para o desenvolvimento local de projetos de Asset computes.

Quando os funcionários do Asset compute são implantados na Adobe I/O Runtime para uso direto por AEM como Cloud Service, esse armazenamento em nuvem não é estritamente necessário, pois AEM fornece o armazenamento em nuvem do qual o ativo é lido e renderizado.

### Armazenamento Blob do Microsoft Azure{#azure-blob-storage}

Se você ainda não tiver acesso ao Armazenamento Blob do Microsoft Azure, inscreva-se para uma conta gratuita de [12 meses](https://azure.microsoft.com/en-us/free/).

Este tutorial usará o Armazenamento Blob do Azure, no entanto, [O Amazon S3](#amazon-s3) também pode ser usado somente para variações secundárias do tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Click-through de provisionamento do Armazenamento Blob do Azure (Sem áudio)_


1. Efetue logon em sua [conta do Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Navegue até a seção __Contas de Armazenamento__ Serviços do Azure
1. Toque em __+ Adicionar__ para criar uma nova conta de Armazenamento Blob
1. Crie um novo __grupo de recursos__ conforme necessário, por exemplo: `aem-as-a-cloud-service`
1. Forneça um __nome de conta de Armazenamento__, por exemplo: `aemguideswkndassetcomput`
   + O __nome da conta do Armazenamento__ será utilizado para [configurar o armazenamento em nuvem](../develop/environment-variables.md) para a Ferramenta de Desenvolvimento de Asset computes local
   + As __chaves de acesso__ associadas à conta do armazenamento também são necessárias quando [configurar o armazenamento de nuvem](../develop/environment-variables.md).
1. Deixe tudo o resto como padrão e toque no botão __Revisar + criar__
   + Como opção, selecione __location__ próximo a você.
1. Revise a solicitação de provisionamento para corrigir e toque no botão __Criar__ se satisfeito

### Amazon S3{#amazon-s3}

É recomendável usar [O Armazenamento Blob do Microsoft Azure](#azure-blob-storage) para concluir este tutorial, no entanto, [O Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) também pode ser usado.

Se estiver usando o armazenamento Amazon S3, especifique as credenciais do armazenamento em nuvem do Amazon S3 quando [configurar as variáveis do ambiente do projeto](../develop/environment-variables.md#amazon-s3).

Se precisar provisionar armazenamento em nuvem especialmente para este tutorial, recomendamos o uso de [Armazenamento Blob do Azure](#azure-blob-storage).
