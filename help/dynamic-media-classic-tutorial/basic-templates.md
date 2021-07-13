---
title: Introdução a modelos básicos
description: Saiba mais sobre Modelos básicos no Dynamic Media Classic, modelos baseados em imagem chamados do Servidor de imagem e que consistem em imagens e texto renderizado. Um modelo pode ser alterado dinamicamente por meio do URL após a publicação do modelo. Você aprenderá a fazer upload de um PSD do Photoshop para o Dynamic Media Classic e usá-lo como a base de um modelo. Crie um modelo básico de merchandising simples que consiste em camadas de imagem. Adicione camadas de texto e torne-as variáveis por meio do uso de parâmetros. Construa um URL de modelo e manipule a imagem dinamicamente pelo navegador da Web.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Gerenciamento de conteúdo
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '6306'
ht-degree: 0%

---


# Introdução a modelos básicos {#basic-templates}

Em termos do Dynamic Media Classic, um modelo é um documento que pode ser alterado dinamicamente por meio do URL após a publicação do modelo. O Dynamic Media Classic oferece modelos básicos, modelos baseados em imagem chamados do Servidor de imagem e que consistem em imagens e texto renderizado.

Um dos aspectos mais eficientes dos modelos é que eles têm pontos de integração diretos que permitem vinculá-los ao seu banco de dados. Assim, você não só pode disponibilizar uma imagem e redimensioná-la, como também pode consultar seu banco de dados para localizar itens novos ou de venda e fazer com que apareça como uma sobreposição na imagem. Você pode solicitar uma descrição do item e fazer com que ele apareça como um rótulo em uma fonte escolhida e no layout. As possibilidades são ilimitadas.

Modelos básicos podem ser implementados de várias maneiras diferentes, de simples a complexa. Por exemplo:

- Comercialização básica. Usa etiquetas como &quot;frete grátis&quot; se o produto tiver frete grátis. Esses rótulos são configurados pela equipe de mercadorias no Photoshop e a Web usa a lógica para saber quando aplicá-los à imagem.
- Comercialização avançada. Cada modelo tem várias variáveis e pode mostrar mais de uma opção ao mesmo tempo. Usa um banco de dados, inventário e regras de negócios para determinar quando mostrar um produto como &quot;Apenas na&quot;, em &quot;Limpeza&quot; ou &quot;Desconto&quot;. Também pode usar a transparência por trás do produto para exibi-lo em diferentes planos de fundo, como em diferentes salas. Os mesmos modelos e/ou ativos podem ser redefinidos na página de detalhes do produto para mostrar uma versão maior ou com zoom do mesmo produto em diferentes planos de fundo.

É importante entender que o Dynamic Media Classic fornece apenas a parte visual desses aplicativos baseados em modelos. As empresas do Dynamic Media Classic ou seus parceiros de integração devem fornecer as regras de negócios, o banco de dados e as habilidades de desenvolvimento para criar os aplicativos. Não há um aplicativo de modelo &quot;incorporado&quot;; os designers configuram o modelo no Dynamic Media Classic e os desenvolvedores usam chamadas de URL para alterar as variáveis no modelo.

Ao final desta seção do tutorial, você saberá:

- Faça upload de um PSD do Photoshop para o Dynamic Media Classic para usá-lo como a base de um template.
- Crie um modelo básico de merchandising simples que consiste em camadas de imagem.
- Adicione camadas de texto e torne-as variáveis por meio do uso de parâmetros.
- Construa um URL de modelo e manipule a imagem dinamicamente pelo navegador da Web.

>[!NOTE]
>
>Todos os URLs neste capítulo são apenas para fins ilustrativos; eles não são links ao vivo.

## Visão geral de modelos básicos

A definição de um modelo básico (ou apenas &quot;modelo&quot;, para encurtar) é uma imagem em camadas endereçável para URL. O resultado final é uma imagem, mas que pode ser alterada pelo URL. Pode consistir em fotos, texto ou gráficos — qualquer combinação de ativos P-TIFF no Dynamic Media Classic.

Os modelos são mais semelhantes aos arquivos PSD do Photoshop, pois têm um fluxo de trabalho semelhante e recursos semelhantes.

- Ambos consistem em camadas que são como folhas de acetato empilhado. Você pode compor imagens parcialmente transparentes e ver através das áreas transparentes de uma camada as camadas abaixo.
- As camadas podem ser movidas e giradas para reposicionar o conteúdo, e a opacidade e os modos de mesclagem podem ser alterados para tornar o conteúdo parcialmente transparente.
- É possível criar camadas baseadas em texto. A qualidade pode ser muito alta porque o Servidor de imagem usa o mesmo mecanismo de texto que o Photoshop e o Illustrator.
- Estilos de camada simples podem ser aplicados a cada camada para criar efeitos especiais, como sombras ou brilhos.

No entanto, ao contrário dos PSDs do Photoshop, as camadas podem ser totalmente dinâmicas e controladas por meio de um URL no Servidor de imagem.

- É possível adicionar variáveis a todas as propriedades do modelo, facilitando a alteração da composição em tempo real.
- As variáveis chamadas de parâmetros permitem expor apenas a parte do modelo que você deseja alterar.

Você só precisa adicionar um espaço reservado para cada camada que variará em vez de colocar todas as camadas em um único arquivo, como faz no Photoshop, mostrar e ocultá-las (embora também possa fazer isso, se preferir).

Usando um espaço reservado, é possível trocar dinamicamente o conteúdo de uma camada por outro ativo publicado e ele usará automaticamente as mesmas propriedades (como tamanho e rotação) da camada que substituiu.

Como os Modelos básicos geralmente são projetados no Photoshop, mas implantados por meio de um URL, um projeto de modelo requer uma mistura de habilidades técnicas e de design. Normalmente, supomos que a pessoa que faz o trabalho de criação de modelo é um designer da Photoshop e que a pessoa que implementa o modelo é um desenvolvedor da Web. As equipes de criação e desenvolvimento devem trabalhar em conjunto para que o modelo seja bem-sucedido.

Os projetos de modelo podem ser relativamente simples ou extremamente complexos, dependendo das regras de negócios e das necessidades do aplicativo. Os Modelos básicos são chamados do Servidor de imagem, no entanto, devido à flexibilidade do ambiente Dynamic Media Classic, você pode até aninhar modelos dentro de outros modelos, permitindo que você crie imagens bastante complexas que podem ser vinculadas por variáveis comumente nomeadas.

- Saiba mais sobre [Noções básicas sobre o modelo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/quick-start-template-basics.html).
- Saiba como criar um [Modelo básico](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#creating_a_template).

## Criando um modelo básico

Ao trabalhar com um modelo básico, você geralmente segue as etapas do fluxo de trabalho no diagrama abaixo. As etapas marcadas com linhas pontilhadas são opcionais se você estiver usando camadas de texto dinâmicas e são indicadas nas instruções abaixo como &quot;Fluxo de trabalho de texto&quot;. Se não estiver usando texto, siga somente o caminho principal.

![imagem](assets/basic-templates/basic-templates-1.png)

_O fluxo de trabalho Basic Template ._

1. Crie e crie ativos. A maioria dos usuários faz isso no Adobe Photoshop. Projete ativos com o tamanho exato necessário — se for uma imagem de 200 pixels para uma página de miniatura, crie-a a 200 pixels. Se precisar aplicar zoom, crie-o com um tamanho de cerca de 2000 pixels. Use o Photoshop (e/ou Illustrator salvo como bitmap) para criar os ativos e use o Dynamic Media Classic para reunir as partes, gerenciar as camadas e adicionar variáveis.
2. Após projetar ativos gráficos, faça o upload deles para o Dynamic Media Classic. Em vez de fazer upload de ativos individuais do PSD, recomendamos que você faça o upload de todo o arquivo PSD em camadas e que o Dynamic Media Classic crie um arquivo por camada, usando a opção **Manter camadas** no upload (consulte abaixo para obter mais detalhes). _Fluxo de trabalho do texto: Se estiver criando um texto dinâmico, faça upload das fontes também. O texto dinâmico é variável e controlado pelo URL. Se o texto for estático ou tiver apenas algumas frases curtas que não mudam — por exemplo, tags que dizem &quot;Novo&quot; ou &quot;Venda&quot;, em vez de &quot;X% Desativado&quot;, com o X sendo um número variável — recomendamos pré-renderizar o texto no Photoshop e fazer upload como camadas rasterizadas como imagens. Será mais fácil e você pode criar o estilo do texto exatamente como quiser._
3. Crie o modelo no Dynamic Media Classic usando o editor de Noções básicas do modelo do menu Criar e adicione camadas de imagem. Fluxo de trabalho do texto: Crie camadas de texto no mesmo editor. Essa etapa é necessária ao criar um modelo manualmente no Dynamic Media Classic. Escolha um tamanho de tela que corresponda ao design, arraste e solte imagens na tela e defina as propriedades da camada (tamanho, rotação, opacidade, etc.). Você não está colocando cada camada possível no modelo, apenas um espaço reservado por camada de imagem. _Fluxo de trabalho do texto: É possível criar camadas de texto com a ferramenta Texto, de modo semelhante à criação de camadas de texto no Photoshop. É possível escolher uma fonte e um estilo usando as mesmas opções disponíveis com a ferramenta Photoshop Type._ Outro fluxo de trabalho é fazer o upload de um PSD e o Dynamic Media Classic gerar um modelo &quot;gratuito&quot; e até mesmo recriar camadas de texto. Isso será discutido mais detalhadamente posteriormente.
4. Depois que as camadas forem criadas, adicione parâmetros (variáveis) a qualquer propriedade de qualquer camada que você gostaria de controlar por meio do URL, incluindo a origem da camada (a própria imagem ). _Fluxo de trabalho do texto: Também é possível adicionar parâmetros às camadas de texto, tanto para controlar o conteúdo do texto, como o tamanho e a posição da própria camada, além de todas as opções de formatação, como cor da fonte, tamanho da fonte, rastreamento horizontal, etc._
5. Crie uma predefinição de imagem que corresponda ao tamanho do modelo. Recomendamos fazer isso para que o modelo seja sempre chamado em um tamanho 1:1 e também para adicionar nitidez a qualquer camada de imagem grande que seja redimensionada para se ajustar ao modelo. Se você estiver criando um modelo para ampliar, essa etapa é desnecessária.
6. Publique, copie o URL da visualização do Dynamic Media Classic e teste-o em um navegador.

## Preparação e upload dos ativos de modelo para o Dynamic Media Classic

Antes de fazer upload dos ativos de modelo no Dynamic Media Classic, você precisará concluir algumas etapas preparatórias.

### Preparação do PSD para upload

Antes de fazer upload do arquivo Photoshop para o Dynamic Media Classic, simplifique as camadas no Photoshop para facilitar o trabalho e ter maior compatibilidade com o Servidor de imagem. O arquivo PSD geralmente consiste em muitos elementos que o Dynamic Media Classic não reconhece e você também pode acabar com muitas peças difíceis de gerenciar. Certifique-se de salvar um backup do seu PSD principal caso precise editar o original posteriormente. Você fará upload da cópia simplificada, e não da principal.

![imagem](assets/basic-templates/basic-templates-2.jpg)

1. Simplifique a estrutura da camada unindo/achatando camadas relacionadas que precisam ser ativadas/desativadas em uma única camada. Por exemplo, o rótulo &quot;NOVO&quot; e o banner azul são unidos em uma única camada, de modo que você possa mostrá-los ou ocultá-los com um único clique.
   ![imagem](assets/basic-templates/basic-templates-3.jpg)
2. Alguns tipos de camadas e efeitos de camada não são compatíveis com o Dynamic Media Classic ou o Servidor de imagem e precisam ser rasterizados antes do upload. Caso contrário, os efeitos podem ser ignorados ou as camadas descartadas. Rasterizar uma camada significa converter se de editável em não editável. Para rasterizar efeitos de camada ou camadas de texto, crie uma camada vazia, selecione e mescle usando **Camadas > Mesclar camadas** ou CTRL + E/CMD + E.

   - O Dynamic Media Classic não pode agrupar ou vincular camadas. Todas as camadas em um grupo ou conjunto vinculado serão convertidas em camadas separadas que não são mais agrupadas/vinculadas.
   - Máscaras de camada serão convertidas em transparência ao fazer upload.
   - As camadas de ajuste não são compatíveis e serão descartadas.
   - As camadas de preenchimento, como as camadas de cor sólida, serão rasterizadas.
   - As camadas de objeto inteligente e as camadas de vetor serão rasterizadas em imagens normais no upload e os Filtros inteligentes serão aplicados e rasterizados.
   - As camadas de texto também serão rasterizadas a menos que você use a opção Extrair texto — consulte abaixo para obter mais informações.
   - A maioria dos efeitos de camada será ignorada e somente alguns modos de mesclagem são compatíveis. Em caso de dúvida, adicione efeitos simples no Dynamic Media Classic (como sombras internas ou de soltar, brilhos internos ou externos) ou use uma camada em branco para mesclar e rasterizar o efeito no Photoshop.

### Trabalhar com fontes

Você também fará upload e publicará suas fontes se precisar gerar texto dinâmico. A única fonte incluída no Dynamic Media Classic é Arial.

Cada empresa é responsável por obter uma licença para usar uma fonte na Web — simplesmente ter uma fonte instalada em seu computador não lhe dá o direito de usá-la comercialmente na Web e sua empresa pode enfrentar uma ação legal do editor de fontes, se usada sem permissão. Além disso, os termos de licença variam. Você pode precisar de licenças separadas para impressão versus exibição de tela, por exemplo.

O Dynamic Media Classic é compatível com as fontes padrão OpenType (OTF), TrueType (TTF) e Tipo 1 Postscript. Fontes de mala somente Mac, arquivos de coleção de tipos, fontes do sistema Windows e fontes de máquina proprietárias (como fontes usadas por gravadores ou máquinas de bordar) não são compatíveis — será necessário convertê-las em um dos formatos de fonte padrão ou substituir uma fonte semelhante a ser usada no Dynamic Media Classic e no Servidor de imagem.

Depois que as fontes forem carregadas no Dynamic Media Classic, como qualquer outro ativo, elas também deverão ser publicadas no Servidor de imagens. Um erro de modelo muito comum é esquecer de publicar suas fontes, o que resultará em um erro de imagem. O Servidor de imagem não substituirá outra fonte em seu lugar. Além disso, se você quiser usar a opção **Extrair texto** ao fazer upload, será necessário fazer upload dos arquivos de fonte antes de fazer upload do PSD que usa essas fontes. O recurso **Extrair texto** tentará recriar o texto como uma camada de texto editável e colocá-lo dentro de um modelo do Dynamic Media Classic. Isso é discutido no próximo tópico, Opções de PSD.

Esteja ciente de que as fontes têm vários nomes internos que geralmente são diferentes de seu nome de arquivo externo. Você pode ver todos os nomes diferentes na página Detalhes do ativo no Dynamic Media Classic. Estes são os nomes da fonte Adobe Caslon Pro Semibold, listada na guia Metadados no Dynamic Media Classic:

![imagem](assets/basic-templates/basic-templates-4.jpg)

_Guia Metadados na página Detalhes de uma fonte no Dynamic Media Classic._

O Dynamic Media Classic usa o nome de arquivo dessa fonte (ACaslonPro-Semibold) como a ID do ativo, no entanto, esse não é o nome usado pelo modelo. O modelo usa o Nome Rich Text Format (RTF), listado na parte inferior. RTF é o &quot;idioma&quot; nativo do mecanismo de texto do Servidor de imagem.

Se você precisar alterar fontes por meio do URL, é necessário chamar o nome RTF da fonte (não a ID do ativo), ou ocorrerá um erro. Nesse caso, o nome correto dessa fonte seria &quot;Adobe Caslon Pro&quot;. Discutiremos mais sobre fontes e RTF no tópico RTF e Parâmetros de texto, abaixo.

Os formatos de arquivo de fonte mais comuns encontrados em sistemas Windows e Mac são OpenType e TrueType. O OpenType tem uma extensão .OTF, enquanto o TrueType é .TTF. Ambos os formatos funcionam igualmente bem no Dynamic Media Classic.

### Selecionar opções ao carregar seu PSD

Não é necessário fazer upload de um arquivo Photoshop (PSD) para criar um modelo; um modelo pode ser construído a partir de qualquer ativo de imagem no Dynamic Media Classic. No entanto, fazer upload de um PSD pode facilitar a criação, pois você normalmente já terá esses ativos em um PSD em camadas. Além disso, o Dynamic Media Classic gera automaticamente um modelo quando você carrega um PSD em camadas.

- **Manter camadas.** Esta é a opção mais importante. Isso instrui o Dynamic Media Classic a criar um ativo de imagem por camada do Photoshop. Se estiver desmarcado, todas as outras opções serão desativadas e o PSD será nivelado em uma única imagem.
- **** **CreateTemplate.** Essa opção pega as várias camadas geradas e cria automaticamente um modelo ao combiná-las novamente. Uma desvantagem do uso do modelo gerado automaticamente é que o Dynamic Media Classic coloca todas as camadas em um arquivo, enquanto precisamos apenas de um único espaço reservado por camada. É fácil o suficiente excluir as camadas extras, mas se você tiver muitas camadas, é mais rápido recriá-las. Certifique-se de renomear o novo modelo; caso contrário, ele será substituído na próxima vez que você carregar novamente o mesmo PSD.
- **Extrair texto.** Isso recria camadas de texto no PSD como camadas de texto no modelo usando a fonte carregada. Essa etapa é necessária se o texto estiver em um caminho no Photoshop e você quiser manter esse caminho no modelo. Este recurso exige que você use a opção **Criar modelo**, pois o texto extraído só pode ser criado por um modelo gerado no upload.
- **Estender camadas ao tamanho do plano de fundo.** Essa configuração torna cada camada do mesmo tamanho da tela geral do PSD. Isso é muito útil para camadas que sempre permanecerão fixas na posição: caso contrário, ao trocar imagens na mesma camada, talvez seja necessário reposicioná-las.
- **Nomenclatura de camada** Isso informa ao Dynamic Media Classic como nomear cada ativo gerado por camada. Recomendamos **Photoshop** **e Layer** **Name** ou Photoshop e **Layer** **Number**. Ambas as opções usam o nome PSD como a primeira parte do nome e adicionam o nome ou o número da camada no final. Por exemplo, se você tiver um PSD chamado &quot;shirt.psd&quot; e ele tiver camadas chamadas &quot;front&quot;, &quot;sleeves&quot; e &quot;collar&quot;, se fizer upload usando a opção **Photoshop e** Layer **Name**, o Dynamic Media Classic geraria as IDs de ativos &quot;shirt_front&quot;, &quot;shirt_sleeves&quot; e &quot;shirt_collar&quot;. Usar uma dessas opções ajuda a garantir que o nome seja exclusivo no Dynamic Media Classic.

## Criar um modelo com camadas de imagem

Embora o Dynamic Media Classic possa criar automaticamente um modelo de um PSD em camadas, você deve saber como criar o modelo manualmente. Como explicado acima, há certas ocasiões em que você não deseja usar o modelo criado pelo Dynamic Media Classic.

### A interface do usuário de Noções básicas do modelo

Primeiramente, vamos nos familiarizar com a interface de edição.

No centro esquerdo, é sua área de trabalho que mostra uma pré-visualização do modelo final. No lado direito estão os painéis Camadas e Propriedades da camada . Estas são as áreas onde você vai fazer o maior trabalho.

![imagem](assets/basic-templates/basic-templates-5.jpg)

_Página Noções básicas sobre o modelo de compilação ._

- **Visualizar/Área de Trabalho.** Esta é a janela principal. Aqui você pode mover, redimensionar e girar camadas com o mouse. Contornos de camada são mostrados como linhas tracejadas.
- **Camadas.** Isso é semelhante ao painel Camadas do Photoshop. À medida que você adiciona camadas ao modelo, elas serão exibidas aqui. As camadas são empilhadas de cima para baixo — a camada superior no painel Camadas será vista acima das outras abaixo na lista.
- **Propriedades da camada.** Aqui, é possível ajustar todas as propriedades de uma camada usando controles numéricos. Primeiro, selecione uma camada e ajuste suas propriedades.
- **** **CompositeURL.** Na parte inferior da interface do usuário está a área de URL composto. Isso não será discutido nesta seção do tutorial, no entanto, aqui você verá seu modelo desconstruído como uma série de modificadores de URL de disponibilização de imagens. Essa área é editável — se você estiver familiarizado com comandos do Servidor de imagem, poderá editar manualmente o modelo aqui. No entanto, você também pode quebrá-lo. Como o Photoshop, a numeração da camada começa em 0. A Tela é a camada 0, e a primeira camada que você mesmo adiciona é a camada 1. Os modos de mesclagem determinam como os pixels de uma camada são mesclados com pixels abaixo. Você pode criar uma variedade de efeitos especiais usando modos de mesclagem.

#### Uso do Editor de noções básicas de modelo

Estas são as etapas do fluxo de trabalho para iniciar seu Modelo básico:

1. No Dynamic Media Classic, vá para **Criar > Noções básicas sobre o modelo**. Você não pode ter nada selecionado ou começar selecionando uma imagem, que se tornará a primeira camada do modelo.
2. Escolha um Tamanho e pressione **OK**. Esse tamanho deve corresponder ao tamanho projetado no Photoshop. O editor de modelos será carregado.
3. Se você não tiver uma imagem selecionada na etapa 1, procure ou navegue até uma imagem no painel de ativos à esquerda e arraste-a para a área de trabalho.

   - A imagem será redimensionada automaticamente para se ajustar ao tamanho da tela. Se você planeja trocar suas imagens de alta resolução, então você normalmente traria uma de suas grandes imagens P-TIFF (2000 px) e as usaria como espaço reservado.
   - Essa deve ser a camada mais inferior do modelo, no entanto, é possível reordenar as camadas posteriormente.

4. Redimensione ou reposicione a camada diretamente na área de trabalho ou ajustando as configurações no painel Propriedades da camada.
5. Arraste camadas de imagem adicionais conforme necessário. Adicione efeitos de camadas, se desejar. Consulte o tópico _Adicionar efeitos de camada_, abaixo.
6. Clique em **Save**, escolha um local e dê um nome ao modelo. Você pode visualizar, no entanto, nesse momento, seu modelo será exatamente como uma imagem achatada do Photoshop — ainda não pode ser alterada.

### Adicionar efeitos de camada

O Servidor de imagem suporta alguns efeitos de camada programática — efeitos especiais que alteram a aparência do conteúdo de uma camada. Elas funcionam de forma semelhante aos efeitos de camada no Photoshop. Elas são anexadas a uma camada, mas controladas independentemente da camada. É possível ajustá-los ou removê-los sem fazer uma alteração permanente na própria camada.

- **Sombra**. Aplica uma sombra fora dos limites da camada, posicionada por um deslocamento de x e y pixel.
- **Sombra interna**. Aplica uma sombra dentro dos limites da camada, posicionada por um deslocamento de x e y pixel.
- **Brilho externo**. Aplica um efeito de brilho uniformemente em todas as bordas da camada.
- **Brilho interno**. Aplica um efeito de brilho uniformemente dentro de todas as bordas da camada.

![imagem](assets/basic-templates/basic-templates-6.jpg)

_Uma camada com e sem sombra de soltar_

Para adicionar um efeito, clique em **Adicionar Efeito** e escolha um efeito no menu. Como as camadas normais, você pode selecionar um efeito no painel Camadas e usar o painel Propriedades da camada para ajustar as configurações.

Os efeitos de sombra são deslocados horizontal ou verticalmente para longe da camada, enquanto os efeitos de brilho são aplicados uniformemente em todas as direções. Os efeitos internos atuam sobre as partes opacas da camada, enquanto os efeitos externos afetam apenas as áreas transparentes.

Saiba mais sobre como[Adicionar efeitos de camada](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#using-shadow-and-glow-effects-on-layers).

### Adicionar parâmetros

Se tudo o que você faz é combinar camadas e salvá-las, o resultado líquido não será diferente de uma imagem nivelada do Photoshop. O que torna os modelos especiais é a capacidade de adicionar parâmetros às propriedades de cada camada, para que possam ser alterados dinamicamente pelo URL.

Em termos do Dynamic Media Classic, um parâmetro é uma variável que pode ser vinculada a uma propriedade de template para que possa ser manipulada por meio de um URL. Ao adicionar um parâmetro a uma camada, o Dynamic Media Classic expõe essa propriedade no URL prefixando o nome do seu parâmetro com um cifrão ($) — por exemplo, se você criar um parâmetro chamado &quot;tamanho&quot; para alterar o tamanho de uma camada, o Dynamic Media Classic renomeará o parâmetro $size.

Se você não adicionar um parâmetro para uma propriedade, essa propriedade permanecerá oculta no banco de dados do Dynamic Media Classic e não será exibida no URL.

![imagem](assets/basic-templates/parameters.png)

Sem parâmetros, normalmente, seus URLs seriam muito mais longos, especialmente se você também estivesse usando texto dinâmico. O texto adiciona muitas dezenas de caracteres extras em cada URL.

Finalmente, seu conjunto inicial de parâmetros se tornará os valores padrão das propriedades no template. Se você criar seu modelo, adicionar parâmetros e, em seguida, chamar o URL sem seus parâmetros, o Servidor de imagem criará a imagem com todos os padrões salvos no modelo. Os parâmetros são necessários somente se você deseja alterar uma propriedade. Se uma propriedade não precisar ser alterada, não será necessário definir um parâmetro.

#### Criação de parâmetros

Este é o workflow para criar parâmetros:

1. Clique no botão **Parameters** ao lado do nome da camada para a qual você deseja criar parâmetros. A tela Parâmetros é aberta. Ela lista cada propriedade na camada e seu valor.
1. Selecione a opção **On** ao lado do nome de cada propriedade que deseja transformar em um parâmetro. Um nome de parâmetro padrão será exibido. Você só pode adicionar parâmetros às propriedades que foram alteradas a partir do estado padrão.

   - Por exemplo, se você adicionar uma camada e mantê-la em sua posição xy padrão de 0,0, o Dynamic Media Classic não exporá uma propriedade **Position**. Para corrigir, mova a camada pelo menos um pixel. Agora o Dynamic Media Classic exporá **Position** como uma propriedade que você pode parametrizar.
   - Para adicionar um parâmetro à propriedade show/hide (que ativa e desativa a camada ), clique no ícone **Show** ou **Hide Layer** para desligar a camada (você pode ativá-la novamente mais tarde, se desejar). O Dynamic Media Classic agora exporá uma propriedade **Hide** que pode ser parametrizada.

1. Renomeie os nomes de parâmetro padrão para algo que será mais fácil de identificar no URL. Por exemplo, se você quiser adicionar um parâmetro para alterar a camada do banner sobre uma imagem, altere o nome padrão de &quot;layer_2_src&quot; para &quot;banner&quot;.
1. Pressione **Close** para sair da tela Parameters (Parâmetros).
1. Repita esse processo para outras camadas clicando no botão **Parameters** e adicionando e renomeando parâmetros.
1. Salve as alterações ao concluir.

>[!TIP]
>
>Renomeie seus parâmetros com algo significativo e desenvolva uma convenção de nomenclatura para padronizar esses nomes. Certifique-se de que a convenção de nomenclatura seja acordada previamente pelas equipes de design e desenvolvimento.
>
>Não é possível adicionar um parâmetro porque você não vê a propriedade? Basta alterar a propriedade da camada a partir de seu padrão (movendo, redimensionando, ocultando, etc.). Agora você deve ver essa propriedade exposta.

Saiba mais sobre [Parâmetros do modelo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html).

## Criação de um modelo com camadas de texto

Agora você aprenderá a criar um modelo básico que inclua camadas de texto.

### Como entender o texto dinâmico

Agora você sabe como criar um modelo básico usando camadas de imagem. Para muitos aplicativos, isso é tudo o que você precisa. Como você viu no exercício anterior, as camadas com texto simples (como &quot;Venda&quot; e &quot;Novo&quot;) podem ser rasterizadas e tratadas como imagens, pois o texto não precisa ser alterado.

No entanto, e se for necessário:

- Adicione um rótulo para dizer &quot;25% de desativação&quot;, com o valor de 25% sendo variável
- Adicionar um rótulo de texto com o nome do produto na parte superior da imagem
- Localize suas camadas em idiomas diferentes, dependendo do país em que o modelo é visto

Nesse caso, é necessário adicionar algumas camadas de texto dinâmicas com parâmetros para controlar o texto e/ou a formatação.

Para criar texto, é necessário fazer upload de algumas fontes. Caso contrário, o Dynamic Media Classic assumirá Arial como padrão. As fontes também devem ser publicadas no Servidor de imagens ou gerarão um erro no momento em que tentarem renderizar qualquer texto que use essa fonte.

### Parâmetros de RTF e Texto

Para adicionar variáveis ao texto usando a ferramenta Noções básicas sobre modelo, você deve entender como o texto é renderizado. O Servidor de Imagens gera texto usando o Mecanismo de Texto do Adobe, o mesmo mecanismo usado pelo Photoshop e Illustrator, e o compõe como uma camada na imagem final. Para se comunicar com o mecanismo, o Servidor de imagem usa o Rich Text Format ou RTF.

RTF é uma especificação de formato de arquivo desenvolvida pela Microsoft para especificar a formatação de documentos. É uma linguagem de marcação padrão usada pela maioria dos softwares de processamento de texto e email. Se você escrevesse em um URL &amp;text=\b1 Hello, o Servidor de Imagens geraria uma imagem com a palavra &quot;Hello&quot; em negrito, pois \b1 é o comando RTF para tornar o texto em negrito.

A boa notícia é que o Dynamic Media Classic gera o RTF para você. Sempre que você digita um texto em um modelo e adiciona formatação, o Dynamic Media Classic grava o código RTF silenciosamente no modelo automaticamente. A razão pela qual a mencionamos é porque você vai adicionar parâmetros diretamente ao próprio RTF, por isso é importante que você esteja um pouco familiarizado com isso.

#### Criar camadas de texto

Você pode criar camadas de texto em um modelo no Dynamic Media Classic das duas maneiras a seguir:

1. Ferramenta de texto no Dynamic Media Classic. Discutiremos este método abaixo. O editor de Noções básicas sobre modelo tem uma ferramenta que permite criar uma caixa de texto, inserir texto e formatar o texto. O Dynamic Media Classic gera o RTF, conforme necessário, e o coloca em uma camada separada.
2. Extrair texto (ao carregar). O outro método é criar a camada de texto no Photoshop e salvá-la no PSD como uma camada de texto normal (em vez de rasterizá-la como uma camada de imagem). Em seguida, carregue o arquivo no Dynamic Media Classic e use a opção **Extrair texto**. O Dynamic Media Classic converterá cada camada de texto do Photoshop em uma camada de texto de Exibição de imagem usando comandos RTF. Se você usar esse método, primeiro carregue as fontes no Dynamic Media Classic, caso contrário, o Dynamic Media Classic substituirá a fonte padrão no upload e não será fácil substituir a fonte correta.

### O Editor de Texto

O texto é inserido por meio do Editor de texto. O Editor de texto é uma interface WYSIWYG que permite inserir e formatar o texto usando controles de formatação semelhantes aos do Photoshop ou Illustrator.

![imagem](assets/basic-templates/basic-templates-9.jpg)

_Editor de texto de noções básicas do modelo._

Você fará a maior parte do seu trabalho na guia **Preview**, que permite que você insira o texto e o veja como ele aparecerá no modelo. Também há uma guia **Source**, que é usada para editar manualmente o RTF, se necessário.

O fluxo de trabalho geral é usar a guia **Preview** para digitar algum texto.

Em seguida, selecione o texto e escolha alguma formatação, como cor da fonte, tamanho da fonte ou justificação, usando os controles na parte superior. Depois que o texto tiver o estilo desejado, clique em **Aplicar** para vê-lo atualizado na visualização da área de trabalho. Em seguida, feche o Editor de texto para retornar à janela principal de Noções básicas sobre modelo .

#### Uso do Editor de texto

Estas são as etapas do fluxo de trabalho para adicionar texto dentro da página de criação Noções básicas sobre modelo :

1. Clique no botão de ferramenta **Texto** na parte superior da página de criação.
2. Arraste uma caixa de texto onde deseja que o texto apareça. A janela Editor de texto será aberta em uma janela modal. Em segundo plano, você verá seu modelo, no entanto, ele não poderá ser editado até concluir a edição do texto.
3. Digite o texto de amostra que deseja exibir quando o modelo for carregado pela primeira vez. Por exemplo, se você estiver criando uma caixa de texto para uma imagem de email personalizada, seu texto poderá dizer &quot;Olá nome. Agora é o momento de salvar!&quot; Posteriormente, você adicionaria um parâmetro de texto para substituir Nome por um valor enviado no URL. O texto não aparecerá no modelo abaixo da janela até você clicar em **Aplicar**.
4. Para formatar o texto, selecione-o arrastando com o mouse e escolha um controle de formatação na interface do usuário.

   - Há muitas opções de formatação. Alguns dos mais comuns são fonte (face), tamanho da fonte e cor da fonte, bem como justificação esquerda/centro/direita.
   - Não esqueça de selecionar o texto primeiro. Caso contrário, você não poderá aplicar nenhuma formatação.
   - Para escolher uma fonte diferente, selecione o texto e abra o menu Fonte . O editor mostrará uma lista de todas as fontes carregadas no Dynamic Media Classic. Se uma fonte também estiver instalada no computador, ela será exibida em preto. Se não estiver instalado no computador, será exibido em vermelho. No entanto, ele ainda será renderizado na janela de pré-visualização quando você clicar em **Aplicar**. Você só precisa fazer upload de fontes no Dynamic Media Classic para disponibilizá-las para qualquer pessoa que use o Dynamic Media Classic. Depois de publicar, o Servidor de imagem usará essas fontes para gerar o texto — seus usuários não precisam instalar nenhuma fonte para ver o texto criado porque faz parte de uma imagem.
   - Diferente do Photoshop e do Illustrator, o Servidor de Imagens pode alinhar o texto verticalmente na caixa de texto. O padrão é alinhamento superior. Para alterar isso, selecione o texto e escolha **Meio** ou **Parte inferior** no menu **Alinhamento vertical**.
   - Se o texto for muito grande para a caixa (ou se a caixa de texto for muito pequena), todo ou parte dele será cortado e desaparecerá. Reduza o tamanho da fonte ou aumente a caixa.

5. Clique em **Aplicar** para ver suas alterações entrarem em vigor na janela da área de trabalho. Você deve clicar em **Aplicar**, caso contrário, suas edições serão perdidas.
6. Quando terminar, clique em **Fechar**. Para voltar ao modo de edição, clique duas vezes na camada de texto para reabrir o Editor de texto.

O editor de texto visualizará exatamente o tamanho da fonte se você tiver a fonte instalada localmente em seu sistema.

### Sobre como adicionar parâmetros às camadas de texto

Agora seguimos um processo semelhante para adicionar parâmetros de texto, como fizemos para parâmetros de camada. As camadas de texto também podem usar parâmetros de camada para tamanho, posição e assim por diante; no entanto, eles podem usar parâmetros adicionais que permitem controlar qualquer aspecto do RTF.

Ao contrário dos parâmetros de camada, você só seleciona o valor que deseja alterar e adiciona um parâmetro a ele, em vez de adicionar um parâmetro a toda a propriedade.

![imagem](assets/basic-templates/basic-templates-10.jpg)

Exemplo de RTF:

![imagem](assets/basic-templates/sample-rtf.png)

Ao examinar o RTF, você precisa descobrir onde cada configuração deve ser alterada. No RTF acima, alguns deles podem fazer algum sentido e você pode ver de onde a formatação veio.

Vocês podem ver a frase Chocolate Mint Sandal — esse é o próprio texto.

- Há uma referência à fonte Poor Richard — é aqui que as fontes são selecionadas.
- Você pode ver um valor RGB: \red56\green53\blue4 — esta é a cor do texto.
- Embora o tamanho da fonte seja 20, você não vê o número 20. No entanto, você vê um comando \fs40 — por algum motivo estranho, o RTF mede as fontes como ponto-e-vírgula. Assim, \fs40 é o tamanho da fonte!

Você tem informações suficientes para criar seus parâmetros, no entanto, há uma referência completa de todos os comandos RTF na documentação do Image Serving. Visite a [Documentação de disponibilização de imagens](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html#concept-0d3136db7f6f49668274541cd4b6364c).

#### Adicionar parâmetros às camadas de texto

Estas são as etapas para adicionar parâmetros às camadas de texto.

1. Clique no botão **Parameters** (um &quot;P&quot;) ao lado do nome da camada de texto para a qual você deseja criar parâmetros. A tela Parâmetros é aberta. A guia **Common** lista cada propriedade na camada e seu valor. Aqui você pode adicionar parâmetros de camada regulares.
1. Clique na guia **Text**. Aqui você pode ver o RTF na parte superior; os parâmetros adicionados estarão abaixo disso.
1. Para adicionar um parâmetro, primeiro destaque o valor que deseja alterar e clique no botão **Adicionar Parâmetro**. Certifique-se de selecionar apenas os valores para comandos e não o comando inteiro em si. Por exemplo, para definir um parâmetro para o nome da fonte na amostra RTF acima, eu destacaria apenas &quot;Poor Richard&quot; e adicionaria um parâmetro a isso, mas não também o &quot;\f0&quot;. Ao clicar em **Adicionar Parâmetro** , ele aparecerá na lista abaixo e o valor do parâmetro aparecerá em vermelho no RTF enquanto ele ainda estiver selecionado. Se você precisar remover um parâmetro, clique na caixa de seleção ao lado de **On** para desativar esse parâmetro e ele desaparecerá.
1. Clique em para renomear seu parâmetro para um nome mais significativo.
1. Quando terminar, seu RTF será destacado em verde, onde os parâmetros existem, e seus nomes e valores de parâmetros serão listados abaixo.
1. Clique em **Fechar** para sair da tela Parâmetros. Em seguida, pressione **Save** para salvar o modelo. Se tiver terminado de editar, pressione **Close** para sair da página Noções básicas sobre o modelo.
1. Clique em **Visualizar** para testar seu modelo no Dynamic Media Classic. Para testar os parâmetros de texto, digite o novo texto ou os novos valores na janela de visualização. Para alterar a fonte, você deve digitar o nome RTF exato da fonte.

>[!TIP]
>
>Para adicionar parâmetros à cor do texto, adicione separadamente parâmetros para vermelho, verde e azul. Por exemplo, se o RTF for `\red56\green53\blue46`, você adicionaria parâmetros separados de vermelho, verde e azul para os valores 56, 53 e 46. No URL, você poderia alterar a cor chamando os três: `&$red=56&$green=53&$blue=46`.

Saiba como [Criar parâmetros de texto dinâmicos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html#creating-dynamic-text-parameters).

## Publicar e criar URLs de modelo

### Criar uma predefinição de imagem

Criar uma predefinição para o modelo não é uma etapa necessária. Recomendamos isso como uma prática recomendada para que o modelo seja sempre chamado em um tamanho 1:1 e também para adicionar nitidez a qualquer camada de imagem grande que seja redimensionada para se ajustar ao modelo. Se você chamar uma imagem sem uma predefinição, o Servidor de imagem poderá redimensionar arbitrariamente sua imagem para o tamanho padrão (cerca de 400 pixels) e não aplicará a nitidez padrão.

Não há nada de especial em uma Predefinição de imagem para um modelo. Se você já tiver uma predefinição para uma imagem estática no mesmo tamanho, poderá usá-la.

### Publicação

Será necessário executar uma publicação para ver as alterações enviadas ao vivo para o Servidor de imagem. Lembre-se do que precisa ser publicado: as várias camadas de ativo de imagem, as fontes do texto dinâmico e o próprio modelo. Semelhante a outros ativos de mídia avançada do Dynamic Media Classic, como Conjuntos de imagens e Conjuntos de rotação, um modelo básico é uma construção artificial — é um item de linha no banco de dados que faz referência às imagens e fontes usando uma série de comandos de Exibição de imagem. Portanto, ao publicar o modelo, tudo o que você está fazendo é atualizar os dados no Servidor de imagem.

Saiba mais sobre como [Publicar seu modelo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/publishing-templates.html).

### Construção do URL do modelo

Um modelo básico tem a mesma sintaxe de URL essencial que uma chamada de imagem normal, conforme explicado anteriormente. Um modelo normalmente terá mais modificadores — comandos separados por um E comercial (&amp;) — como parâmetros com valores. No entanto, a principal diferença é que você chama o modelo como a imagem principal, em vez de chamar para uma imagem estática.

![imagem](assets/basic-templates/basic-templates-11.jpg)

Ao contrário das Predefinições de imagem, que têm um cifrão ($) em cada lado do nome predefinido, os parâmetros têm um cifrão único no início. A colocação desses sinais de dólar é importante.

**Correto:**

`$text=46-inch LCD HDTV`

**Incorreto:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Como observado anteriormente, os parâmetros são usados para alterar o modelo. Se você chamar o modelo sem parâmetros, ele reverterá para as configurações padrão, conforme projetado na ferramenta de criação de Noções básicas sobre modelo . Se uma propriedade não precisar ser alterada, não será necessário definir um parâmetro.

![](assets/basic-templates/sandals-without-with-parameters.png)
_imageExamples de um template sem definir parâmetros (acima) e com parâmetros (abaixo)._
