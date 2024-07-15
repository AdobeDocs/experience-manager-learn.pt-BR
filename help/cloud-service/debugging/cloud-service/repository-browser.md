---
title: Depuração do AEM com o Navegador do repositório
description: O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento de dados subjacente do AEM, permitindo uma depuração fácil do ambiente do AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service com o Navegador do repositório

O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento de dados subjacente do AEM, permitindo uma depuração fácil do ambiente do AEM as a Cloud Service. O Navegador do repositório é compatível com uma exibição somente leitura dos recursos e propriedades do AEM nos ambientes de Produção, Preparo e Desenvolvimento, bem como com os serviços Author, Publish e Preview.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

O Navegador do Repositório é __ONLY__ disponível em ambientes AEM as a Cloud Service (use [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar o SDK do AEM local).

## Acessar o navegador do repositório

Para acessar o Navegador do repositório no AEM as a Cloud Service:

1. Verifique se o usuário tem [o acesso necessário](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Fazer logon no [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Selecione o programa que contém o ambiente AEM as a Cloud Service a ser depurado
1. Abra o [Developer Console](./developer-console.md) correspondente ao ambiente do AEM as a Cloud Service para depurar
1. Selecione a guia __Navegador do repositório__
1. Selecione a camada de serviço AEM para procurar
   + Todos os autores
   + Todos os editores
   + Todas as Visualizações
1. Selecione __Abrir Navegador do Repositório__

O Navegador do Repositório é aberto para a camada de serviço selecionada (Autor, Publish ou Pré-visualização) em um modo somente leitura, exibindo recursos e propriedades aos quais seu usuário tem acesso.

## Acesso ao Publish e à Pré-visualização

Por padrão, o acesso ao Publish ou Preview é limitado, reduzindo os recursos disponíveis no Navegador do repositório. [Para exibir todos os recursos no Publish (ou na Pré-visualização), adicione usuários a uma função de Administradores do Publish (ou de Pré-visualização).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
