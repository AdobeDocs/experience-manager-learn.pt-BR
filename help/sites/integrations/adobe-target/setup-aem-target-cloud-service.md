---
title: Criar conta do Adobe Target Cloud Service
description: Passo a passo sobre como integrar o Adobe Experience Manager como Cloud Service com o Adobe Target usando a autenticação Cloud Service e Adobe
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Criar conta do Adobe Target Cloud Service {#adobe-target-cloud-service}

Passo a passo sobre como integrar o Adobe Experience Manager como Cloud Service com o Adobe Target usando a autenticação Cloud Service e Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Há um problema conhecido com a configuração dos Cloud Services Adobe Target mostrado no vídeo. Em vez disso, é recomendável seguir as mesmas etapas no vídeo, mas usar a configuração [](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)Legacy Adobe Target Cloud Services.

## Problemas comuns

Ao exportar o Fragmento de experiência para o público alvo Adobe, se a integração de Público alvo no console de administração não tiver a permissão correta, você poderá receber um erro, como mostrado abaixo.

**Erro** de interface do usuário da API do![Público alvo de erro da interface do usuário](assets/error-target-offer.png)

**Erro no Console da API do Erro** de Log![do Público alvo](assets/target-console-error.png)


**Solução**

1. Navegue até [Admin Console](https://adminconsole.adobe.com/)
2. Selecione Produtos > Adobe Target > Perfil do produto
3. Na guia Integrações, selecione sua integração (projeto de E/S do Adobe)
4. Atribuir função de Editor ou Aprovador.

![Erro de API de público alvo](assets/target-permissions.png)

Adicionar a permissão correta à integração do Público alvo ajudará você a resolver o problema acima.