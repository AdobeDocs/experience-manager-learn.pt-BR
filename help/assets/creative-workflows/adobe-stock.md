---
title: Uso de ativos da Adobe Stock com a AEM Assets
description: 'AEM oferece aos usuários a capacidade de pesquisar, pré-visualização, salvar e licenciar ativos da Adobe Stock diretamente da AEM. As organizações agora podem integrar seu plano Adobe Stock Enterprise com a AEM Assets para garantir que os ativos licenciados estejam amplamente disponíveis para seus projetos de criação e marketing, com os poderosos recursos de gerenciamento de ativos da AEM. '
feature: creative-cloud-integration
topics: authoring, collaboration, operations, sharing, metadata, images, stock
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 6%

---


# Uso do Adobe Stock com AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 fornece aos usuários a capacidade de pesquisar, pré-visualização, salvar e licenciar ativos da Adobe Stock diretamente da AEM. As organizações agora podem integrar seu plano Adobe Stock Enterprise com a AEM Assets para garantir que os ativos licenciados estejam amplamente disponíveis para seus projetos de criação e marketing, com os poderosos recursos de gerenciamento de ativos da AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>A integração exige um [plano Adobe Stock corporativo](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 com pelo menos o Service Pack 2 implantado. Para obter AEM detalhes do service pack 6.4, consulte estas [notas de versão](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html).

A integração entre a Adobe Stock e a AEM Assets permite que autores e comerciantes de conteúdo licenciem e usem facilmente ativos de ações para fins criativos ou de marketing. Você pode realizar uma pesquisa de ativos do Stock usando o Omni Search, adicionando o filtro de localização como Adobe Stock ou navegando pela navegação principal do AEM Assets e clicando no ícone Pesquisar interface do usuário do Adobe Stock Coral.

## Recursos

### Pesquisar e salvar

* Execute a pesquisa de ativos Adobe Stock sem sair AEM espaço de trabalho.
* Salve os ativos da Adobe Stock para pré-visualização, sem licenciar o ativo.
* Capacidade de licenciar e salvar ativos Adobe Stock na AEM Assets
* Capacidade de pesquisar ativos similares da Adobe Stock na interface do usuário do AEM Assets
* Visualização de um ativo selecionado da Pesquisa de ações no AEM Assets no site da Adobe Stock
* Os arquivos de ativos licenciados são marcados com um selo azul licenciado para fácil identificação

### Metadados dos ativos

* O ativo licenciado é armazenado na AEM Assets. As propriedades do ativo contêm metadados do Stock em uma guia separada de metadados do ativo
* Capacidade de adicionar referências de licença aos metadados do ativo

### Perfil do Asset Stock

* Um usuário pode selecionar perfil Adobe Stock em *Usuário > Minhas preferências > Configuração de ações*
* Referências obrigatórias e opcionais podem ser adicionadas à janela Licenciamento de ativos.
* Capacidade de escolher a preferência de idioma para a janela Licenciamento de ativos com base na região.

### Filtro

* Um usuário pode filtrar ativos de ações com base em Tipo de ativo, Orientação e Visualização similar
* O tipo de ativo inclui Fotos, Ilustrações, Vetores, Vídeos, Modelos, 3D, Premium, Editorial
* A orientação inclui horizontal, vertical e quadrada.
* O filtro visualização semelhante requer o número do arquivo Adobe Stock

### Controle de acesso

* Os administradores podem fornecer permissões a determinados usuários/grupos para licenciar ativos de ações ao configurar a configuração do serviço de nuvem da Adobe Stock.
* Se um usuário/grupo específico não tiver permissão para licenciar ativos de ações, o recurso *Pesquisa de ativos de ações / Licenciamento de ativos* será desativado.

## Configurar o Adobe Stock com o AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 fornece aos usuários a capacidade de pesquisar, pré-visualização, salvar e licenciar ativos da Adobe Stock diretamente da AEM. Este vídeo aborda uma rápida apresentação de como configurar o Adobe Stocks com o AEM Assets usando o Adobe I/O Console.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Para a configuração do serviço Adobe Stock Cloud, é necessário selecionar o Ambiente PROD e o caminho do ativo licenciado como /content/dam. O campo ambiente seria removido na próxima versão AEM e o caminho do ativo licenciado faz parte de um recurso futuro e o suporte para esse campo será introduzido na próxima AEM.

>[!NOTE]
>
>A integração requer um [plano Adobe Stock corporativo](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 com pelo menos [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) implantado. Para obter AEM detalhes do service pack 6.4, consulte estas [notas de versão](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Você também precisaria de permissões de administrador para [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

### Instalação {#installations}

* Para o AEM 6.4, é necessário instalar o [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) e reinstalar o arquivo cq-dam-stock-integration-content-1.0.4.zip.
* Verifique se você tem permissões de administrador em [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

#### Configurar a configuração do Adobe IMS usando o Adobe I/O Console {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Criar uma configuração de conta técnica Adobe IMS em **Ferramentas > Segurança**
2. Selecione *Solução em nuvem* como *Adobe Stock* e crie um novo certificado ou reutilize um certificado existente para a configuração.
3. Navegue até o Adobe I/O Console e crie uma nova integração de Conta de Serviço para *Adobe Stock*.
4. Faça upload do certificado da Etapa 2 para a integração da sua Conta de serviço da Adobe Stock.
5. Escolha a configuração necessária do perfil Adobe Stock e conclua a integração do serviço.
6. Use os detalhes de integração para concluir a configuração da conta técnica Adobe IMS
7. Verifique se você pode receber o token de acesso usando a Conta técnica Adobe IMS.

![Conta técnica do Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurar Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services}

1. Crie uma nova configuração de serviço em nuvem para Adobe Stock em **Ferramentas > Cloud Services.**
2. Selecione a *Configuração Adobe IMS* criada na seção acima para a configuração *Adobe Stock Cloud*

3. Certifique-se de selecionar **AMBIENTE** como PROD. O ambiente de preparo não é suportado e será removido na próxima versão do AEM.
4. **O** caminho de ativo licenciado pode ser apontado para qualquer diretório em /content/dam. O suporte a recursos para este campo será adicionado na próxima versão do AEM
5. Selecione a localidade e conclua a configuração.
6. Você também pode adicionar usuários/grupos ao serviço da Adobe Stock Cloud para permitir o acesso de usuários ou grupos específicos.

![Configuração de ações do Adobe Assets](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionais

* [Plano de ações da empresa](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de versão do AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Integrar AEM e Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API de integração do Adobe I/O Console](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentos da API do Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)