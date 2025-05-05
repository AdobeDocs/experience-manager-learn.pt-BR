---
title: Depuração do AEM com o Navegador do repositório
description: O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento interno de dados do AEM, permitindo uma depuração fácil do ambiente do AEM as a Cloud Service.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service com o Navegador do repositório

O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento interno de dados do AEM, permitindo uma depuração fácil do ambiente do AEM as a Cloud Service. O Navegador do repositório é compatível com uma exibição somente leitura dos recursos e propriedades do AEM nos ambientes de Produção, Preparo e Desenvolvimento, bem como dos serviços Author, Publish e Preview.

>[!VIDEO](https://video.tv.adobe.com/v/3447059?quality=12&learn=on&captions=por_br)

O Navegador do Repositório __SOMENTE__ está disponível em ambientes AEM as a Cloud Service (use o [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar o AEM SDK local).

## Acessar o navegador do repositório

Para acessar o Navegador do repositório no AEM as a Cloud Service:

1. Verifique se o usuário tem [o acesso necessário](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=pt-BR#access-prerequisites)
1. Fazer logon no [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Selecione o programa que contém o ambiente AEM as a Cloud Service a ser depurado
1. Abra o [Developer Console](./developer-console.md) correspondente ao ambiente do AEM as a Cloud Service para depurar
1. Selecione a guia __Navegador do repositório__
1. Selecione a camada de serviço do AEM a ser procurada
   + Todos os autores
   + Todos os editores
   + Todas as Visualizações
1. Selecione __Abrir Navegador do Repositório__

O Navegador do Repositório é aberto para a camada de serviço selecionada (Autor, Publicação ou Visualização) em um modo somente leitura, exibindo recursos e propriedades aos quais seu usuário tem acesso.

## Acesso de publicação e visualização

Por padrão, o acesso a Publicar ou Visualizar é limitado, reduzindo os recursos disponíveis no Navegador do repositório. [Para exibir todos os recursos em Publicar (ou Visualizar), adicione usuários a uma função de Administradores de Publicação (ou Visualização).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=pt-BR#navigate-the-hierarchy)
