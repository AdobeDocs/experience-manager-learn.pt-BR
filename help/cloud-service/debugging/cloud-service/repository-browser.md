---
title: Depuração do AEM com o Navegador do repositório
description: O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento de dados subjacente do AEM, permitindo uma depuração fácil do ambiente as a Cloud Service do AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service com o Navegador do repositório

O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento de dados subjacente do AEM, permitindo uma depuração fácil do ambiente as a Cloud Service do AEM. O Navegador do repositório é compatível com uma exibição somente leitura dos recursos e propriedades do AEM nos ambientes de Produção, Preparo e Desenvolvimento, bem como com os serviços Author, Publish e Preview.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

O Navegador do Repositório é __SOMENTE__ disponível em ambientes as a Cloud Service AEM (use [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar o AEM SDK local).

## Acessar o navegador do repositório

Para acessar o Navegador do repositório no AEM as a Cloud Service:

1. Certifique-se de que o usuário tenha [o acesso necessário](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Efetue logon no [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Selecione o programa que contém o ambiente as a Cloud Service AEM a ser depurado
1. Abra o [Console do desenvolvedor](./developer-console.md) correspondente ao ambiente do AEM as a Cloud Service para depurar
1. Selecione o __Navegador do repositório__ guia
1. Selecione a camada de serviço AEM para procurar
   + Todos os autores
   + Todos os editores
   + Todas as Visualizações
1. Selecionar __Abrir navegador do repositório__

O Navegador do Repositório é aberto para a camada de serviço selecionada (Autor, Publicação ou Visualização) em um modo somente leitura, exibindo recursos e propriedades aos quais seu usuário tem acesso.

## Acesso de publicação e visualização

Por padrão, o acesso a Publicar ou Visualizar é limitado, reduzindo os recursos disponíveis no Navegador do repositório. [Para exibir todos os recursos em Publicar (ou Visualizar), adicione usuários a uma função Publicar (ou Visualizar) Administradores.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
