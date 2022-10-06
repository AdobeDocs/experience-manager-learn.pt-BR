---
title: Uso do Recorte inteligente com o AEM Assets Dynamic Media
description: O Recorte inteligente usa o Adobe Sensei para eliminar as tarefas demoradas e dispendiosas de recortar o conteúdo para obter um design responsivo.
sub-product: dynamic-media
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 295bbfb6-241f-41c0-972d-d9688863cea1
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

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
>As coordenadas de recorte inteligente dependem da taxa de proporção. Ou seja, para as várias configurações de recorte inteligente em um perfil de imagem, se a proporção for a mesma para as dimensões adicionadas no perfil de imagem, a mesma proporção será enviada para o Dynamic Media. Por causa disso, a mesma área de corte é sugerida no Editor de Corte Inteligente. Por exemplo, uma configuração de corte de 100x100 e 200x200 resultaria na geração do mesmo recorte inteligente pelo sistema.
