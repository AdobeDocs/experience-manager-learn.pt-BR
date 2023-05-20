---
title: Utilização do Brand Portal
description: Apresentações em vídeo do autor do AEM e integração com o AEM Assets Brand Portal.
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 2%

---

# Utilização do Brand Portal com o AEM Assets{#using-brand-portal-with-aem-assets}

Guias em vídeo da integração do Adobe Experience Manager (AEM) com o Assets Brand Portal.

## Recursos e aprimoramentos do Brand Portal setembro de 2019

Em setembro de 2019, a Brand Portal apresenta com mais destaque a Origem de ativos, que aumenta a velocidade do conteúdo e permite a troca fácil e rápida de ativos entre autores de Experience Manager e criadores e colaboradores de terceiros.

### Origem de ativos do Brand Portal{#asset-sourcing}

A origem de ativos da Brand Portal é usada para coletar ativos de agências e equipes de terceiros, sincronizando-os continuamente com o Autor do Experience Manager para análise e uso.

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*O Experience Manager Author 6.5 SP2 (6.5.2) ou superior é necessário para usar a Origem de ativos*

Revisão [Ativar autor de Experience Manager para origem de ativos](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=pt-BR) para obter instruções sobre como configurar e configurar a Origem de ativos no Experience Manager Author.

## Recursos e aprimoramentos do Brand Portal de fevereiro de 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

A versão de fevereiro de 2019 do Brand Portal se concentra em melhorias na pesquisa de texto e nas solicitações dos principais clientes.

### Aprimoramentos de pesquisa

O Brand Portal aprimora a pesquisa com pesquisa de texto parcial no predicado de propriedade no painel de filtragem. Para permitir a pesquisa de texto parcial, você precisa ativar Pesquisa parcial no Predicado de propriedade no formulário de pesquisa.

Leia para saber mais sobre pesquisa de texto parcial e pesquisa com curinga.

#### Pesquisa de frase parcial

Agora é possível pesquisar ativos especificando apenas uma parte (ou seja, uma ou duas palavras) da frase pesquisada no painel de filtragem.

**Caso de uso** : a pesquisa parcial de frases é útil quando você não tem certeza da combinação exata de palavras que ocorrem na frase pesquisada.

Por exemplo, se o formulário de pesquisa no Brand Portal usar o Predicado de propriedade para pesquisa parcial no título de ativos, especificar o termo camp retornará todos os ativos com a palavra camp na frase de título.

#### Pesquisa com curinga

O Brand Portal permite usar o asterisco (*) na consulta de pesquisa junto com uma parte da palavra na frase pesquisada.

**Caso de uso** :Se não tiver certeza das palavras exatas que ocorrem na frase pesquisada, você poderá usar uma pesquisa com curinga para preencher as lacunas na consulta de pesquisa.

Por exemplo, especificar climb* retorna todos os ativos que têm palavras que começam com os caracteres sobem na frase de título se o formulário de pesquisa no Brand Portal usar o Predicado de propriedade para pesquisa parcial no título de ativos.

Da mesma forma, especificando:

* \*climb retorna todos os ativos que têm palavras que terminam com caracteres que sobem na frase de título.
* \*climb\* retorna todos os ativos que contêm palavras que compreendem os caracteres que sobem na frase de título.

#### Ativar hierarquia de pastas

Agora, os administradores podem configurar como as pastas são exibidas para usuários não administradores (Editores, Visualizadores e Usuários convidados) no logon.
[Ativar hierarquia de pastas](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) A configuração é adicionada em Configurações gerais, no painel ferramentas administrativas. Se a configuração for:

* Ativada, a árvore de pastas que começa na pasta raiz fica visível para usuários não administradores. Assim, concedendo a eles uma experiência de navegação semelhante aos administradores.
* Desativado, somente as pastas compartilhadas são exibidas na landing page.

[Ativar hierarquia de pastas](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) A funcionalidade (quando ativada) ajuda a diferenciar as pastas com os mesmos nomes compartilhados de hierarquias diferentes. Ao fazer logon, os usuários não administradores agora veem as pastas principais virtuais (e ancestrais) das pastas compartilhadas.

As pastas compartilhadas são organizadas nos respectivos diretórios em pastas virtuais. Você pode reconhecer essas pastas virtuais com um ícone de cadeado.

Observe que a miniatura padrão das pastas virtuais é a imagem em miniatura da primeira pasta compartilhada.

### Suporte a representações de vídeo do Dynamic Media

Os usuários cuja instância do Autor do AEM está no modo híbrido do Dynamic Media podem visualizar e baixar as representações do Dynamic Media, além dos arquivos de vídeo originais.

Para permitir a pré-visualização e o download de representações de mídia dinâmica em contas de locatário específicas, os administradores precisam especificar a Configuração do Dynamic Media (URL do serviço de vídeo (URL do gateway DM) e a ID de registro para buscar o vídeo dinâmico) na Configuração de vídeo do painel de ferramentas administrativas.

Os vídeos do Dynamic Media podem ser visualizados em:

* Página de detalhes do ativo
* Exibição de cartão do ativo
* Página de visualização do compartilhamento de link

Os códigos de vídeo do Dynamic Media podem ser baixados de:

* Brand Portal
* Link compartilhado

### Publicação agendada no Brand Portal

Fluxo de trabalho de publicação de ativos (e pastas) a partir do [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) A instância do autor para o Brand Portal pode ser agendada para uma data ou hora posterior.

Da mesma forma, os ativos publicados podem ser removidos do portal em uma data (hora) posterior, agendando o fluxo de trabalho Cancelar publicação da Brand Portal.

### Alias de locatário configurável no URL

As organizações podem personalizar o URL do portal tendo um prefixo alternativo no URL. Para obter um alias para o nome do locatário no URL do portal existente, as organizações precisam entrar em contato com o suporte ao Adobe.

Observe que somente o prefixo do URL do Brand Portal pode ser personalizado, e não o URL inteiro.
Por exemplo, uma organização com um domínio existente `wknd.brand-portal.adobe.com` pode obter `wkndinc.brand-portal.adobe.com` criado mediante solicitação.

No entanto, a instância do autor do AEM pode ser [configurado](https://helpx.adobe.com/br/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) somente com a URL da id do locatário e não com a URL do alias do locatário (alternativo).

**Caso de uso** : as organizações podem atender às suas necessidades de marca, obtendo o URL do portal personalizado, em vez de aderir ao URL fornecido pelo Adobe.

## Recursos e aprimoramentos do Brand Portal dezembro de 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### Acesso de convidado

AEM Brand portal permite o acesso de visitantes ao portal. Um usuário convidado não exige credenciais para entrar no portal e pode acessar e baixar todas as pastas e coleções públicas. Os usuários convidados podem adicionar ativos à caixa de luz (coleção privada) e baixar os mesmos. Eles também podem exibir a pesquisa inteligente de tags e pesquisar predicados definidos pelos administradores. A sessão de convidado não permite que os usuários criem coleções e pesquisas salvas ou as compartilhem ainda mais, acessem configurações de pasta e coleções e compartilhem ativos como links.

### Download acelerado

Os usuários do Brand Portal podem aproveitar os downloads rápidos baseados no Aspera para obter velocidades até 25 vezes mais rápidas e uma experiência de download contínua, independentemente da sua localização no mundo. Para baixar os ativos mais rapidamente do Brand Portal ou do link compartilhado, os usuários precisam selecionar a opção Habilitar aceleração de download na caixa de diálogo de download, desde que a aceleração de download esteja habilitada em sua organização.

* [Guia para acelerar downloads do Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Servidor de teste Aspera Connect](https://test-connect.asperasoft.com/)

### Relatório de logon do usuário

Um novo relatório, para rastrear os logons de usuários, foi introduzido. O relatório Logons de usuário pode ser fundamental para permitir que as organizações auditem e mantenham uma verificação dos administradores delegados e outros usuários do Brand Portal.

Os logs do relatório exibem nomes, IDs de email, personas (administrador, visualizador, editor, convidado), grupos, último logon, status da atividade e contagem de logon de cada usuário.

### Acesso às representações originais

Os administradores podem restringir o acesso do usuário a arquivos de imagem originais (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop) e conceder acesso a representações de baixa resolução que eles baixam do Brand Portal ou de um link compartilhado. Esse acesso pode ser controlado no nível do grupo de usuários na guia Grupos da página Funções do usuário no painel Ferramentas administrativas.

### Novas configurações

Seis novas configurações são adicionadas para que os administradores ativem/desativem as seguintes funcionalidades em locatários específicos:

* Permitir acesso de convidado
* Permitir que os usuários solicitem acesso ao Brand Portal
* Permitir que administradores excluam ativos da Brand Portal
* Permitir criação de coleções públicas
* Permitir criação de coleções públicas inteligentes
* Permitir aceleração de download

### Outras melhorias

* *Caminho de hierarquia de pastas em visualizações de cartão e lista* — permite que os usuários saibam o local das pastas armazenadas em uma instância do Brand Portal. Ajuda os usuários a diferenciar pastas com o mesmo nome em diferentes hierarquias de pastas.
* *Opção de visão geral* — O fornece metadados sobre o ativo/pasta a usuários não administradores, selecionando o ativo/pasta e depois a opção de visão geral na barra de ferramentas. Atualmente, exibe título, data de criação e caminho

### Adobe I/O hospeda a interface do usuário para configurar integrações oAuth

A Brand Portal usa o Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) para criar um aplicativo JWT, que permite configurar integrações oAuth para permitir a integração do AEM Assets com o Brand Portal. Anteriormente, a interface para configurar integrações OAuth era hospedada no `https://marketing.adobe.com/developer/`. Para saber mais sobre a integração do AEM Assets com o Brand Portal para publicação de ativos e coleções no Brand Portal, consulte [Configurar a integração do AEM Assets com o Brand Portal](https://helpx.adobe.com/br/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Recursos e aprimoramentos do Brand Portal de fevereiro de 2018{#brand-portal-features-and-enhancements-632}

Novos recursos com funcionalidade aprimorada voltada para o alinhamento do Brand Portal com o AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### Melhorias na navegação

* Atualização da interface do usuário que se alinha ao AEM e usa a interface do usuário Coral3.
* Acesso rápido e fácil a ferramentas administrativas através do novo logotipo do Adobe.
* Navegação do produto por uma sobreposição
* Navegação rápida para pastas principais de uma pasta secundária.
* Opção Omnisearch para navegar até ferramentas e conteúdo administrativos.
* Exibição de cartão, exibição em lista e exibição em coluna ajudam a navegar facilmente por pastas aninhadas.
* A classificação de ativos na Exibição de lista não está mais restrita ao número de ativos que estão sendo exibidos na tela. Todos os ativos em uma pasta são classificados.

### Melhorias na pesquisa

* O recurso Omnisearch permite realizar uma pesquisa rápida de ativos e arquivos no Brand Portal.
* O Omnisearch também oferece uma opção para pesquisar ativos em uma pasta ou local específico
* Sugestões automáticas de palavras-chave para facilitar a pesquisa
* Melhore seu Omnisearch com filtros adicionais. Opção para salvar o resultado da pesquisa em uma coleção inteligente para que você visite novamente sua pesquisa mais tarde.
* Compatível com pesquisa de ativos com tags inteligentes
* AEM Os ativos com tags inteligentes podem ser compartilhados de AEM para Brand Portal e usar tags inteligentes para pesquisa de ativos no Brand Portal.

### Melhorias no compartilhamento de arquivos

* O usuário pode compartilhar um ativo usando a opção de compartilhamento de link.
* Ao compartilhar ativos, o usuário precisa definir uma data de expiração para cada ativo. Fornece aos usuários mais controle sobre os ativos compartilhados.
* Um usuário externo com o link Compartilhamento de ativos pode baixar a imagem e exibir suas propriedades.
* A hierarquia de pasta aninhada original é preservada para pastas de ativos baixados.

### Recursos administrativos e de relatórios

* O esquema de metadados do AEM Assets agora pode ser publicado do AEM para o Brand Portal.
* Os administradores podem criar e gerenciar três tipos de relatórios: ativos baixados, expirados e publicados
* Capacidade de configurar a coluna que precisa ser incluída no relatório.
* Crie predefinições de imagem para ativos no Brand Portal.
* Capacidade de modificar o Formulário do painel de pesquisa do administrador ou o Forms de pesquisa para incluir opções de filtragem adicionais.
* Atualizar e pré-visualizar papel de parede personalizado para sua marca
* Relatório de uso para saber sobre o número de usuários, o espaço de armazenamento usado e o total de ativos.

## Recursos adicionais{#additional-resources}

* [Novidades do Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/introduction/whats-new.html?lang=pt-BR#introduction)
* [Agentes de replicação do AEM Author](https://helpx.adobe.com/br/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guia para download acelerado](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentação do AEM Assets Brand Portal Adobe](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentação do AEM Assets Dynamic Media Adobe](https://experienceleague.adobe.com/docs/)
* [Baixar Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Servidor de teste Aspera Connect](https://test-connect.asperasoft.com/)
