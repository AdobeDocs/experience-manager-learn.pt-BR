---
title: Uso do Brand Portal
description: Apresentação de vídeo da integração do AEM Author e do AEM Assets Brand Portal.
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 2%

---


# Uso do Brand Portal com AEM Assets{#using-brand-portal-with-aem-assets}

Guias de vídeo da integração do Adobe Experience Manager (AEM) Assets Brand Portal.

## Recursos e aprimoramentos de setembro de 2019 do Brand Portal

A Brand Portal de setembro de 2019 apresenta a origem dos ativos, que aumenta a velocidade do conteúdo e permite uma troca fácil e rápida de ativos entre autores de Experience Manager e criativos e contribuidores de terceiros.

### Origem de ativos do Brand Portal{#asset-sourcing}

A origem dos ativos da Brand Portal é usada para coletar ativos de agências e equipes de terceiros, sincronizando-os perfeitamente com o autor do Experience Manager para revisão e uso.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*O Autor do Experience Manager 6.5 SP2 (6.5.2) ou superior é necessário para usar a origem dos ativos*

Revise [Ativar autor do Experience Manager para origem de ativos](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=en) para obter instruções sobre como configurar e configurar a origem de ativos no autor do Experience Manager.

## Recursos e aprimoramentos de fevereiro de 2019 do Brand Portal{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

A versão de fevereiro de 2019 do Brand Portal se concentra em melhorias na pesquisa de texto e nas principais solicitações de clientes.

### Melhorias de pesquisa

O Brand Portal aprimora a pesquisa com pesquisa de texto parcial no predicado de propriedade no painel de filtragem. Para permitir a pesquisa de texto parcial, é necessário ativar a Pesquisa parcial no Predicado de propriedade no formulário de pesquisa.

Leia para saber mais sobre pesquisa de texto parcial e pesquisa curinga.

#### Pesquisa de frase parcial

Agora é possível pesquisar ativos especificando apenas uma parte, ou seja, uma palavra ou duas, da frase pesquisada no painel de filtragem.

**Caso**  de uso: A pesquisa de frase parcial é útil quando você não tem certeza da combinação exata de palavras que ocorrem na frase pesquisada.

Por exemplo, se o formulário de pesquisa no Brand Portal usar o Predicado de propriedade para pesquisa parcial no título do ativo, a especificação do termo acampamento retornará todos os ativos com o campo de palavras na frase de título.

#### Pesquisa curinga

O Brand Portal permite usar o asterisco (*) na consulta de pesquisa, juntamente com uma parte da palavra na frase pesquisada.

**Caso de uso** : se você não tiver certeza das palavras exatas que ocorrem na frase pesquisada, poderá usar uma pesquisa curinga para preencher as lacunas em sua consulta de pesquisa.

Por exemplo, especificar climb* retorna todos os ativos com palavras que começam com os caracteres escalados em sua frase de título, se o formulário de pesquisa no Brand Portal usar Predicado de propriedade para pesquisa parcial no título dos ativos.

Da mesma forma, especificando:

* \*climb retorna todos os ativos que têm palavras que terminam com caracteres que sobem em sua frase de título.
* \*climb\* retorna todos os ativos com palavras que incluem os caracteres que subem em sua frase de título.

#### Ativar hierarquia de pastas

Agora, os administradores podem configurar como as pastas são exibidas para usuários não administradores (editores, visualizadores e usuários convidados) no logon.
[A configuração Ativar ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) hierarquia de pastas é adicionada em Configurações gerais, no painel Ferramentas administrativas. Se a configuração for:

* Ativada, a árvore de pastas que começa na pasta raiz está visível para usuários não administradores. Dessa forma, a concessão de uma experiência de navegação semelhante aos administradores.
* Desabilitado, somente as pastas compartilhadas são exibidas na landing page.

[A funcionalidade Ativar ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) hierarquia de pastas (quando ativada) ajuda a diferenciar as pastas com os mesmos nomes compartilhados de diferentes hierarquias. Ao fazer logon, usuários não administradores agora visualizam as pastas pai virtual (e ancestral) das pastas compartilhadas.

As pastas compartilhadas são organizadas nos respectivos diretórios em pastas virtuais. Você pode reconhecer essas pastas virtuais com um ícone de cadeado.

Observe que a miniatura padrão das pastas virtuais é a imagem em miniatura da primeira pasta compartilhada.

### Suporte para representações de vídeo do Dynamic Media

Os usuários cuja instância do autor do AEM esteja no modo híbrido do Dynamic Media podem visualizar e baixar as representações de mídia dinâmica, além dos arquivos de vídeo originais.

Para permitir a pré-visualização e o download de representações de mídia dinâmica em contas de locatários específicas, os administradores precisam especificar a Configuração do Dynamic Media (URL do serviço de vídeo (URL do gateway do DM) e a ID de registro para buscar o vídeo dinâmico) na configuração de vídeo no painel Ferramentas administrativas.

Os vídeos do Dynamic Media podem ser visualizados em:

* Página Detalhes do ativo
* Exibição de cartão do ativo
* Página de visualização de compartilhamento de link

É possível baixar os códigos de vídeo do Dynamic Media em:

* Brand Portal
* Link compartilhado

### Publicação agendada para o Brand Portal

Fluxo de trabalho de publicação de ativos (e pastas) de [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) A instância do autor para o Brand Portal pode ser agendada para uma data e hora posteriores.

Da mesma forma, os ativos publicados podem ser removidos do portal em uma data posterior (hora), agendando o fluxo de trabalho Cancelar publicação do Brand Portal .

### Alias do locatário configurável no URL

As organizações podem personalizar o URL do portal, com um prefixo alternativo no URL. Para obter um alias para o nome do locatário em seu URL de portal existente, as organizações precisam entrar em contato com o suporte ao Adobe.

Observe que somente o prefixo do URL do Brand Portal pode ser personalizado e não o URL inteiro.
Por exemplo, uma organização com o domínio existente `wknd.brand-portal.adobe.com` pode ser criada `wkndinc.brand-portal.adobe.com` mediante solicitação.

No entanto, a instância do autor do AEM pode ser [configurada](https://helpx.adobe.com/br/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) somente com o URL da ID do locatário e não com o URL do alias do locatário (alternativo).

**Caso**  de uso: As organizações podem atender às suas necessidades de marca, personalizando o URL do portal, em vez de aderir ao URL fornecido pelo Adobe.

## Recursos e aprimoramentos de dezembro de 2018 do Brand Portal{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Acesso de convidado

AEM Portal da Marca permite acesso de convidado ao portal. Um usuário convidado não precisa de credenciais para entrar no portal e pode acessar e baixar todas as pastas públicas e coleções. Os usuários convidados podem adicionar ativos a sua caixa de entrada (coleção privada) e baixar os mesmos. Eles também podem visualizar a pesquisa de tags inteligentes e os predicados de pesquisa definidos pelos administradores. A sessão de convidado não permite que os usuários criem coleções e pesquisas salvas ou as compartilhe mais, acesse as configurações de pastas e coleções e compartilhe ativos como links.

### Download acelerado

Os usuários da Brand Portal podem aproveitar os downloads rápidos baseados em Aspera para obter velocidades até 25 vezes mais rápidas e desfrutar de uma experiência de download contínua, independentemente de sua localização em todo o mundo. Para baixar os ativos mais rapidamente do Brand Portal ou do link compartilhado, os usuários precisam selecionar a opção Habilitar aceleração de download na caixa de diálogo de download, desde que a aceleração de download esteja ativada em sua organização.

* [Guia para acelerar downloads no Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Assimilar servidor de teste do Connect](https://test-connect.asperasoft.com/)

### Relatório de logon do usuário

Um novo relatório, para rastrear os logons de usuários, foi introduzido. O relatório Logons de usuário pode ser fundamental para permitir que as organizações auditem e mantenham uma verificação sobre os administradores delegados e outros usuários do Brand Portal.

Os logs de relatório exibem nomes, IDs de email, personas (administrador, visualizador, editor, convidado), grupos, último logon, status da atividade e contagem de logon de cada usuário.

### Acesso às representações originais

Os administradores podem restringir o acesso do usuário aos arquivos de imagem originais (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop e conceder acesso a representações de baixa resolução baixadas pelo Brand Portal ou pelo link compartilhado. Esse acesso pode ser controlado no nível do grupo de usuários na guia Grupos da página Funções do usuário no painel Ferramentas administrativas.

### Novas configurações

Seis novas configurações são adicionadas aos administradores para ativar/desativar as seguintes funcionalidades em locatários específicos:

* Permitir acesso de convidado
* Permitir que usuários solicitem acesso ao Brand Portal
* Permitir que administradores excluam ativos do Brand Portal
* Permitir criação de coleções públicas
* Permitir a criação de coleções inteligentes públicas
* Permitir aceleração de download

### Outras melhorias

* *Caminho da hierarquia de pastas nas visualizações*  de cartão e lista — permite que os usuários saibam o local das pastas armazenadas em uma instância do Brand Portal. Ajuda os usuários a diferenciar pastas com o mesmo nome em diferentes hierarquias de pastas.
* *Opção de visão geral*  — fornece metadados de usuários não administradores sobre o ativo/pasta, selecionando o ativo/pasta e depois a opção de visão geral na barra de ferramentas. Atualmente, exibe o título, a data de criação e o caminho

### Adobe I/O Hosts UI para configurar as Integrações de oAuth

O Brand Portal usa a interface Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) para criar o aplicativo JWT, que permite a configuração de integrações oAuth para permitir a integração do AEM Assets com o Brand Portal. Anteriormente, a interface do usuário para configurar integrações OAuth era hospedada em `https://marketing.adobe.com/developer/`. Para saber mais sobre a integração do AEM Assets com o Brand Portal para publicação de ativos e coleções no Brand Portal, consulte [Configurar integração do AEM Assets com o Brand Portal](https://helpx.adobe.com/br/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Recursos e aprimoramentos de fevereiro de 2018 do Brand Portal{#brand-portal-features-and-enhancements-632}

Novos recursos aprimorados voltados para o alinhamento do Brand Portal com o AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Melhorias na navegação

* Atualização da interface do usuário que se alinha ao AEM e usa a interface do usuário do Coral3.
* Acesso rápido e fácil às ferramentas administrativas através do novo logotipo do Adobe.
* Navegação do produto por meio de uma sobreposição
* Navegação rápida para pastas pai de uma pasta filho.
* Opção Omnisearch para navegar até ferramentas administrativas e conteúdo.
* A Exibição de cartão, a Exibição de lista e a Exibição de coluna ajudam você a navegar facilmente por pastas aninhadas.
* A classificação de ativos na Exibição de lista não está mais restrita ao número de ativos que estão sendo exibidos na tela. Todos os ativos em uma pasta são classificados.

### Melhorias na pesquisa

* O recurso Omnisearch permite realizar uma pesquisa rápida de ativos e arquivos no Brand Portal.
* O Omnisearch também fornece uma opção para procurar ativos em uma pasta ou local específico
* Sugestões de palavras-chave automáticas para facilitar a pesquisa
* Melhore o Omnisearch com filtros adicionais. Opção para salvar o resultado da pesquisa em uma coleção inteligente para que você visite novamente sua pesquisa posteriormente.
* Suporta pesquisa de ativos com tags inteligentes
* AEM Os ativos marcados inteligentes podem ser compartilhados do AEM para a Brand Portal e usar tags inteligentes para pesquisa de ativos no Brand Portal.

### Melhorias no compartilhamento de arquivos

* O usuário pode compartilhar um ativo usando a opção de compartilhamento de link.
* Ao compartilhar ativos, o usuário pode definir uma data de expiração para cada ativo. Fornece aos usuários mais controle sobre os ativos compartilhados.
* Um usuário externo com o link de Compartilhamento de ativos pode baixar a imagem e exibir suas propriedades.
* A hierarquia de pasta aninhada original é preservada para pastas de ativos baixadas.

### Relatórios e recursos administrativos

* O esquema de metadados do AEM Assets agora pode ser publicado do AEM para o Brand Portal.
* Os administradores podem criar e gerenciar três tipos de relatórios: ativos baixados, expirados e publicados
* Capacidade de configurar a coluna que precisa ser incluída no relatório.
* Criar predefinições de imagens para ativos no Brand Portal.
* Capacidade de modificar o Formulário de painel de pesquisa do administrador ou Pesquisar no Forms para incluir opções de filtragem adicionais.
* Atualizar e visualizar papel de parede personalizado para sua marca
* Relatório de uso para saber mais sobre o número de usuários, o espaço de armazenamento usado e o total de ativos.

## Recursos adicionais{#additional-resources}

* [Novidades no Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agentes de replicação do autor do AEM](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guia para download acelerado](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Adobe Docs AEM Assets Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentação do Adobe AEM Assets Dynamic Media](https://experienceleague.adobe.com/docs/)
* [Baixar o Assimilar Connect](https://downloads.asperasoft.com/connect2/)
* [Assimilar servidor de teste do Connect](https://test-connect.asperasoft.com/)