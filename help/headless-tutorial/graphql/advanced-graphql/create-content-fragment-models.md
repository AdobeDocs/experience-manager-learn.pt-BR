---
title: Criar modelos de fragmento de conteúdo — Conceitos avançados de AEM headless — GraphQL
description: Neste capítulo dos conceitos avançados do Adobe Experience Manager (AEM) Headless, saiba como editar um modelo de fragmento de conteúdo adicionando espaços reservados para guias, data e hora, objetos JSON, referências de fragmento e referências de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 2122ab13-f9df-4f36-9c7e-8980033c3b10
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 1%

---

# Criar modelos de fragmentos de conteúdo {#create-content-fragment-models}

Este capítulo aborda as etapas para criar cinco modelos de fragmento de conteúdo:

* **Informação de contato**
* **Endereço**
* **Pessoa**
* **Local**
* **Equipe**

Os modelos de fragmento de conteúdo permitem definir relações entre tipos de conteúdo e manter essas relações como esquemas. Use referências de fragmento aninhadas, vários tipos de dados de conteúdo e o tipo de guia para organização visual do conteúdo. Tipos de dados mais avançados, como espaços reservados para guias, referências de fragmentos, objetos JSON e o tipo de dados data e hora.

Este capítulo também aborda como aprimorar as regras de validação para referências de conteúdo, como imagens.

## Pré-requisitos {#prerequisites}

Este é um tutorial avançado. Antes de continuar com este capítulo, verifique se você concluiu o [configuração rápida](../quick-setup/cloud-service.md). Certifique-se de ler também o anterior [visão geral](../overview.md) capítulo para obter mais informações sobre a configuração do tutorial avançado.

## Objetivos {#objectives}

* Criar modelos de fragmentos de conteúdo.
* Adicione espaços reservados em guias, data e hora, objetos JSON, referências de fragmento e referências de conteúdo aos modelos.
* Adicione validação às referências de conteúdo.

## Visão geral do modelo de fragmento de conteúdo {#content-fragment-model-overview}

O vídeo a seguir fornece uma breve introdução aos modelos de fragmento de conteúdo e como eles são usados neste tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/340037?quality=12&learn=on)

## Criar modelos de fragmentos de conteúdo {#create-models}

Vamos criar alguns modelos de fragmento de conteúdo para o aplicativo WKND. Se você precisar de uma introdução básica à criação de modelos de fragmento de conteúdo, consulte o capítulo apropriado no [tutorial básico](../multi-step/content-fragment-models.md).

1. Navegue até **Ferramentas** > **Geral** > **Modelos de fragmentos do conteúdo**.

   ![Caminho dos modelos de fragmento de conteúdo](assets/define-content-fragment-models/content-fragment-models-path.png)

1. Selecionar **WKND compartilhado** para visualizar a lista de modelos de fragmento de conteúdo existentes para o site.

### Modelo de informações de contato {#contact-info-model}

Em seguida, crie um modelo que contenha as informações de contato de uma pessoa ou local.

1. Selecionar **Criar** no canto superior direito.

1. Forneça ao modelo um título de &quot;Informações de contato&quot; e selecione **Criar**. No modal de sucesso exibido, selecione **Abertura** para editar o modelo recém-criado.

1. Comece arrastando um **Texto em linha única** no modelo. Dê a ele um **Rótulo do campo** de &quot;Telefone&quot; na **Propriedades** guia. O nome da propriedade é preenchido automaticamente como `phone`. Marque a caixa de seleção para criar o campo **Obrigatório**.

1. Navegue até a **Tipos de dados** e adicionar outro **Texto em linha única** abaixo do campo &quot;Telefone&quot;. Dê a ele um **Rótulo do campo** de &quot;Email&quot; e também o defina como **Obrigatório**.

O Adobe Experience Manager vem com alguns métodos de validação integrados. Esses métodos de validação permitem adicionar regras de governança a campos específicos nos modelos de fragmento de conteúdo. Nesse caso, vamos adicionar uma regra de validação para garantir que os usuários só possam inserir endereços de email válidos ao preencher esse campo. No **Tipo de validação** selecione **E-mail**.

O modelo completo de fragmento de conteúdo deve ser semelhante a:

![Caminho do modelo de informações de contato](assets/define-content-fragment-models/contact-info-model.png)

Depois de concluído, selecione **Salvar** para confirmar as alterações e fechar o Editor de modelos de fragmentos de conteúdo.

### Modelo de endereço {#address-model}

Em seguida, crie um modelo para um endereço.

1. No **WKND compartilhado**, selecione **Criar** no canto superior direito.

1. Insira um título de &quot;Endereço&quot; e selecione **Criar**. No modal de sucesso exibido, selecione **Abertura** para editar o modelo recém-criado.

1. Arraste e solte uma **Texto em linha única** no modelo e forneça a ele um **Rótulo do campo** de &quot;Endereço&quot;. O nome da propriedade é preenchido como `streetAddress`. Selecione o **Obrigatório** caixa de seleção

1. Repita as etapas acima e adicione mais quatro campos de &quot;Texto de linha única&quot; ao modelo. Use os seguintes rótulos:

   * Cidade
   * Estado
   * Código postal
   * País

1. Selecionar **Salvar** para salvar as alterações no modelo de Endereço.

   O modelo de fragmento &quot;Endereço&quot; concluído deve ter esta aparência:
   ![Modelo de endereço](assets/define-content-fragment-models/address-model.png)

### Modelo de pessoa {#person-model}

Em seguida, crie um modelo que contenha informações sobre uma pessoa.

1. No canto superior direito, selecione **Criar**.

1. Dê ao modelo um título de &quot;Pessoa&quot; e selecione **Criar**. No modal de sucesso exibido, selecione **Abertura** para editar o modelo recém-criado.

1. Comece arrastando um **Texto em linha única** no modelo. Dê a ele um **Rótulo do campo** de &quot;Nome completo&quot;. O nome da propriedade é preenchido automaticamente como `fullName`. Marque a caixa de seleção para criar o campo **Obrigatório**.

   ![Opções de nome completo](assets/define-content-fragment-models/full-name.png)

1. Os modelos de fragmento de conteúdo podem ser consultados em outros modelos. Navegue até a **Tipos de dados** e arraste e solte a guia **Referência do fragmento** e forneça um rótulo de &quot;Contact Info&quot;.

1. No **Propriedades** , no menu **Modelos de fragmentos do conteúdo permitido** , selecione o ícone de pasta e escolha o **Informações de contato** modelo de fragmento criado anteriormente.

1. Adicionar um **Referência de conteúdo** e dê a ele um **Rótulo do campo** de &quot;Foto do perfil&quot;. Selecione o ícone de pasta em **Caminho raiz** para abrir a modal de seleção de caminho. Selecione um caminho raiz selecionando **conteúdo** > **Assets** e marcando a caixa de seleção de **WKND compartilhado**. Use o **Selecionar** botão na parte superior direita para salvar o caminho. O caminho do texto final deve ser lido `/content/dam/wknd-shared`.

   ![Caminho raiz da referência de conteúdo](assets/define-content-fragment-models/content-reference-root-path.png)

1. Em **Aceitar apenas tipos de conteúdo especificados**, selecione &quot;Imagem&quot;.

   ![Opções de imagem do perfil](assets/define-content-fragment-models/profile-picture.png)

1. Para limitar o tamanho e as dimensões do arquivo de imagem, vamos ver algumas opções de validação para o campo de referência de conteúdo.

   Em **Aceitar apenas o tamanho de arquivo especificado**, selecione &quot;Menor que ou igual a&quot; e campos adicionais aparecerão abaixo.
   ![Aceitar apenas o tamanho de arquivo especificado](assets/define-content-fragment-models/accept-specified-file-size.png)

1. Para **Máx**, digite &quot;5&quot; e para **Selecionar unidade**, selecione &quot;Megabytes (MB)&quot;. Essa validação permite que apenas imagens do tamanho especificado sejam escolhidas.

1. Em **Aceitar apenas a largura de imagem especificada**, selecione &quot;Largura máxima&quot;. No **Máximo (pixels)** que for exibido, digite &quot;10000&quot;. Selecione as mesmas opções para **Aceitar apenas uma altura de imagem especificada**.

   Essas validações garantem que as imagens adicionadas não excedam os valores especificados. Agora, as regras de validação se parecem com o seguinte:

   ![Regras de validação de referência de conteúdo](assets/define-content-fragment-models/content-reference-validation.png)

1. Adicionar um **Texto multilinha** e dê a ele um **Rótulo do campo** de &quot;Biografia&quot;. Deixe a **Tipo padrão** como a opção padrão &quot;Rich Text&quot;.

   ![Opções de biografia](assets/define-content-fragment-models/biography.png)

1. Navegue até a **Tipos de dados** e arraste uma **Enumeração** campo abaixo de &quot;Biografia&quot;. Em vez do padrão **Renderizar como** selecione **Lista suspensa** e dê a ele um **Rótulo do campo** do &quot;Nível de experiência do professor&quot;. Informe uma seleção de opções de nível de experiência do professor, como _Especialista, avançado, intermediário_.

1. Em seguida, arraste outro **Enumeração** campo em &quot;Nível de experiência do professor&quot; e escolha &quot;caixas de seleção&quot; no campo **Renderizar como** opção. Dê a ele um **Rótulo do campo** de &quot;Competências&quot;. Entre em diferentes habilidades, como escalada, surf, ciclismo, esqui e mochila. O rótulo da opção e o valor da opção devem corresponder conforme abaixo:

   ![Enumeração de habilidades](assets/define-content-fragment-models/skills-enum.png)

1. Por fim, crie um rótulo de campo &quot;Detalhes do administrador&quot; usando um **Texto multilinha** campo.

Selecionar **Salvar** para confirmar as alterações e fechar o Editor de modelos de fragmentos de conteúdo.

### Modelo de localização {#location-model}

O próximo modelo de fragmento de conteúdo descreve um local físico. Esse modelo usa espaços reservados em guias. Os espaços reservados para guias ajudam a organizar os tipos de dados no editor de modelo e o conteúdo no editor de fragmento, respectivamente, categorizando o conteúdo. Cada espaço reservado cria uma guia, semelhante a uma guia em um navegador da Internet, no editor de Fragmento de conteúdo. O modelo de Localização deve ter duas guias: Detalhes da localização e Endereço da localização.

1. Como anteriormente, selecione **Criar** para criar outro modelo de fragmento de conteúdo. Para o Título do modelo, digite &quot;Local&quot;. Selecionar **Criar** seguido por **Abertura** no modal de sucesso exibido.

1. Adicionar um **Espaço reservado da guia** ao modelo e rotule-o como &quot;Detalhes de localização&quot;.

1. Arraste e solte uma **Texto em linha única** e rotule-o como &quot;Nome&quot;. Abaixo deste rótulo de campo, adicione um **texto multilinha** e rotule-o como &quot;Descrição&quot;.

1. Em seguida, adicione um **Referência do fragmento** e rotule-o como &quot;Informações de contato&quot;. Na guia Propriedades, em **Modelos de fragmentos do conteúdo permitido**, selecione o **Ícone de pasta** e escolha o modelo de fragmento &quot;Informações de contato&quot; criado anteriormente.

1. Adicionar um **Referência de conteúdo** em &quot;Informações de contato&quot;. Rotule-o como &quot;Imagem de localização&quot;. A variável **Caminho raiz** deve ser `/content/dam/wknd-shared.` Em **Aceitar apenas tipos de conteúdo especificados**, selecione &quot;Imagem&quot;.

1. Vamos também adicionar um **Objeto JSON** sob a &quot;Imagem do local&quot;. Como esse tipo de dados é flexível, ele pode ser usado para exibir quaisquer dados que você deseja incluir no conteúdo. Nesse caso, o objeto JSON é usado para exibir informações sobre o tempo. Rotule o objeto JSON &quot;Clima por temporada&quot;. No **Propriedades** , adicionar um **Descrição** portanto, fica claro para o usuário quais dados devem ser inseridos aqui: &quot;Dados JSON sobre o clima no local do evento por estação (primavera, verão, outono, inverno)&quot;.

   ![Opções de objeto JSON](assets/define-content-fragment-models/json-object.png)

1. Para criar a guia Endereço do local, adicione um **Espaço reservado da guia** ao modelo e rotule-o como &quot;Endereço do local&quot;.

1. Arraste e solte uma **Referência do fragmento** e na guia de propriedades, rotule-o como &quot;Endereço&quot; e, em **Modelos de fragmentos do conteúdo permitido**, selecione o **Endereço** modelo.

1. Selecionar **Salvar** para confirmar as alterações e fechar o Editor de modelos de fragmentos de conteúdo. O modelo de Localização concluído deve aparecer conforme abaixo:

   ![Opções de referência de conteúdo](assets/define-content-fragment-models/location-model.png)

### Modelo da equipe {#team-model}

Finalmente, crie um modelo que descreva uma equipe de pessoas.

1. No **WKND compartilhado** selecione **Criar** para criar outro modelo de fragmento de conteúdo. Para o Título do modelo, digite &quot;Equipe&quot;. Como anteriormente, selecione **Criar** seguido por **Abertura** no modal de sucesso exibido.

1. Adicionar um **Texto multilinha** ao formulário. Em **Rótulo do campo**, digite &quot;Descrição&quot;.

1. Adicionar um **Data e hora** ao modelo e rotule-o como &quot;Data de fundação da equipe&quot;. Nesse caso, mantenha o padrão **Tipo** defina como &quot;Date&quot;, mas observe que também é possível usar &quot;Date &amp; Time&quot; ou &quot;Time&quot;.

   ![Opções de data e hora](assets/define-content-fragment-models/date-and-time.png)

1. Navegue até a **Tipos de dados** guia. Abaixo da &quot;Data de fundação da equipe&quot;, adicione uma **Referência do fragmento**. No **Renderizar como** selecione &quot;vários campos&quot;. Para **Rótulo do campo**, digite &quot;Membros da equipe&quot;. Este campo está vinculado à _Person_ modelo criado anteriormente. Como o tipo de dados é um multicampo, vários fragmentos de Pessoa podem ser adicionados, permitindo a criação de uma equipe de pessoas.

   ![Opções de referência do fragmento](assets/define-content-fragment-models/fragment-reference.png)

1. Em **Modelos de fragmentos do conteúdo permitido**, use o ícone de pasta para abrir a modal Selecionar caminho e selecione a **Person** modelo. Use o **Selecionar** botão para salvar o caminho.

   ![Selecionar modelo de pessoa](assets/define-content-fragment-models/select-person-model.png)

1. Selecionar **Salvar** para confirmar as alterações e fechar o Editor de modelos de fragmentos de conteúdo.

## Adicionar referências de fragmento ao modelo de aventura {#fragment-references}

Semelhante à forma como o modelo Equipe tem uma referência de fragmento para o modelo Pessoa, os modelos Equipe e Localização devem ser referenciados a partir do modelo Aventura para exibir esses novos modelos no aplicativo WKND.

1. No **WKND compartilhado** selecione a **Aventura** e selecione **Editar** no início da navegação.

   ![Caminho de edição de aventura](assets/define-content-fragment-models/adventure-edit-path.png)

1. Na parte inferior do formulário, abaixo de &quot;O que trazer&quot;, adicione uma **Referência do fragmento** campo. Insira um **Rótulo do campo** de &quot;Localização&quot;. Em **Modelos de fragmentos do conteúdo permitido**, selecione o **Localização** modelo.

   ![Opções de referência do fragmento de localização](assets/define-content-fragment-models/location-fragment-reference.png)

1. Adicionar mais um **Referência do fragmento** e rotule-o como &quot;Equipe do professor&quot;. Em **Modelos de fragmentos do conteúdo permitido**, selecione o **Equipe** modelo.

   ![Opções de referência do fragmento da equipe](assets/define-content-fragment-models/team-fragment-reference.png)

1. Adicionar outro **Referência do fragmento** e rotule-o como &quot;Administrador&quot;.

   ![Opções de referência de fragmento do administrador](assets/define-content-fragment-models/administrator-fragment-reference.png)

1. Selecionar **Salvar** para confirmar as alterações e fechar o Editor de modelos de fragmentos de conteúdo.

## Práticas recomendadas     {#best-practices}

Existem algumas práticas recomendadas relacionadas à criação de modelos de fragmento de conteúdo:

* Criar modelos que mapeiam para componentes UX. Por exemplo, o aplicativo WKND tem modelos de fragmento de conteúdo para aventuras, artigos e localização. Você também pode adicionar cabeçalhos, promoções ou isenções de responsabilidade. Cada um desses exemplos compõe um componente UX específico.

* Crie o menor número de modelos possível. Limitar o número de modelos permite maximizar a reutilização e simplificar o gerenciamento de conteúdo.

* Aninhe os modelos de fragmento de conteúdo tão profundamente quanto necessário, mas somente conforme necessário. Lembre-se de que o aninhamento é realizado com referências de fragmento ou de conteúdo. Considere um máximo de cinco níveis de aninhamento.

## Parabéns! {#congratulations}

Parabéns! Agora você adicionou guias, usou os tipos de dados de objeto data e hora e JSON e aprendeu mais sobre referências de fragmento e conteúdo. Você também adicionou regras de validação de referência de conteúdo.

## Próximas etapas {#next-steps}

O próximo capítulo desta série [criação de fragmentos de conteúdo](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md) a partir dos modelos criados neste capítulo. Saiba como usar os tipos de dados introduzidos neste capítulo e criar políticas de pasta para limitar quais modelos de fragmento de conteúdo podem ser criados em uma pasta de ativos.

Embora seja opcional para este tutorial, publique todo o conteúdo em situações de produção reais. Para obter uma revisão dos ambientes Autor e Publicação no AEM, consulte o
[Série de vídeos AEM Headless e GraphQL](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md).
