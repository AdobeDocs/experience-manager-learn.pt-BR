---
title: Uso do Recorte inteligente com o AEM Assets Dynamic Media
description: O Recorte inteligente usa o Adobe Sensei para eliminar as tarefas demoradas e dispendiosas de recortar o conteúdo para obter um design responsivo.
sub-product: dynamic-media
feature: Recorte inteligente, perfis de imagem
version: 6.4, 6.5
topic: Gerenciamento de conteúdo
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 3%

---


# Uso do Recorte inteligente com o AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

O Recorte inteligente usa o Adobe Sensei para eliminar as tarefas demoradas e dispendiosas de recortar o conteúdo para obter um design responsivo.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>O Vídeo assume que a instância do AEM está sendo executada no modo Dynamic Media S7. [As instruções sobre como configurar AEM com o Dynamic Media podem ser encontradas aqui.](https://helpx.adobe.com/br/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## O recurso de Corte inteligente da Dynamic Media da Adobe Experience Manager inclui

* AEM administradores de ativos podem criar facilmente perfis de imagem para Corte inteligente com base na largura e na altura do dispositivo.
* O corte inteligente pode ser executado para um ativo individual ou pode ser executado para todos os ativos em uma pasta.
* O Layout de edição de recorte inteligente pode ser redimensionado para melhor visibilidade.
* O componente Dynamic Media do AEM Sites é compatível com o Recorte inteligente.
* O URL publicado para o ativo Corte inteligente está disponível para ser usado com aplicativos de terceiros que aceitam ativos hospedados.

>[!NOTE]
>
>As coordenadas de recorte inteligente dependem da taxa de proporção. Ou seja, para as várias configurações de recorte inteligente em um perfil de imagem, se a proporção for a mesma para as dimensões adicionadas no perfil de imagem, a mesma proporção será enviada para o Dynamic Media. Por causa disso, a mesma área de corte será sugerida no Editor de recorte inteligente. Por exemplo, uma configuração de corte de 100x100 e 200x200 resultaria na geração do mesmo recorte inteligente pelo sistema.
