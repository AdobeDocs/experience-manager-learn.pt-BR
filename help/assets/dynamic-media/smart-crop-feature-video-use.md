---
title: Uso do Recorte inteligente com o AEM Assets Dynamic Media
description: O Recorte inteligente usa o Adobe Sensei para eliminar as tarefas demoradas e caras de recortar conteúdo para oferecer design responsivo.
sub-product: dynamic-media
feature: Recorte inteligente, perfis de imagem
version: 6.4, 6.5
topic: Gerenciamento de conteúdo
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 3%

---


# Uso do Recorte inteligente com o AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

O Recorte inteligente usa o Adobe Sensei para eliminar as tarefas demoradas e caras de recortar conteúdo para oferecer design responsivo.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>O Vídeo assume que a instância do AEM está sendo executada no modo Dynamic Media S7. [Instruções sobre como configurar o AEM com o Dynamic Media podem ser encontradas aqui.](https://helpx.adobe.com/br/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## O recurso de Corte inteligente de mídia dinâmica do Adobe Experience Manager inclui

* Os administradores do AEM Asset podem criar perfis de imagem facilmente para Corte inteligente com base na largura e na altura do dispositivo.
* O corte inteligente pode ser executado para um ativo individual ou pode ser executado para todos os ativos em uma pasta.
* O Layout de edição de recorte inteligente pode ser redimensionado para melhor visibilidade.
* O componente Mídia dinâmica do AEM Sites é compatível com o Recorte inteligente.
* O URL publicado para o ativo Corte inteligente está disponível para ser usado com aplicativos de terceiros que aceitam ativos hospedados.

>[!NOTE]
>
>As coordenadas de recorte inteligente dependem da taxa de proporção. Ou seja, para as várias configurações de recorte inteligente em um perfil de imagem, se a proporção for a mesma para as dimensões adicionadas no perfil de imagem, a mesma proporção será enviada para o Dynamic Media. Por causa disso, a mesma área de corte será sugerida no Editor de recorte inteligente. Por exemplo, uma configuração de corte de 100x100 e 200x200 resultaria na geração do mesmo recorte inteligente pelo sistema.
