---
title: Introdução aos modelos básicos
description: Saiba mais sobre os modelos básicos no Dynamic Media Classic, modelos baseados em imagem chamados a partir do Servidor de imagens e que consistem em imagens e texto renderizado. Um modelo pode ser alterado dinamicamente pelo URL após a publicação do modelo. Você aprenderá a carregar um Photoshop PSD para o Dynamic Media Classic para usá-lo como a base de um modelo. Crie um Modelo básico de comercialização simples que consiste em camadas de imagem. Adicione camadas de texto e torne-as variáveis por meio do uso de parâmetros. Construa um URL de modelo e manipule a imagem dinamicamente pelo navegador da Web.
sub-product: dynamic-media
feature: templates
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '6301'
ht-degree: 0%

---


# Introdução aos modelos básicos {#basic-templates}

Em termos do Dynamic Media Classic, um modelo é um documento que pode ser alterado dinamicamente pelo URL após a publicação do modelo. Modelos básicos do oferta Dynamic Media Classic, modelos baseados em imagem chamados a partir do Servidor de imagens e que consistem em imagens e texto renderizado.

Um dos aspectos mais avançados dos modelos é que eles têm pontos de integração diretos que permitem que você os vincule ao seu banco de dados. Portanto, não somente você pode servir uma imagem e redimensioná-la, como também pode query seu banco de dados para encontrar itens novos ou de venda e fazer com que eles apareçam como uma sobreposição na imagem. Você pode solicitar uma descrição do item e fazer com que ele apareça como um rótulo em uma fonte escolhida e um layout. As possibilidades são ilimitadas.

Modelos básicos podem ser implementados de várias maneiras diferentes, de simples a complexos. Por exemplo:

- Comercialização básica. Usa rótulos como &quot;frete gratuito&quot; se o produto tiver frete gratuito. Essas etiquetas são configuradas pela equipe de mercadorias no Photoshop e a Web usa a lógica para saber quando aplicá-las à imagem.
- Comercialização avançada. Cada modelo tem várias variáveis e pode mostrar mais de uma opção ao mesmo tempo. Usa um banco de dados, inventário e regras de negócios para determinar quando mostrar um produto como &quot;Apenas para dentro&quot;, &quot;Limpeza&quot; ou &quot;Sold Out&quot;. Também pode usar a transparência atrás do produto para exibi-lo em diferentes planos de fundo, como em salas diferentes. Os mesmos modelos e/ou ativos podem ser redefinidos na página de detalhes do produto para mostrar uma versão maior ou com zoom do mesmo produto em diferentes planos de fundo.

É importante entender que o Dynamic Media Classic fornece apenas a parte visual desses aplicativos baseados em modelos. As empresas do Dynamic Media Classic ou seus parceiros de integração devem fornecer as regras de negócios, o banco de dados e as habilidades de desenvolvimento para criar os aplicativos. Não existe nenhuma aplicação de modelo &quot;integrada&quot;; os designers configuram o modelo no Dynamic Media Classic, e os desenvolvedores usam chamadas de URL para alterar as variáveis no modelo.

Ao final desta seção do tutorial, você saberá como:

- Carregue um Photoshop PSD no Dynamic Media Classic para usá-lo como base de um modelo.
- Crie um Modelo básico de comercialização simples que consiste em camadas de imagem.
- Adicione camadas de texto e torne-as variáveis por meio do uso de parâmetros.
- Construa um URL de modelo e manipule a imagem dinamicamente pelo navegador da Web.

>[!NOTE]
>
>Todos os URLs neste capítulo são apenas para fins ilustrativos; não são ligações ao vivo.

## Visão Geral de Modelos Básicos

A definição de um Modelo básico (ou apenas &quot;modelo&quot;, para abreviação) é uma imagem em camadas endereçável para URL. O resultado final é uma imagem, mas que pode ser alterada pelo URL. Pode consistir em fotografias, texto ou gráficos. qualquer combinação de ativos P-TIFF no Dynamic Media Classic.

Os modelos são mais semelhantes aos arquivos PSD da Photoshop, pois têm um fluxo de trabalho semelhante e recursos semelhantes.

- Ambas consistem em camadas que são como folhas de acetato empilhado. É possível compor imagens parcialmente transparentes e ver as áreas transparentes de uma camada até as camadas abaixo.
- As camadas podem ser movidas e giradas para reposicionar o conteúdo, e os modos de opacidade e mesclagem podem ser alterados para tornar o conteúdo parcialmente transparente.
- É possível criar camadas baseadas em texto. A qualidade pode ser muito alta porque o Servidor de imagens usa o mesmo mecanismo de texto que o Photoshop e o Illustrator.
- Estilos de camada simples podem ser aplicados a cada camada para criar efeitos especiais, como sombras projetadas ou brilhos.

No entanto, diferentemente dos PSDs do Photoshop, as camadas podem ser totalmente dinâmicas e controladas por meio de um URL no Servidor de imagens.

- É possível adicionar variáveis a todas as propriedades do modelo, facilitando a alteração da composição dinamicamente.
- As variáveis chamadas de parâmetros permitem que você exponha apenas a parte do modelo que deseja alterar.

Você só precisa adicionar um espaço reservado para cada camada que variará em vez de colocar todas as camadas em um único arquivo como faz no Photoshop e exibi-las e ocultá-las (embora você também possa fazer isso, se preferir).

Usando um espaço reservado, é possível trocar dinamicamente o conteúdo de uma camada por outro ativo publicado, e as mesmas propriedades (como tamanho e rotação) da camada que ela substituiu serão automaticamente usadas.

Como os Modelos básicos são normalmente projetados no Photoshop, mas implantados por meio de um URL, um projeto de modelo requer uma combinação de habilidades técnicas e de design. Nós geralmente presumimos que a pessoa que faz o trabalho de modelo criativo é um designer da Photoshop, e a pessoa que implementa o modelo é um desenvolvedor da Web. As equipes de criação e desenvolvimento devem trabalhar em estreita colaboração para que o modelo seja bem-sucedido.

Os projetos modelo podem ser relativamente simples ou extremamente complexos dependendo das regras de negócios e das necessidades do aplicativo. Os modelos básicos são chamados do servidor de imagens, no entanto, devido à flexibilidade do ambiente do Dynamic Media Classic, você pode até aninhar modelos dentro de outros modelos, permitindo que você crie imagens bastante complexas que podem ser vinculadas por variáveis comumente nomeadas.

- Saiba mais sobre [Noções básicas sobre o modelo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/quick-start-template-basics.html).
- Saiba como criar um [Modelo básico](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#creating_a_template).

## Criando um Modelo Básico

Ao trabalhar com um modelo básico, você geralmente segue as etapas do fluxo de trabalho no diagrama abaixo. As etapas marcadas com linhas pontilhadas são opcionais se você estiver usando camadas de texto dinâmicas e são indicadas nas instruções abaixo como &quot;Fluxo de trabalho de texto&quot;. Se não estiver usando texto, siga somente o caminho principal.

![imagem](assets/basic-templates/basic-templates-1.png)

_O fluxo de trabalho do modelo básico._

1. Crie e crie seus ativos. A maioria dos usuários faz isso no Adobe Photoshop. Projete ativos com o tamanho exato de que precisa. se for uma imagem de 200 pixels para uma página em miniatura, crie-a em 200 pixels. Se precisar aplicar zoom nele, projete-o em um tamanho de cerca de 2.000 pixels. Use o Photoshop (e/ou o Illustrator salvo como bitmap) para criar os ativos e use o Dynamic Media Classic para reunir as peças, gerenciar as camadas e adicionar variáveis.
2. Depois de projetar os ativos gráficos, carregue-os no Dynamic Media Classic. Em vez de carregar ativos individuais do PSD, recomendamos que você carregue todo o arquivo PSD em camadas e que o Dynamic Media Classic crie um arquivo por camada, usando a opção **Manter camadas** no upload (consulte abaixo para obter mais detalhes). _Fluxo de trabalho de texto: Se estiver criando um texto dinâmico, faça upload de suas fontes também. O texto dinâmico é variável e controlado pelo URL. Se o seu texto estiver estático ou tiver apenas algumas frases curtas que não mudam... por exemplo, tags que dizem &quot;Novo&quot; ou &quot;Venda&quot;, em vez de &quot;X% Desativado&quot;, com o X sendo um número variável. recomendamos pré-renderizar o texto no Photoshop e fazer upload de camadas rasterizadas como imagens. Será mais fácil e você poderá criar o estilo do texto exatamente como desejar._
3. Crie o modelo no Dynamic Media Classic usando o editor de fundamentos do modelo do menu Criar e adicione camadas de imagem. Fluxo de trabalho de texto: Crie camadas de texto no mesmo editor. Esta etapa é necessária ao criar um modelo manualmente no Dynamic Media Classic. Escolha um tamanho de tela que corresponda ao seu design, arraste e solte imagens na tela e defina as propriedades da camada (tamanho, rotação, opacidade, etc.). Não está colocando todas as camadas possíveis no modelo, apenas um espaço reservado por camada de imagem. _Fluxo de trabalho de texto: Crie camadas de texto com a ferramenta Texto, de modo semelhante à criação de camadas de texto no Photoshop. É possível escolher uma fonte e estilo usando as mesmas opções disponíveis com a ferramenta Photoshop Type._ Outro fluxo de trabalho é carregar um PSD e fazer com que o Dynamic Media Classic gere um modelo &quot;gratuito&quot;, podendo até mesmo recriar camadas de texto. Esta questão será discutida mais pormenorizadamente mais tarde.
4. Depois que as camadas forem criadas, adicione parâmetros (variáveis) a qualquer propriedade de qualquer camada que você gostaria de controlar por meio do URL, incluindo a origem da camada (a própria imagem ). _Fluxo de trabalho de texto: Também é possível adicionar parâmetros às camadas de texto, tanto para controlar o conteúdo do texto, o tamanho e a posição da própria camada, como todas as opções de formatação, como cor da fonte, tamanho da fonte, rastreamento horizontal etc._
5. Crie uma predefinição de imagem que corresponda ao tamanho do modelo. Recomendamos fazer isso para que o modelo seja sempre chamado em um tamanho 1:1 e também para adicionar nitidez a qualquer camada de imagem grande que seja redimensionada para se ajustar ao modelo. Se você estiver criando um modelo para ser ampliado, essa etapa será desnecessária.
6. Publique, copie o URL da pré-visualização do Dynamic Media Classic e teste-o em um navegador.

## Preparação e upload dos ativos de modelo para o Dynamic Media Classic

Antes de fazer upload dos ativos de modelo para o Dynamic Media Classic, você precisará concluir algumas etapas preparatórias.

### Preparação do PSD para upload

Antes de fazer upload do arquivo Photoshop para o Dynamic Media Classic, simplifique as camadas no Photoshop para facilitar o trabalho e a maior compatibilidade com o Servidor de imagens. Seu arquivo PSD geralmente será composto de muitos elementos que o Dynamic Media Classic não reconhece, e você também pode acabar com muitas peças que são difíceis de gerenciar. Certifique-se de salvar um backup do seu PSD principal, caso precise editar o original posteriormente. Você carregará a cópia simplificada, e não a principal.

![imagem](assets/basic-templates/basic-templates-2.jpg)

1. Simplifique a estrutura da camada unindo/nivelando camadas relacionadas que precisam ser ativadas/desativadas juntas em uma única camada. Por exemplo, o rótulo &quot;NOVO&quot; e o banner azul são mesclados em uma única camada para que você possa mostrá-los ou ocultá-los com um único clique.
   ![imagem](assets/basic-templates/basic-templates-3.jpg)
2. Alguns tipos de camadas e efeitos de camadas não são suportados pelo Dynamic Media Classic ou pelo Image Server e precisam ser rasterizados antes do upload. Caso contrário, os efeitos podem ser ignorados ou as camadas descartadas. Rasterizar uma camada significa converter se de editável em não editável. Para rasterizar efeitos de camada ou camadas de texto, crie uma camada vazia, selecione ambas e mescle usando **Camadas > Mesclar camadas** ou CTRL + E/CMD + E.

   - O Dynamic Media Classic não pode agrupar ou vincular camadas. Todas as camadas em um grupo ou conjunto vinculado serão convertidas em camadas separadas que não estão mais agrupadas/vinculadas.
   - As máscaras de camada serão convertidas em transparência no upload.
   - As camadas de ajuste não são suportadas e serão descartadas.
   - As camadas de preenchimento, como as camadas de Cor sólida, serão rasterizadas.
   - As camadas de objetos inteligentes e as camadas vetoriais serão rasterizadas em imagens normais no upload e os Filtros inteligentes serão aplicados e rasterizados.
   - As camadas de texto também serão rasterizadas a menos que você use a opção Extrair texto — consulte abaixo para obter mais informações.
   - A maioria dos efeitos de camada serão ignorados e apenas alguns modos de mesclagem são suportados. Em caso de dúvida, adicione efeitos simples no Dynamic Media Classic (como sombras internas ou soltas, brilhos internos ou externos) ou use uma camada em branco para unir e rasterizar o efeito no Photoshop.

### Trabalhar com fontes

Você também fará upload e publicará suas fontes se precisar gerar texto dinâmico. A única fonte incluída no Dynamic Media Classic é Arial.

Cada empresa é responsável por obter uma licença para usar uma fonte na internet. simplesmente ter uma fonte instalada em seu computador não dá a você o direito de usá-la comercialmente na Web, e sua empresa pode enfrentar uma ação legal do editor de fontes se usada sem permissão. Além disso, os termos de licença variam. você pode precisar de licenças separadas para impressão versus tela de exibição, por exemplo.

O Dynamic Media Classic é compatível com fontes padrão OpenType (OTF), TrueType (TTF) e Tipo 1 Postscript. Não há suporte para fontes de mala, arquivos de coleção de tipos, fontes do sistema Windows e fontes de máquinas proprietárias (como fontes usadas por gravadoras ou máquinas bordados) apenas para Mac — todas elas não são suportadas — será necessário convertê-los em um dos formatos de fonte padrão ou substituir uma fonte semelhante para uso no Dynamic Media Classic e no Image Server.

Depois que as fontes são carregadas no Dynamic Media Classic, como qualquer outro ativo, elas também devem ser publicadas no Servidor de imagens. Um erro de modelo muito comum é esquecer de publicar suas fontes, o que resultará em um erro de imagem. o Servidor de imagens não substituirá outra fonte em seu lugar. Além disso, se você quiser usar a opção **Extrair texto** ao fazer upload, faça upload dos arquivos de fonte antes de fazer upload do PSD que usa essas fontes. O recurso **Extrair texto** tentará recriar o texto como uma camada de texto editável e colocá-lo dentro de um modelo do Dynamic Media Classic. Isso é discutido no próximo tópico, Opções de PSD.

Lembre-se de que as fontes têm vários nomes internos que são geralmente diferentes do nome de arquivo externo. Você pode ver todos os nomes diferentes na página Detalhes desse ativo no Dynamic Media Classic. Estes são os nomes da fonte Adobe Caslon Pro Semibold, listada na guia Metadados no Dynamic Media Classic:

![imagem](assets/basic-templates/basic-templates-4.jpg)

_Guia Metadados na página Detalhes de uma fonte no Dynamic Media Classic._

O Dynamic Media Classic usa o nome de arquivo dessa fonte (ACaslonPro-Semibold) como sua ID de ativo, no entanto, esse não é o nome usado pelo modelo. O modelo usa o nome Rich Text Format (RTF), listado na parte inferior. RTF é o &quot;idioma&quot; nativo do mecanismo de texto do Servidor de imagens.

Se for necessário alterar as fontes por meio do URL, você deve chamar o nome RTF da fonte (não a ID do ativo) ou aparecerá um erro. Nesse caso, o nome adequado para essa fonte seria &quot;Adobe Caslon Pro&quot;. Discutiremos mais sobre fontes e RTF nos tópicos RTF e Parâmetros de texto, abaixo.

Os formatos de arquivo de fonte mais comuns encontrados em sistemas Windows e Mac são OpenType e TrueType. OpenType tem uma extensão .OTF, enquanto TrueType é .TTF. Ambos os formatos funcionam bem no Dynamic Media Classic.

### Selecionar opções ao carregar seu PSD

Não é necessário carregar um arquivo Photoshop (PSD) para criar um modelo; um modelo pode ser construído a partir de qualquer ativo de imagem no Dynamic Media Classic. No entanto, fazer upload de um PSD pode facilitar a criação, pois você já terá esses ativos em um PSD em camadas. Além disso, o Dynamic Media Classic gera automaticamente um modelo quando você carrega um PSD em camadas.

- **Manter camadas.** Esta é a opção mais importante. Isso instrui o Dynamic Media Classic a criar um ativo de imagem por camada do Photoshop. Se desmarcada, todas as outras opções serão desativadas e o PSD será nivelado em uma única imagem.
- **** **CreateTemplate.** Essa opção pega as várias camadas geradas e cria automaticamente um modelo ao combiná-las novamente. Uma desvantagem para usar o modelo gerado automaticamente é que o Dynamic Media Classic coloca todas as camadas em um arquivo, ao passo que só precisamos de um único espaço reservado por camada. É fácil o suficiente excluir as camadas extras, mas se você tiver muitas camadas, é mais rápido recriá-las. Certifique-se de renomear o novo modelo; caso contrário, ele será substituído na próxima vez que você carregar novamente o mesmo PSD.
- **Extrair texto.** Isso recria camadas de texto no PSD como camadas de texto no modelo usando a fonte carregada. Essa etapa é necessária se o texto estiver em um caminho no Photoshop e você quiser manter esse caminho no modelo. Este recurso exige que você use a opção **Criar modelo**, já que o texto extraído só pode ser criado por um modelo gerado no upload.
- **Estende as camadas para o tamanho do plano de fundo.** Essa configuração faz com que cada camada tenha o mesmo tamanho da tela PSD geral. Isso é muito útil para camadas que sempre permanecerão fixas na posição: caso contrário, ao trocar imagens na mesma camada, talvez seja necessário reposicioná-las.
- **Nomenclatura de camada.** Isso diz ao Dynamic Media Classic como nomear cada ativo gerado por camada. Recomendamos **Photoshop** **e Layer** **Name** ou Photoshop e **Layer** **Number**. Ambas as opções usam o nome PSD como a primeira parte do nome e adicionam o nome ou o número da camada no final. Por exemplo, se você tiver um PSD chamado &quot;shirt.psd&quot; e ele tiver camadas chamadas &quot;front&quot;, &quot;truques&quot; e &quot;collar&quot;, se fizer upload usando a opção **Photoshop e** Camada **Name**, o Dynamic Media Classic gerará as IDs de ativo &quot;shirt_front&quot;, &quot;shirt_sleves&quot; e &quot;shirt_collar .&quot; Usar uma dessas opções ajuda a garantir que o nome seja exclusivo no Dynamic Media Classic.

## Criação de um modelo com camadas de imagem

Embora o Dynamic Media Classic possa criar automaticamente um modelo a partir de um PSD em camadas, você deve saber como criar o modelo manualmente. Como explicado acima, algumas vezes você não deseja usar o modelo criado pelo Dynamic Media Classic.

### A interface básica do modelo

Primeiramente, familiarizemos-nos com a interface de edição.

No centro esquerdo, sua área de trabalho mostra uma pré-visualização do modelo final. No lado direito estão os painéis Camadas e Propriedades da camada. Estas áreas são onde você vai trabalhar mais.

![imagem](assets/basic-templates/basic-templates-5.jpg)

_Página Criar fundamentos do modelo._

- **Pré-visualização/Área de trabalho.** Esta é a janela principal. Aqui você pode mover, redimensionar e girar camadas com o mouse. Os contornos de camada são mostrados como linhas tracejadas.
- **Camadas.** É semelhante ao painel de camadas do Photoshop. À medida que você adiciona camadas ao modelo, elas serão exibidas aqui. As camadas são empilhadas de cima para baixo. a camada superior no painel Camadas será vista acima das outras abaixo na lista.
- **Propriedades da camada.** Aqui, você pode ajustar todas as propriedades de uma camada usando controles numéricos. Primeiro, selecione uma camada e ajuste suas propriedades.
- **** **CompositeURL.** Na parte inferior da interface do usuário está a área de URL composto. Isso não será discutido nesta seção do tutorial, no entanto, aqui você verá seu modelo desconstruído como uma série de modificadores de URL do Servidor de imagens. Esta área é editável. se você estiver familiarizado com os comandos do Servidor de imagens, poderá editar manualmente o modelo aqui. No entanto, você também pode quebrá-lo. Como o Photoshop, start de numeração de camadas em 0. A tela de desenho é a camada 0 e a primeira camada que você mesmo adicionar é a camada 1. Os modos de mesclagem determinam como os pixels de uma camada se mesclam com os pixels abaixo dela. Você pode criar uma variedade de efeitos especiais usando modos de mesclagem.

#### Uso do Editor de fundamentos de modelo

Estas são as etapas do fluxo de trabalho para start do seu Modelo básico:

1. No Dynamic Media Classic, vá para **Criar > Noções básicas do modelo**. Você não pode ter nada selecionado ou start selecionando uma imagem, que se tornará a primeira camada do modelo.
2. Escolha um Tamanho e pressione **OK**. Esse tamanho deve corresponder ao tamanho projetado no Photoshop. O editor de modelos será carregado.
3. Se você não tiver uma imagem selecionada na etapa 1, procure ou navegue até uma imagem no painel de ativos à esquerda e arraste-a para a área de trabalho.

   - A imagem será redimensionada automaticamente para se ajustar ao tamanho da tela de desenho. Se você planeja trocar suas imagens de alta resolução, então você normalmente traria uma de suas grandes imagens (2000 px) P-TIFF e a usaria como espaço reservado.
   - Essa deve ser a camada mais inferior do modelo, no entanto, é possível reordenar as camadas mais tarde.

4. Redimensione ou reposicione a camada diretamente na área de trabalho ou ajustando as configurações no painel Propriedades da camada.
5. Arraste outras camadas de imagem conforme necessário. Adicione efeitos de camadas, se desejar. Consulte o tópico _Adicionar efeitos de camada_, abaixo.
6. Clique em **Salvar**, escolha um local e dê um nome ao modelo. Podem pré-visualização, no entanto neste momento o vosso modelo será exatamente como uma imagem Photoshop achatada. ainda não é alterável.

### Adicionar efeitos de camada

O Image Server suporta alguns efeitos de camada programáticos. efeitos especiais que alteram a aparência do conteúdo de uma camada. Eles funcionam de forma semelhante aos efeitos de camada no Photoshop. Eles são acoplados a uma camada, mas controlados independentemente da camada. É possível ajustá-los ou removê-los sem fazer uma alteração permanente na própria camada.

- **Sombra**. Aplica uma sombra fora dos limites da camada, posicionada por um deslocamento de x e y pixels.
- **Sombra** interna. Aplica uma sombra dentro dos limites da camada, posicionada por um deslocamento de x e y pixels.
- **Brilho** externo. Aplica um efeito de brilho uniformemente em todas as bordas da camada.
- **Brilho** interno. Aplica um efeito de brilho igualmente dentro de todas as bordas da camada.

![imagem](assets/basic-templates/basic-templates-6.jpg)

_Uma camada com e sem uma sombra projetada_

Para adicionar um efeito, clique em **Adicionar efeito** e escolha um efeito no menu. Como as camadas normais, você pode selecionar um efeito no painel Camadas e usar o painel Propriedades da camada para ajustar suas configurações.

Os efeitos de sombra são deslocados horizontal ou verticalmente para longe da camada, enquanto os efeitos de Brilho são aplicados uniformemente em todas as direções. Os efeitos internos atuam sobre as partes opacas da camada, enquanto os efeitos externos afetam apenas as áreas transparentes.

Saiba mais sobre[Adicionar efeitos de camada](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#using-shadow-and-glow-effects-on-layers).

### Adicionar parâmetros

Se tudo o que você faz é combinar camadas e salvá-las, o resultado líquido não é diferente de uma imagem nivelada do Photoshop. O que torna os modelos especiais é a capacidade de adicionar parâmetros às propriedades de cada camada, para que possam ser alterados dinamicamente pelo URL.

Em termos do Dynamic Media Classic, um parâmetro é uma variável que pode ser vinculada a uma propriedade de modelo para que possa ser manipulada por um URL. Quando você adiciona um parâmetro a uma camada, o Dynamic Media Classic expõe essa propriedade no URL prefixando o nome do parâmetro com um sinal de dólar ($) — por exemplo, se você criar um parâmetro chamado &quot;tamanho&quot; para alterar o tamanho de uma camada, o Dynamic Media Classic renomeará seu parâmetro como $size.

Se você não adicionar um parâmetro para uma propriedade, essa propriedade permanecerá oculta no banco de dados do Dynamic Media Classic e não será exibida no URL.

![imagem](assets/basic-templates/parameters.png)

Sem parâmetros, seus URLs normalmente seriam muito mais longos, especialmente se você também estiver usando texto dinâmico. O texto adiciona dezenas de caracteres extras a cada URL.

Finalmente, seu conjunto inicial de parâmetros se tornará os valores padrão das propriedades no modelo. Se você criar seu modelo, adicionar parâmetros e, em seguida, chamar o URL sem seus parâmetros, o Servidor de imagens criará a imagem com todos os padrões salvos no modelo. Os parâmetros são necessários somente se você deseja alterar uma propriedade. Se uma propriedade não precisar ser alterada, não será necessário definir um parâmetro.

#### Criação de parâmetros

Este é o fluxo de trabalho para criar parâmetros:

1. Clique no botão **Parâmetros** ao lado do nome da camada para a qual você deseja criar parâmetros. A tela Parâmetros é aberta. Ele lista cada propriedade na camada e seu valor.
1. Selecione a opção **On** ao lado do nome de cada propriedade que você deseja transformar em um parâmetro. Um nome de parâmetro padrão será exibido. Você só pode adicionar parâmetros a propriedades que foram alteradas a partir de seu estado padrão.

   - Por exemplo, se você adicionar uma camada e mantê-la em sua posição de proxy padrão de 0,0, o Dynamic Media Classic não exporá uma propriedade **Position**. Para corrigir, mova a camada pelo menos um pixel. Agora o Dynamic Media Classic exporá **Position** como uma propriedade que você pode parametrizar.
   - Para adicionar um parâmetro à propriedade show/hide (que ativa e desativa a camada ), clique no ícone **Mostrar** ou **Ocultar camada** para desligar a camada (você pode ativá-la novamente depois, se desejar). O Dynamic Media Classic agora expõe uma propriedade **Ocultar** que pode ser parametrizada.

1. Renomeie os nomes dos parâmetros padrão para algo que será mais fácil de identificar no URL. Por exemplo, se você quiser adicionar um parâmetro para alterar a camada do banner na parte superior de uma imagem, altere o nome padrão de &quot;layer_2_src&quot; para &quot;banner&quot;.
1. Pressione **Close** para sair da tela Parâmetros.
1. Repita esse processo para outras camadas clicando no botão **Parâmetros** e adicionando e renomeando parâmetros.
1. Salve as alterações quando terminar.

>[!TIP]
>
>Renomeie seus parâmetros para algo significativo e desenvolva uma convenção de nomenclatura para padronizar esses nomes. Certifique-se de que a convenção de nomenclatura seja previamente acordada pelas equipes de projeto e desenvolvimento.
>
>Não é possível adicionar um parâmetro porque você não vê a propriedade? Basta alterar a propriedade da camada do padrão (movendo, redimensionando, ocultando etc.). Agora você deve ver essa propriedade exposta.

Saiba mais sobre [Parâmetros de Modelo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html).

## Criação de um modelo com camadas de texto

Agora você aprenderá a criar um Modelo básico que inclua camadas de texto.

### Como entender o texto dinâmico

Agora você sabe como criar um modelo básico usando camadas de imagem. Para muitos aplicativos, isso é tudo o que você precisa. Como você viu no exercício anterior, as camadas com texto simples (como &quot;Venda&quot; e &quot;Novo&quot;) podem ser rasterizadas e tratadas como imagens, pois o texto não precisa ser alterado.

No entanto, e se você precisasse:

- Adicione um rótulo para dizer &quot;25% desligado&quot;, com o valor de 25% variável
- Adicione um rótulo de texto com o nome do produto na parte superior da imagem
- Localize suas camadas em diferentes idiomas, dependendo do país em que seu modelo é visto

Nesse caso, você gostaria de adicionar algumas camadas de texto dinâmicas com parâmetros para controlar o texto e/ou a formatação.

Para criar texto, é necessário carregar algumas fontes. caso contrário, o Dynamic Media Classic assumirá o padrão Arial. As fontes também devem ser publicadas no Servidor de imagens ou gerarão um erro no momento em que tentarem renderizar qualquer texto que use essa fonte.

### Parâmetros de RTF e Texto

Para adicionar variáveis ao texto usando a ferramenta Informações básicas sobre modelo, você deve entender como o texto é renderizado. O Servidor de imagens gera texto usando o Mecanismo de texto Adobe, o mesmo mecanismo usado pela Photoshop e Illustrator, e o compõe como uma camada na imagem final. Para se comunicar com o mecanismo, o Servidor de imagens usa Rich Text Format ou RTF.

RTF é uma especificação de formato de arquivo desenvolvida pela Microsoft para especificar a formatação de documentos. É uma linguagem de marcação padrão usada pela maioria dos softwares de processamento de texto e e-mail. Se você gravasse em um URL &amp;text=\b1 Hello, o Servidor de imagens geraria uma imagem com a palavra &quot;Hello&quot; em negrito, pois \b1 é o comando RTF para tornar o texto em negrito.

A boa notícia é que o Dynamic Media Classic gera o RTF para você. Sempre que você digita um texto em um modelo e adiciona formatação, o Dynamic Media Classic grava silenciosamente o código RTF no modelo automaticamente. A razão pela qual o mencionamos é porque você vai adicionar parâmetros diretamente ao próprio RTF, portanto é importante que você esteja um pouco familiarizado com ele.

#### Criação de camadas de texto

Você pode criar camadas de texto em um modelo no Dynamic Media Classic das duas maneiras a seguir:

1. Ferramenta de texto no Dynamic Media Classic. Vamos discutir este método abaixo. O Editor de fundamentos do modelo tem uma ferramenta que permite criar uma caixa de texto, inserir texto e formatar o texto. O Dynamic Media Classic gera o RTF conforme necessário e o coloca em uma camada separada.
2. Extrair texto (no upload). O outro método é criar a camada de texto no Photoshop e salvá-la no PSD como uma camada de texto normal (em vez de rasterizá-la como uma camada de imagem). Em seguida, carregue o arquivo no Dynamic Media Classic e use a opção **Extrair texto**. O Dynamic Media Classic converterá cada camada de texto do Photoshop em uma camada de texto do Servidor de imagens usando comandos RTF. Se você usar esse método, primeiro faça upload das fontes para o Dynamic Media Classic. Caso contrário, o Dynamic Media Classic substituirá uma fonte padrão no upload e não haverá uma maneira fácil de substituir a fonte correta.

### O Editor de texto

Insira o texto usando o Editor de texto. O Editor de texto é uma interface WYSIWYG que permite que você insira e formate o texto usando controles de formatação semelhantes aos do Photoshop ou Illustrator.

![imagem](assets/basic-templates/basic-templates-9.jpg)

_Editor de texto de fundamentos do modelo._

Você fará a maior parte do seu trabalho na guia **Pré-visualização**, que permite que você insira o texto e o visualize como ele aparecerá no modelo. Também há uma guia **Source**, que é usada para editar manualmente o RTF, se necessário.

O fluxo de trabalho geral é usar a guia **Pré-visualização** para digitar algum texto.

Em seguida, selecione o texto e escolha alguma formatação, como cor da fonte, tamanho da fonte ou justificação, usando os controles na parte superior. Depois que o texto tiver o estilo desejado, clique em **Aplicar** para vê-lo atualizado na pré-visualização da área de trabalho. Em seguida, feche o Editor de texto para voltar à janela principal Informações básicas sobre o modelo.

#### Uso do Editor de texto

Estas são as etapas do fluxo de trabalho para adicionar texto dentro da página de criação Informações básicas sobre modelo:

1. Clique no botão da ferramenta **Texto** na parte superior da página de criação.
2. Arraste uma caixa de texto onde deseja que o texto apareça. A janela do Editor de texto será aberta em uma janela modal. Em segundo plano, você verá seu modelo, no entanto, ele não poderá ser editado até que você conclua a edição do texto.
3. Digite o texto de amostra que deseja exibir quando o modelo for carregado pela primeira vez. Por exemplo, se você estiver criando uma caixa de texto para uma imagem de email personalizada, seu texto poderá dizer &quot;Hi Name&quot;. Agora é hora de salvar!&quot; Posteriormente, você adicionaria um parâmetro de texto para substituir Nome por um valor enviado no URL. Seu texto não aparecerá no modelo abaixo da janela até que você clique em **Aplicar**.
4. Para formatar o texto, selecione-o arrastando com o mouse e escolha um controle de formatação na interface do usuário.

   - Há muitas opções de formatação. Alguns dos mais comuns são fonte (face), tamanho de fonte e cor de fonte, bem como justificação esquerda/centro/direita.
   - Não se esqueça de selecionar o texto primeiro. Caso contrário, não será possível aplicar nenhuma formatação.
   - Para escolher uma fonte diferente, selecione o texto e abra o menu Fonte. O editor mostrará uma lista de todas as fontes carregadas no Dynamic Media Classic. Se uma fonte também estiver instalada no computador, ela aparecerá em preto. Se não estiver instalado no computador, será exibido em vermelho. No entanto, ele ainda será renderizado na janela pré-visualização quando você clicar em **Aplicar**. Você só precisa carregar fontes no Dynamic Media Classic para disponibilizá-las a qualquer pessoa usando o Dynamic Media Classic. Após a publicação, o Servidor de imagens usará essas fontes para gerar o texto. os usuários não precisam instalar fontes para ver o texto criado por fazer parte de uma imagem.
   - Diferentemente do Photoshop e do Illustrator, o Servidor de imagens pode alinhar o texto verticalmente na caixa de texto. O padrão é o alinhamento superior. Para alterar isso, selecione o texto e escolha **Médio** ou **Inferior** no menu **Alinhamento Vertical**.
   - Se o texto ficar grande demais para a caixa (ou se a caixa de texto estiver muito pequena), todo ou parte dele será cortado e desaparecerá. Reduza o tamanho da fonte ou torne a caixa maior.

5. Clique em **Aplicar** para ver as alterações entrarem em vigor na janela da área de trabalho. Você deve clicar em **Aplicar**, caso contrário, as edições serão perdidas.
6. Quando terminar, clique em **Fechar**. Se quiser voltar ao modo de edição, clique com o duplo na camada de texto para reabrir o Editor de texto.

O editor de texto pré-visualização exatamente o tamanho da fonte se você tiver a fonte instalada localmente no sistema.

### Sobre a adição de parâmetros a camadas de texto

Agora seguimos um processo semelhante para adicionar parâmetros de texto como fizemos para parâmetros de camada. As camadas de texto também podem usar parâmetros de camada para tamanho, posição e assim por diante; no entanto, eles podem usar parâmetros adicionais que permitem controlar qualquer aspecto do RTF.

Diferentemente dos parâmetros da camada, você seleciona apenas o valor que deseja alterar e adiciona um parâmetro a ele, em vez de adicionar um parâmetro à propriedade inteira.

![imagem](assets/basic-templates/basic-templates-10.jpg)

Exemplo de RTF:

![imagem](assets/basic-templates/sample-rtf.png)

Ao examinar o RTF, é necessário descobrir onde cada configuração é que você deseja alterar. No RTF acima, alguns deles podem fazer algum sentido e você pode ver de onde a formatação vem.

Podem ver a frase Chocolate Mint Sandal. esse é o próprio texto.

- Há uma referência à fonte Poor Richard. é aqui que as fontes são selecionadas.
- Você pode ver um valor RGB: \red56\green53\blue4 — esta é a cor do texto.
- Embora o tamanho da fonte seja 20, você não vê o número 20. No entanto, você verá um comando \fs40 — por algum motivo estranho, o RTF mede as fontes como pontos intermediários. Portanto, \fs40 é o tamanho da fonte!

Você tem informações suficientes para criar seus parâmetros, no entanto, há uma referência completa de todos os comandos RTF na documentação do Servidor de imagens. Visite a [Documentação de disponibilização de imagem](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html#concept-0d3136db7f6f49668274541cd4b6364c).

#### Adicionar parâmetros a camadas de texto

Estas são as etapas para adicionar parâmetros a camadas de texto.

1. Clique no botão **Parâmetros** (um &quot;P&quot;) ao lado do nome da camada de texto para a qual você deseja criar parâmetros. A tela Parâmetros é aberta. A guia **Comum** lista cada propriedade na camada e seu valor. Aqui você pode adicionar parâmetros de camada regulares.
1. Clique na guia **Texto**. Aqui vocês podem ver o RTF no topo. os parâmetros adicionados estarão abaixo disso.
1. Para adicionar um parâmetro, primeiro destaque o valor que você deseja alterar e clique no botão **Adicionar parâmetro**. Certifique-se de selecionar apenas os valores para comandos e não o comando inteiro em si. Por exemplo, para definir um parâmetro para o nome da fonte na amostra RTF acima, eu destacaria apenas &quot;Poor Richard&quot; e adicionaria um parâmetro a isso, mas não também &quot;\f0&quot;. Quando você clica em **Adicionar parâmetro**, ele aparece na lista abaixo e o valor do parâmetro aparecerá em vermelho no RTF enquanto ele ainda estiver selecionado. Se precisar remover um parâmetro, clique na caixa de seleção ao lado de **On** para desativar esse parâmetro e ele desaparecerá.
1. Clique para renomear seu parâmetro para um nome mais significativo.
1. Quando terminar, seu RTF será realçado em verde, onde os parâmetros existem, e seus nomes e valores de parâmetro serão listados abaixo.
1. Clique em **Fechar** para sair da tela Parâmetros. Em seguida, pressione **Save** para salvar o modelo. Se tiver terminado a edição, pressione **Fechar** para sair da página Informações básicas sobre o modelo.
1. Clique em **Pré-visualização** para testar seu modelo no Dynamic Media Classic. Para testar seus parâmetros de texto, digite um novo texto ou novos valores na janela pré-visualização. Para alterar a fonte, digite o nome RTF exato da fonte.

>[!TIP]
>
>Para adicionar parâmetros à cor do texto, adicione separadamente parâmetros para vermelho, verde e azul. Por exemplo, se o RTF for `\red56\green53\blue46`, você adicionaria parâmetros separados vermelho, verde e azul para os valores 56, 53 e 46. No URL, você alteraria a cor chamando os três: `&$red=56&$green=53&$blue=46`.

Saiba como [Criar parâmetros de texto dinâmicos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html#creating-dynamic-text-parameters).

## Publicar e criar URLs de modelo

### Criar uma predefinição de imagem

Criar uma predefinição para o modelo não é uma etapa necessária. Recomendamos que seja uma prática recomendada para que o modelo seja sempre chamado em um tamanho 1:1 e também para adicionar nitidez a qualquer camada de imagem grande que seja redimensionada para se ajustar ao modelo. Se você chamar uma imagem sem uma predefinição, o Servidor de imagens poderá redimensionar arbitrariamente sua imagem para o tamanho padrão (cerca de 400 pixels) e não aplicará a nitidez padrão.

Não há nada especial sobre uma predefinição de imagem para um modelo. Se você já tiver uma predefinição para uma imagem estática do mesmo tamanho, poderá usá-la.

### Publicação

Será necessário executar uma publicação para ver as alterações enviadas ao vivo para o Servidor de imagens. Lembre-se do que precisa ser publicado: as várias camadas de ativos de imagem, as fontes do texto dinâmico e o próprio modelo. Semelhante a outros ativos de mídia avançada do Dynamic Media Classic, como Conjuntos de Imagens e Conjuntos de rotação, um Modelo básico é uma construção artificial. é um item de linha no banco de dados que faz referência às imagens e fontes usando uma série de comandos do Serviço de imagem. Portanto, ao publicar o modelo, tudo o que você está fazendo é atualizar os dados no Servidor de imagens.

Saiba mais sobre [Publicar seu modelo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/publishing-templates.html).

### Construção do URL do modelo

Um Modelo básico tem a mesma sintaxe de URL essencial que uma chamada de imagem normal, como explicado anteriormente. Normalmente, um modelo terá mais modificadores. comandos separados por um E comercial (&amp;) — como parâmetros com valores. No entanto, a principal diferença é que você chama o modelo como sua imagem principal, em vez de chamar para uma imagem estática.

![imagem](assets/basic-templates/basic-templates-11.jpg)

Diferentemente das predefinições de imagem, que têm um cifrão ($) em cada lado do nome predefinido, os parâmetros têm um único cifrão no início. A colocação desses sinais de dólar é importante.

**Correto:**

`$text=46-inch LCD HDTV`

**Incorreto:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Como observado anteriormente, os parâmetros são usados para alterar o modelo. Se você chamar o modelo sem parâmetros, ele reverterá para suas configurações padrão, conforme projetado na ferramenta de criação Fundamentos do modelo. Se uma propriedade não precisar ser alterada, não será necessário definir um parâmetro.

![](assets/basic-templates/sandals-without-with-parameters.png)
_imageExemplos de um modelo sem definir parâmetros (acima) e com parâmetros (abaixo)._
