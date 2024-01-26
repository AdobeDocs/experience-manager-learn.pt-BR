---
title: Uso do Adobe Stock Assets com o AEM Assets
description: O AEM fornece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. Agora, as organizações podem integrar seu plano Adobe Stock Enterprise à AEM Assets para garantir que os ativos licenciados estejam disponíveis amplamente para seus projetos criativos e de marketing, com os poderosos recursos de gerenciamento de ativos do AEM.
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1130
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 1%

---

# Utilização do Adobe Stock com o AEM Assets{#using-adobe-stock-assets-with-aem-assets}

O AEM 6.4.2 oferece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. Agora, as organizações podem integrar seu plano Adobe Stock Enterprise à AEM Assets para garantir que os ativos licenciados estejam disponíveis amplamente para seus projetos criativos e de marketing, com os poderosos recursos de gerenciamento de ativos do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>A integração exige uma [plano Adobe Stock corporativo](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 com pelo menos o Service Pack 2 implantado. Para obter detalhes sobre o service pack do AEM 6.4, consulte estes [notas de versão](https://helpx.adobe.com/br/experience-manager/6-4/release-notes/sp-release-notes.html).

A integração do Adobe Stock e do AEM Assets permite que os autores de conteúdo e profissionais de marketing licenciem e usem facilmente ativos de estoque para fins criativos ou de marketing. Você pode executar uma pesquisa de ativo do Stock usando a Pesquisa Omni, adicionando o filtro de localização como Adobe Stock ou navegando pela navegação principal do AEM Assets e clicando no ícone Pesquisar interface do usuário Adobe Stock Coral.

## Recursos

### Pesquisar e salvar

* Realizar a pesquisa de ativos do Adobe Stock sem sair do espaço de trabalho do AEM.
* Salvar ativos do Adobe Stock para visualização, sem licenciar o ativo.
* Capacidade de licenciar e salvar ativos do Adobe Stock no AEM Assets
* Capacidade de pesquisar ativos semelhantes no Adobe Stock na interface do usuário do AEM Assets
* Exibir um ativo selecionado da Pesquisa do Stock no AEM Assets no site do Adobe Stock
* Os arquivos de ativos licenciados são marcados com um selo azul licenciado para facilitar a identificação

### Metadados dos ativos

* O ativo licenciado é armazenado no AEM Assets. As propriedades do ativo contêm metadados do Stock em uma guia de metadados do ativo separada
* Capacidade de adicionar referências de licença aos metadados de ativos

### Perfil do Asset Stock

* Um usuário pode selecionar o perfil do Adobe Stock em *Usuário > Minhas preferências > Configuração do Stock*
* As referências obrigatórias e opcionais podem ser adicionadas à janela de licenciamento de ativos.
* Capacidade de escolher a preferência de idioma para a janela Licenciamento de ativo com base na região.

### Filtro

* Um usuário pode filtrar ativos de estoque com base no Tipo de ativo, Orientação e Visualização semelhante
* O tipo de ativo inclui Fotos, Ilustrações, Vetores, Vídeos, Modelos, 3D, Premium, Editorial
* A orientação inclui Horizontal, Vertical e Quadrado.
* O filtro Exibir semelhante requer o número de arquivo do Adobe Stock

### Controle de acesso

* Os administradores podem fornecer permissões a determinados usuários/grupos para licenciar ativos de estoque ao definir a configuração do Adobe Stock Cloud Service.
* Se um usuário/grupo específico não tiver permissão para licenciar ativos de estoque, *Pesquisa de ativos do Stock/Licenciamento de ativos* O recurso seria desativado.

## Configurar o Adobe Stock com o AEM Assets{#set-up-adobe-stock-with-aem-assets}

O AEM 6.4.2 oferece aos usuários a capacidade de pesquisar, visualizar, salvar e licenciar ativos do Adobe Stock diretamente do AEM. Este vídeo aborda as instruções rápidas sobre como configurar o Adobe Stocks com o AEM Assets usando o Console do Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Para a configuração do serviço da Adobe Stock Cloud, você deve selecionar o ambiente de produção e o caminho do ativo licenciado para `/content/dam`. O campo Ambiente agora é removido no AEM.

>[!NOTE]
>
>A integração exige uma [plano Adobe Stock corporativo](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 com pelo menos [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) implantado. Para obter detalhes sobre o service pack do AEM 6.4, consulte estes [notas de versão](https://helpx.adobe.com/br/experience-manager/6-4/release-notes/sp-release-notes.html). Você também precisaria de permissões de administrador para [Console Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

### Instalação {#installations}

* Para o AEM 6.4, é necessário instalar o [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) e reinstale o arquivo cq-dam-stock-integration-content-1.0.4.zip.
* Verifique se você tem permissões de administrador no [Console Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager para configurar a integração.

#### Defina a configuração do Adobe IMS usando o console de Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Crie uma configuração de conta técnica do Adobe IMS em **Ferramentas > Segurança**
2. Selecione o *Solução em nuvem* as *Adobe Stock* e criar um novo certificado ou reutilizar um certificado existente para a configuração.
3. Acesse o Console do Adobe I/O e crie uma nova integração de Conta de serviço para *Adobe Stock*.
4. Faça upload do certificado da Etapa 2 para a integração da Conta de serviço da Adobe Stock.
5. Escolha a configuração de perfil do Adobe Stock necessária e conclua a integração de serviço.
6. Use os detalhes de integração para concluir a configuração da conta técnica do Adobe IMS
7. Verifique se você pode receber o token de acesso usando a conta técnica do Adobe IMS.

![Conta técnica do Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurar o Adobe Stock Cloud Service {#set-up-adobe-stock-cloud-services}

1. Crie uma nova configuração do Cloud Service para o Adobe Stock em **Ferramentas > Cloud Service.**
2. Selecione o *Configuração do Adobe IMS* criado na seção acima para o seu *Adobe Stock Cloud* configuração

3. Selecione o **AMBIENTE** como PROD.
4. **Caminho do ativo licenciado** pode ser apontado para qualquer diretório em `/content/dam`.
5. Selecione o local e conclua a configuração.
6. Você também pode adicionar usuários/grupos ao serviço na nuvem do Adobe Stock para habilitar o acesso de usuários ou grupos específicos.

![Configuração do estoque de ativos Adobe](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionais

* [Enterprise Stock Plan](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de versão do AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=pt-BR)
* [Integrar AEM e Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [API de integração do console do Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentação da API do Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)
