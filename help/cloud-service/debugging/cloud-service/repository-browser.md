---
title: Depuração de AEM com o Navegador de Repositório
description: O Navegador de Repositório é uma ferramenta poderosa que fornece visibilidade sobre AEM armazenamento de dados subjacente, permitindo a fácil depuração AEM ambiente as a Cloud Service.
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

# Depuração de AEM as a Cloud Service com o Navegador de Repositório

O Navegador de Repositório é uma ferramenta poderosa que fornece visibilidade sobre AEM armazenamento de dados subjacente, permitindo a fácil depuração AEM ambiente as a Cloud Service. O Navegador de Repositório oferece suporte a uma visualização somente leitura dos recursos e propriedades de AEM em Produção, Estágio e Desenvolvimento, bem como aos serviços Autor, Publicação e Visualização.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

O navegador de repositório é __SOMENTE__ disponível em ambientes AEM as a Cloud Service (use [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar o SDK de AEM local).

## Acessar o navegador do repositório

Para acessar o Navegador de Repositório em AEM as a Cloud Service:

1. Certifique-se de que o usuário tenha [o acesso necessário](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Faça logon em [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Selecione o Programa que contém o ambiente as a Cloud Service AEM a ser depurado
1. Abra o [Console do desenvolvedor](./developer-console.md) correspondente ao ambiente as a Cloud Service AEM a ser depurado
1. Selecione o __Navegador de Repositório__ guia
1. Selecione a camada de serviço de AEM para procurar
   + Todos os autores
   + Todos os editores
   + Todas as visualizações
1. Selecionar __Abrir navegador do repositório__

O Navegador de Repositório é aberto para o nível de serviço selecionado (Autor, Publicação ou Visualização) em um modo somente leitura, exibindo recursos e propriedades aos quais o usuário tem acesso.

## Acesso de publicação e visualização

Por padrão, o acesso a Publicar ou Visualizar é limitado, reduzindo os recursos disponíveis no Navegador de Repositório. [Para exibir todos os recursos em Publicar (ou Visualizar), adicione usuários a uma função Publicar (ou Visualizar) administradores .](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
