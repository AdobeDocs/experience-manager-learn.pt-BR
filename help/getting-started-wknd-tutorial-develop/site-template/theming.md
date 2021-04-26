---
title: Fluxo de trabalho de temas
seo-title: Introdução ao AEM Sites - Fluxo de trabalho Theming
description: Saiba como atualizar as fontes de tema de um site do Adobe Experience Manager para aplicar estilos específicos de marca. Saiba como usar um servidor proxy para exibir uma pré-visualização ao vivo das atualizações de CSS e Javascript. Este tutorial também abordará como implantar atualizações de temas em um site AEM usando as ações do GitHub.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Componentes principais
topic: Gerenciamento de conteúdo, desenvolvimento
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# Fluxo de trabalho de tema {#theming}

>[!CAUTION]
>
> Os recursos rápidos de criação de sites mostrados aqui serão lançados na segunda metade de 2021. A documentação relacionada está disponível para fins de visualização.

Neste capítulo, atualizaremos as fontes de tema de um site do Adobe Experience Manager para aplicar estilos específicos da marca. Nós aprenderemos a usar um servidor proxy para exibir uma pré-visualização das atualizações de CSS e Javascript enquanto codificamos no site ativo. Este tutorial também abordará como implantar atualizações de temas em um site AEM usando as ações do GitHub.

No final, nosso site será atualizado para incluir estilos para corresponder à marca WKND.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e é pressuposto que as etapas descritas no capítulo [Modelos de página](./page-templates.md) foram concluídas.

## Objetivos

1. Saiba como as fontes de tema de um site podem ser baixadas e modificadas.
1. Saiba mais sobre o código do site ativo para uma visualização em tempo real.
1. Entenda o fluxo de trabalho completo do fornecimento de atualizações de CSS e JavaScript compiladas como parte de um tema usando as ações do GitHub.

## Atualizar um tema {#theme-update}

Em seguida, faça alterações nas fontes de tema para que o site corresponda à aparência da Marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Etapas de alto nível para o vídeo:

1. Crie um usuário local no AEM para uso com um servidor de desenvolvimento de proxy.
1. Baixe as fontes de tema do AEM e abra usando um IDE local, como VSCode.
1. Modifique as fontes de temas e use um servidor dev de proxy para visualizar as alterações de CSS e JavaScript em tempo real.
1. Atualize as fontes de temas para que o artigo da revista corresponda aos estilos e modelos da marca WKND.

### Arquivos de solução

Baixe os estilos concluídos para o [Tema WKND](assets/theming/WKND-THEME-src.zip)

## Implantar um tema {#deploy-theme}

Implante atualizações em um tema em um ambiente de AEM usando as Ações do GitHub.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Etapas de alto nível para o vídeo:

1. Adicione seu projeto de fontes de temas a [GitHub como um novo repositório](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line).
1. Crie [um token de acesso pessoal no GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) e salve em um local seguro.
1. Configure as configurações do GitHub no seu ambiente de AEM para apontar para o repositório GitHub e incluir seu token de acesso pessoal.
1. Crie os seguintes [segredos criptografados](https://docs.github.com/en/actions/reference/encrypted-secrets) no seu Repositório GitHub:
   * **AEM_SITE**  - raiz do seu site AEM (ou seja,  `wknd`)
   * **AEM_URL**  - url do ambiente de criação do AEM
   * **GIT_TOKEN**  - Token de acesso pessoal da Etapa 2.
1. Execute a ação do GitHub: **Crie e implante artefatos do Github**. Isso terá o efeito downstream de executar a ação: **Atualize a configuração de tema no AEM com a id de artefato**, que implantará as fontes de tema no ambiente AEM conforme especificado por `AEM_URL` e `AEM_SITE`.

### Exemplo de acordos de recompra

Há alguns exemplos de acordos de recompra do GitHub que podem ser usados como referência:

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  - Usado como exemplo para projetos &quot;do mundo real&quot;.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  - Usado como exemplo para aqueles que seguem o tutorial.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar e atualizou um tema para AEM!

### Próximas etapas {#next-steps}

Faça um mergulho mais profundo no desenvolvimento AEM e entenda mais da tecnologia subjacente criando um site usando o [AEM Arquétipo de projeto](../project-archetype/overview.md).
