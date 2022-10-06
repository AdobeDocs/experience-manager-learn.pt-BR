---
title: Uso do Adobe Stock Assets com a AEM Assets
description: O AEM fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos da Adobe Stock diretamente da AEM. As empresas agora podem integrar o plano Adobe Stock Enterprise com a AEM Assets para garantir que os ativos licenciados estejam amplamente disponíveis para seus projetos criativos e de marketing, com os poderosos recursos de gerenciamento de ativos da AEM.
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 3%

---

# Uso do Adobe Stock com AEM Assets{#using-adobe-stock-assets-with-aem-assets}

O AEM 6.4.2 fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. As empresas agora podem integrar o plano Adobe Stock Enterprise com a AEM Assets para garantir que os ativos licenciados estejam amplamente disponíveis para seus projetos criativos e de marketing, com os poderosos recursos de gerenciamento de ativos da AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>A integração exige um [plano corporativo Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 com pelo menos o Service Pack 2 implantado. Para obter AEM detalhes do service pack 6.4, consulte estes [notas de versão](https://helpx.adobe.com/br/experience-manager/6-4/release-notes/sp-release-notes.html).

A integração Adobe Stock e AEM Assets permite que autores e profissionais de marketing de conteúdo licenciem e usem facilmente ativos de estoque para fins criativos ou de marketing. Você pode realizar uma pesquisa de ativos no Stock usando a Pesquisa do Omni, adicionando o filtro de localização como Adobe Stock ou navegando pela navegação principal do AEM Assets e clicando no ícone Pesquisar interface do usuário do Coral do Adobe Stock .

## Recursos

### Pesquisar e salvar

* Realize a pesquisa de ativos do Adobe Stock sem sair AEM espaço de trabalho.
* Salve os ativos do Adobe Stock para visualização, sem licenciar o ativo.
* Capacidade de licenciar e salvar ativos Adobe Stock no AEM Assets
* Capacidade de pesquisar ativos semelhantes da Adobe Stock na interface do usuário do AEM Assets
* Visualizar um ativo selecionado da Pesquisa de ações no AEM Assets no site da Adobe Stock
* Os arquivos de ativos licenciados são marcados com um selo azul licenciado para fácil identificação

### Metadados dos ativos

* O ativo licenciado é armazenado no AEM Assets. As propriedades do ativo contêm metadados de Estoque em uma guia de metadados de ativo separada
* Capacidade de adicionar referências de licença aos metadados do ativo

### Perfil do Asset Stock

* Um usuário pode selecionar o perfil do Adobe Stock em *Usuário > Minhas preferências > Configuração de estoque*
* Referências obrigatórias e opcionais podem ser adicionadas à janela Licenciamento de ativos.
* Capacidade de escolher a preferência de idioma para a janela Licenciamento de ativos com base na região.

### Filtro

* Um usuário pode filtrar ativos de estoque com base em Tipo de ativo, Orientação e Exibir semelhante
* O tipo de ativo inclui Fotos, Ilustrações, Vetores, Vídeos, Modelos, 3D, Premium, Editorial
* A orientação inclui horizontal, vertical e quadrado.
* Exibir filtro semelhante requer o número do arquivo Adobe Stock

### Controle de acesso

* Os administradores podem fornecer permissões a determinados usuários/grupos para licenciar ativos de estoque ao configurar a configuração do Adobe Stock Cloud Service.
* Se um usuário/grupo específico não tiver permissão para licenciar ativos de estoque, *Pesquisa de ativos de estoque/Licenciamento de ativos* O recurso estaria desativado.

## Configurar o Adobe Stock com AEM Assets{#set-up-adobe-stock-with-aem-assets}

O AEM 6.4.2 fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. Este vídeo aborda a rápida apresentação de como configurar o Adobe Stocks com o AEM Assets usando o Adobe I/O Console.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Para a configuração do serviço da Adobe Stock Cloud, é necessário selecionar o ambiente de produção e o caminho do ativo licenciado para `/content/dam`. O campo Environment agora é removido em AEM.

>[!NOTE]
>
>A integração exige um [plano corporativo Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 com, pelo menos, [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) implantado. Para obter AEM detalhes do service pack 6.4, consulte estes [notas de versão](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Você também precisaria de permissões de administrador para [Console Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

### Instalação {#installations}

* Para o AEM 6.4, é necessário instalar o [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) e reinstale o arquivo cq-dam-stock-integration-content-1.0.4.zip.
* Verifique se você tem permissões de administrador no [Console Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

#### Configurar o Adobe IMS usando o console Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Criar uma configuração de conta técnica do Adobe IMS em **Ferramentas > Segurança**
2. Selecione o *Solução da nuvem* as *Adobe Stock* e criar um novo certificado ou reutilizar um certificado existente para a configuração.
3. Navegue até o Console do Adobe I/O e crie uma nova integração da Conta de Serviço para *Adobe Stock*.
4. Faça upload do certificado da Etapa 2 para a integração da sua conta de serviço do Adobe Stock.
5. Escolha a configuração de perfil do Adobe Stock necessária e conclua a integração de serviço.
6. Use os detalhes de integração para concluir a configuração da conta técnica do Adobe IMS
7. Certifique-se de receber o token de acesso usando a conta técnica do Adobe IMS.

![Conta técnica do Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurar Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services}

1. Criar uma nova configuração do serviço de nuvem para o Adobe Stock em **Ferramentas > Cloud Services.**
2. Selecione o *Configuração do Adobe IMS* criado na seção acima para *Adobe Stock Cloud* configuração

3. Certifique-se de selecionar a variável **AMBIENTE** como PROD.
4. **Caminho do ativo licenciado** pode ser apontado para qualquer diretório em `/content/dam`.
5. Selecione a localidade e conclua a configuração.
6. Você também pode adicionar usuários/grupos ao seu serviço da Adobe Stock Cloud para ativar o acesso de usuários ou grupos específicos.

![Configuração do Adobe Assets Stock](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionais

* [Plano de estoque empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de versão do AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=pt-BR)
* [Integrar AEM e Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [API de integração do console do Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentação da API do Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)
