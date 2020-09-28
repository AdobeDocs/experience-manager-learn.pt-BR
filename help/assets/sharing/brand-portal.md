---
title: Uso do Brand Portal
seo-title: Uso do Brand Portal com a AEM Assets
description: Vídeo de apresentação da integração do autor de AEM e do Portal de marcas da AEM Assets.
seo-description: Vídeo de apresentação da integração do autor de AEM e do Portal de marcas da AEM Assets.
feature: brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions, administration
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
team: tm
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1788'
ht-degree: 1%

---


# Uso do Brand Portal com a AEM Assets{#using-brand-portal-with-aem-assets}

Guias de vídeo da integração do Portal de marcas dos ativos Adobe Experience Manager (AEM).

## Recursos e melhorias do Brand Portal, setembro de 2019

O Brand Portal, de setembro de 2019, apresenta a Fonte de ativos, que aumenta a velocidade do conteúdo e permite uma troca fácil e rápida de ativos entre autores de Experience Manager e criativos e contribuidores de terceiros.

### Seleção de recursos do Portal de marcas{#asset-sourcing}

A Seleção de ativos do Portal de marcas é usada para coletar ativos de agências e equipes de terceiros, sincronizando-os perfeitamente com o Autor do Experience Manager para revisão e uso.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*O Experience Manager Author 6.5 SP2 (6.5.2) ou superior é necessário para usar a Origem de ativos*

Revise [Ativar autor de Experience Manager para seleção](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) de recursos para obter instruções sobre como configurar e configurar a seleção de fonte de ativos no autor de Experience Manager.

## Recursos e melhorias do Brand Portal, fevereiro de 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

A versão de fevereiro de 2019 do Brand Portal foca em melhorias na pesquisa de texto e nas principais solicitações dos clientes.

### Aprimoramentos de pesquisa

O Brand Portal aprimora a pesquisa com pesquisa de texto parcial no predicado de propriedade no painel de filtragem. Para permitir a pesquisa de texto parcial, é necessário ativar a Pesquisa parcial no Predicado de propriedade no formulário de pesquisa.

Leia para saber mais sobre pesquisa de texto parcial e pesquisa de curinga.

#### Pesquisa de frase parcial

Agora você pode pesquisar ativos especificando apenas uma parte (ou seja, uma palavra ou duas) da frase pesquisada no painel de filtragem.

**Caso** de uso: A pesquisa de frases parciais é útil quando você não tem certeza da combinação exata de palavras que ocorrem na frase pesquisada.

Por exemplo, se o formulário de pesquisa no Brand Portal usar o Predicado de propriedade para uma pesquisa parcial sobre o título dos ativos, a especificação do termo campo retornará todos os ativos com o campo de palavras em suas frases de título.

#### Pesquisa curinga

O Brand Portal permite o uso do asterisco (*) no query de pesquisa junto com uma parte da palavra na frase pesquisada.

**Caso** de uso: se você não tiver certeza das palavras exatas que ocorrem na frase pesquisada, poderá usar uma pesquisa curinga para preencher as lacunas no query de pesquisa.

Por exemplo, especificar climb* retorna todos os ativos com palavras que começam com os caracteres que sobem em suas frases de título se o formulário de pesquisa no Portal de marcas usar o Predicado de propriedade para pesquisa parcial no título dos ativos.

Da mesma forma, especificando:

* \*escalb retorna todos os ativos que têm palavras que terminam com caracteres que sobem em suas frases de título.
* \*escalb\* retorna todos os ativos com palavras que incluem os caracteres escalados em suas frases de título.

#### Ativar hierarquia de pastas

Agora, os administradores podem configurar como as pastas são exibidas para usuários não administradores (editores, visualizadores e usuários convidados) no logon.
[A configuração Ativar hierarquia](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) de pastas é adicionada em Configurações gerais, no painel de ferramentas administrativas. Se a configuração for:

* Habilitada, a árvore de pastas que começa na pasta raiz está visível para usuários não administradores. Assim, concedendo a eles uma experiência de navegação semelhante aos administradores.
* Desativado, somente as pastas compartilhadas são exibidas na landing page.

[A funcionalidade Ativar hierarquia](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) de pastas (quando ativada) ajuda a diferenciar as pastas com os mesmos nomes compartilhados de diferentes hierarquias. Ao fazer logon, usuários não administradores agora veem as pastas pai virtual (e ancestral) das pastas compartilhadas.

As pastas compartilhadas são organizadas nos respectivos diretórios em pastas virtuais. É possível reconhecer essas pastas virtuais com um ícone de cadeado.

Observe que a miniatura padrão das pastas virtuais é a imagem em miniatura da primeira pasta compartilhada.

### Suporte para execuções de vídeo do Dynamic Media

Os usuários cuja instância de autor de AEM esteja no modo híbrido Dynamic Media podem pré-visualização e baixar as execuções de mídia dinâmica, além dos arquivos de vídeo originais.

Para permitir a pré-visualização e o download de execuções de mídia dinâmica em contas de locatário específicas, os administradores precisam especificar a Configuração de Dynamic Media (URL do serviço de vídeo (URL do Gateway DM) e a ID de registro para buscar o vídeo dinâmico) na configuração de vídeo no painel de ferramentas administrativas.

Os vídeos do Dynamic Media podem ser visualizados em:

* Página de detalhes do ativo
* Visualização do cartão do ativo
* Página pré-visualização de compartilhamento de links

As codificações de Vídeo do Dynamic Media podem ser baixadas de:

* Brand Portal
* Link compartilhado

### Publicação agendada para o Brand Portal

O fluxo de trabalho de publicação de ativos (e pastas) de [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) Instância do autor para o Brand Portal pode ser agendado para uma data e hora posteriores.

Da mesma forma, os recursos publicados podem ser removidos do portal em uma data posterior (hora), agendando o fluxo de trabalho Cancelar publicação do Brand Portal.

### Alias de locatário configurável no URL

As organizações podem personalizar o URL do portal, tendo um prefixo alternativo no URL. Para obter um alias para o nome do locatário em seu URL de portal existente, as organizações precisam entrar em contato com o suporte ao Adobe.

Observe que somente o prefixo do URL do Portal de Marcas pode ser personalizado e não o URL inteiro.
Por exemplo, uma organização com domínio existente `wknd.brand-portal.adobe.com` pode ser `wkndinc.brand-portal.adobe.com` criada mediante solicitação.

No entanto, a instância do autor de AEM pode ser [configurada](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) somente com o URL de ID do locatário e não com o URL de alias do locatário (alternativo).

**Caso** de uso: As organizações podem atender às suas necessidades de marca ao personalizar o URL do portal, em vez de se manterem no URL fornecido pelo Adobe.

## Recursos e melhorias do Brand Portal, dezembro de 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Acesso de convidado

O portal de marcas AEM permite que os convidados acessem o portal. Um usuário convidado não precisa de credenciais para entrar no portal e pode acessar e baixar todas as pastas públicas e coleções. Os usuários convidados podem adicionar ativos à sua caixa de luz (coleção privada) e baixar o mesmo. Eles também podem visualização previsões de pesquisa e pesquisa de tags inteligentes definidas pelos administradores. A sessão de convidado não permite que os usuários criem coleções e pesquisas salvas ou as compartilhem ainda mais, acessem configurações de pastas e coleções e compartilhem ativos como links.

### Download acelerado

Os usuários do Brand Portal podem aproveitar os downloads rápidos baseados em Aspera para obter velocidades até 25 vezes mais rápidas e desfrutar de uma experiência contínua de download, independentemente de sua localização no mundo inteiro. Para baixar os recursos mais rapidamente do Brand Portal ou do link compartilhado, os usuários precisam selecionar a opção Ativar aceleração de download na caixa de diálogo de download, desde que a aceleração de download esteja ativada em sua organização.

* [Guia para acelerar os downloads do Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### Relatório de logon do usuário

Um novo relatório, para rastrear os logons de usuário, foi introduzido. O relatório Logons de usuário pode ser fundamental para permitir que as organizações façam auditoria e mantenham uma verificação sobre os administradores delegados e outros usuários do Brand Portal.

Os registros do relatório exibem nomes, IDs de e-mail, personas (administrador, visualizador, editor, convidado), grupos, último logon, status de atividade e contagem de logon de cada usuário.

### Acesso às representações originais

Os administradores podem restringir o acesso do usuário aos arquivos de imagem originais (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop ) e dar acesso às renderizações de baixa resolução que eles baixam do Brand Portal ou do link compartilhado. Esse acesso pode ser controlado no nível do grupo de usuários na guia Grupos da página Funções do usuário no painel Ferramentas administrativas.

### Novas configurações

Seis novas configurações são adicionadas aos administradores para ativar/desativar as seguintes funcionalidades em locatários específicos:

* Permitir acesso de convidado
* Permitir que os usuários solicitem acesso ao Brand Portal
* Permitir que os administradores excluam ativos do Brand Portal
* Permitir criação de coleções públicas
* Permitir a criação de coleções inteligentes públicas
* Permitir aceleração de download

### Outras melhorias

* *Caminho da hierarquia de pastas nas visualizações* de cartão e lista — permite que os usuários saibam o local das pastas armazenadas em uma instância do Brand Portal. Ajuda os usuários a diferenciar pastas com o mesmo nome em diferentes hierarquias de pastas.
* *Opção* Visão geral — fornece aos usuários não administradores metadados sobre o ativo/pasta selecionando o ativo/pasta e, em seguida, selecionando a opção de visão geral na barra de ferramentas. Atualmente, exibe o título, a data de criação e o caminho

### Adobe I/O Hosts UI para configurar as integrações de autenticação

O Brand Portal usa a interface [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) do Adobe para criar o aplicativo JWT, que permite a configuração de integrações do oAuth para permitir a integração do AEM Assets com o Brand Portal. Anteriormente, a interface do usuário para configurar integrações OAuth era hospedada em `https://marketing.adobe.com/developer/`. Para saber mais sobre a integração do AEM Assets com o Brand Portal para a publicação de ativos e coleções no Brand Portal, consulte [Configurar a integração do AEM Assets com o Brand Portal](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Recursos e melhorias do Brand Portal, fevereiro de 2018{#brand-portal-features-and-enhancements-632}

Novos recursos aprimorados de funcionalidade voltados para alinhar o Brand Portal com a AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Melhorias na navegação

* Atualização da interface do usuário que se alinha ao AEM e usa a interface do usuário Coral3.
* Acesso rápido e fácil às ferramentas administrativas através do novo logotipo do Adobe.
* Navegação de produto por meio de uma sobreposição
* Navegação rápida para pastas pai de uma pasta secundária.
* Opção Omnisearch para navegar até ferramentas administrativas e conteúdo.
* A Visualização de cartão, a visualização de Lista e a Visualização de colunas ajudam você a navegar facilmente pelas pastas aninhadas.
* A classificação de ativos na Visualização de Listas não está mais restrita ao número de ativos exibidos na tela. Todos os ativos em uma pasta são classificados.

### Melhorias na pesquisa

* O recurso Omnisearch permite que você realize uma pesquisa rápida por ativos e arquivos no Brand Portal.
* O Omnisearch também fornece uma opção para pesquisar ativos em uma pasta ou local específico
* Sugestões de palavras-chave automáticas para facilitar a pesquisa
* Melhore seu Omnisearch com filtros adicionais. Opção para salvar o resultado da pesquisa em uma coleção inteligente para que você visite novamente sua pesquisa mais tarde.
* Suporta pesquisa inteligente de ativos marcados
* AEM ativos marcados inteligentes podem ser compartilhados do AEM para o Brand Portal e usar tags inteligentes para a pesquisa de ativos no Brand Portal.

### Melhorias no compartilhamento de arquivos

* O usuário pode compartilhar um ativo usando a opção de compartilhamento de link.
* Ao compartilhar ativos, o usuário tem que definir uma data de expiração para cada ativo. Fornece aos usuários mais controle sobre os ativos compartilhados.
* Um usuário externo com um link de compartilhamento de ativos pode baixar a imagem e visualização suas propriedades.
* A hierarquia de pastas aninhadas original é preservada para pastas de ativos baixadas.

### Recursos administrativos e de relatórios

* O schema de metadados da AEM Assets agora pode ser publicado do AEM para o Brand Portal.
* Os administradores podem criar e gerenciar três tipos de relatórios: ativos baixados, expirados e publicados
* Capacidade de configurar a coluna que precisa ser incluída no relatório.
* Crie predefinições de imagens para ativos no Portal de marcas.
* Capacidade de modificar o Formulário de painel de pesquisa de administrador ou Pesquisar no Forms para incluir opções de filtragem adicionais.
* Atualizar e pré-visualização wallpaper personalizado para sua marca
* Relatório de uso para saber mais sobre o número de usuários, o espaço de armazenamento usado e o total de ativos.

## Recursos adicionais{#additional-resources}

* [Novidades do Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agentes de replicação do autor de AEM](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guia para download acelerado](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentos do Adobe do Portal de Marcas da AEM Assets](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentos do Adobe do AEM Assets Dynamic Media](https://docs.adobe.com/docs/br/aem/6-3/author/assets/dynamic-media.html)
* [Baixar o Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)