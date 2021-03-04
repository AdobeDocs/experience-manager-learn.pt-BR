---
title: Uso de ativos do Adobe Stock com AEM Assets
description: 'O AEM fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. As empresas agora podem integrar o plano Adobe Stock Enterprise com o AEM Assets para garantir que os ativos licenciados estejam amplamente disponíveis para seus projetos criativos e de marketing, com os poderosos recursos de gerenciamento de ativos do AEM. '
feature: Adobe Stock
version: 6.4, 6.5
topic: Gerenciamento de conteúdo
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 6%

---


# Uso do Adobe Stock com AEM Assets{#using-adobe-stock-assets-with-aem-assets}

O AEM 6.4.2 fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. As empresas agora podem integrar o plano Adobe Stock Enterprise com o AEM Assets para garantir que os ativos licenciados estejam amplamente disponíveis para seus projetos criativos e de marketing, com os poderosos recursos de gerenciamento de ativos do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>A integração exige um [plano corporativo do Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e o AEM 6.4 com pelo menos o Service Pack 2 implantado. Para obter os detalhes do service pack do AEM 6.4, consulte estas [notas de versão](https://helpx.adobe.com/br/experience-manager/6-4/release-notes/sp-release-notes.html).

A integração entre o Adobe Stock e o AEM Assets permite que autores de conteúdo e profissionais de marketing licenciem e usem facilmente ativos de estoque para fins criativos ou de marketing. Você pode realizar uma pesquisa de ativos no Stock usando a Pesquisa do Omni, adicionando o filtro de localização como Adobe Stock ou navegando pela navegação principal dos ativos AEM e clicando no ícone Pesquisar interface do usuário do Adobe Stock Coral.

## Recursos

### Pesquisar e salvar

* Realize a pesquisa de ativos do Adobe Stock sem sair da área de trabalho do AEM.
* Salve os ativos do Adobe Stock para visualização, sem licenciar o ativo.
* Capacidade de licenciar e salvar ativos do Adobe Stock no AEM Assets
* Capacidade de pesquisar ativos semelhantes do Adobe Stock na interface do usuário do AEM Assets
* Visualizar um ativo selecionado da Pesquisa de ações no AEM Assets no site do Adobe Stock
* Os arquivos de ativos licenciados são marcados com um selo azul licenciado para fácil identificação

### Metadados dos ativos

* O ativo licenciado é armazenado no AEM Assets. As propriedades do ativo contêm metadados de Estoque em uma guia de metadados de ativo separada
* Capacidade de adicionar referências de licença aos metadados do ativo

### Perfil do Asset Stock

* Um usuário pode selecionar o perfil do Adobe Stock em *Usuário > Minhas preferências > Configuração do Stock*
* Referências obrigatórias e opcionais podem ser adicionadas à janela Licenciamento de ativos.
* Capacidade de escolher a preferência de idioma para a janela Licenciamento de ativos com base na região.

### Filtro

* Um usuário pode filtrar ativos de estoque com base em Tipo de ativo, Orientação e Exibir semelhante
* O tipo de ativo inclui Fotos, Ilustrações, Vetores, Vídeos, Modelos, 3D, Premium, Editorial
* A orientação inclui horizontal, vertical e quadrado.
* Exibir filtro semelhante requer o número do arquivo do Adobe Stock

### Controle de acesso

* Os administradores podem fornecer permissões a determinados usuários/grupos para licenciar ativos de estoque ao configurar a configuração do serviço de nuvem do Adobe Stock.
* Se um usuário/grupo específico não tiver permissão para licenciar ativos de estoque, o recurso *Pesquisa de ativos/Licenciamento de ativos* estaria desativado.

## Configurar o Adobe Stock com o AEM Assets{#set-up-adobe-stock-with-aem-assets}

O AEM 6.4.2 fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. Este vídeo aborda uma apresentação rápida de como configurar o Adobe Stocks com o AEM Assets usando o Console de E/S da Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Para a configuração do serviço Adobe Stock Cloud, você deve selecionar o Ambiente de PROD e o caminho do ativo licenciado para /content/dam. O campo Ambiente seria removido na próxima versão do AEM e o caminho do ativo licenciado faz parte de um recurso futuro e o suporte para esse campo será introduzido na próxima versão do AEM.

>[!NOTE]
>
>A integração exige um [plano corporativo do Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e o AEM 6.4 com pelo menos [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) implantado. Para obter os detalhes do service pack do AEM 6.4, consulte estas [notas de versão](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Você também precisaria de permissões de administrador para [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

### Instalação {#installations}

* Para o AEM 6.4, você precisa instalar o [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) e depois reinstalar o arquivo cq-dam-stock-integration-content-1.0.4.zip.
* Verifique se você tem permissões de administrador no [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e no Adobe Experience Manager para configurar a integração.

#### Configurar o Adobe IMS usando o Console do Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Criar uma configuração de conta técnica do Adobe IMS em **Ferramentas > Segurança**
2. Selecione a *Cloud Solution* como *Adobe Stock* e crie um novo certificado ou reutilize um certificado existente para a configuração.
3. Navegue até o Console do Adobe I/O e crie uma nova integração da Conta de Serviço para *Adobe Stock*.
4. Faça upload do certificado da Etapa 2 para a integração da conta do Adobe Stock Service.
5. Escolha a configuração de perfil necessária do Adobe Stock e conclua a integração de serviço.
6. Use os detalhes de integração para concluir a configuração da conta técnica do Adobe IMS
7. Certifique-se de receber o token de acesso usando a conta técnica do Adobe IMS.

![Conta técnica do Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurar os serviços da Adobe Stock Cloud {#set-up-adobe-stock-cloud-services}

1. Crie uma nova configuração do serviço em nuvem para o Adobe Stock em **Ferramentas > Serviços em nuvem.**
2. Selecione a *Configuração do Adobe IMS* criada na seção acima para sua configuração *Adobe Stock Cloud*

3. Certifique-se de selecionar o **AMBIENTE** como PROD. O ambiente de preparo não é suportado e será removido na próxima versão do AEM.
4. **O** caminho do Ativo licenciado pode ser apontado para qualquer diretório em /content/dam. O suporte a recursos desse campo será adicionado na próxima versão do AEM
5. Selecione a localidade e conclua a configuração.
6. Você também pode adicionar usuários/grupos ao seu serviço Adobe Stock Cloud para ativar o acesso de usuários ou grupos específicos.

![Configuração do Adobe Assets Stock](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionais

* [Plano de estoque empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de versão do AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Integrar o AEM e o Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API de integração do console do Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentação da API do Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)