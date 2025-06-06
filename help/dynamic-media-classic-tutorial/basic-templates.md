---
title: Introdução aos modelos básicos
description: Saiba mais sobre os Modelos básicos no Dynamic Media Classic, modelos baseados em imagem chamados do Servidor de imagens e que consistem em imagens e texto renderizado. Um modelo pode ser alterado dinamicamente por meio da URL após a publicação do modelo. Você aprenderá a fazer upload de um PSD do Photoshop no Dynamic Media Classic para usá-lo como a base de um modelo. Crie um Modelo básico de merchandising simples que consiste em camadas de imagem. Adicione camadas de texto e torne-as variáveis por meio do uso de parâmetros. Construa um URL de modelo e manipule a imagem dinamicamente por meio do navegador da Web.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: d4e16b45-0095-44b4-8c16-89adc15e0cf9
duration: 1338
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '6219'
ht-degree: 0%

---

# Introdução aos modelos básicos {#basic-templates}

Em termos de Dynamic Media Classic, um modelo é um documento que pode ser alterado dinamicamente por meio do URL após a publicação do modelo. O Dynamic Media Classic oferece Modelos básicos, modelos baseados em imagem chamados do Servidor de imagens e que consistem em imagens e texto renderizado.

Um dos aspectos mais eficientes dos modelos é que eles têm pontos de integração diretos que permitem vinculá-los ao banco de dados. Assim, você não só pode servir uma imagem e redimensioná-la, você pode consultar seu banco de dados para encontrar itens novos ou de venda e fazer que ele apareça como uma sobreposição na imagem. Você pode solicitar uma descrição do item e fazer com que ele apareça como um rótulo em uma fonte escolhida e no layout. As possibilidades são ilimitadas.

Os modelos básicos podem ser implementados de várias maneiras diferentes, desde simples até complexos. Por exemplo:

- Merchandising básico. Usa rótulos como &quot;frete grátis&quot; se o produto tiver frete grátis. Essas etiquetas são configuradas pela equipe de produtos no Photoshop e a Web usa a lógica para saber quando aplicá-las à imagem.
- Merchandising avançado. Cada modelo tem várias variáveis e pode mostrar mais de uma opção ao mesmo tempo. Usa um banco de dados, inventário e regras de negócios para determinar quando mostrar um produto como &quot;Acabou de entrar&quot;, &quot;Liberação&quot; ou &quot;Vendido&quot;. Também pode usar a transparência atrás do produto para mostrá-lo em diferentes planos de fundo, como em salas diferentes. Os mesmos modelos e/ou ativos podem ser redefinidos na página de detalhes do produto para mostrar uma versão maior ou com zoom do mesmo produto em diferentes planos de fundo.

É importante entender que o Dynamic Media Classic fornece apenas a parte visual desses aplicativos baseados em modelos. As empresas da Dynamic Media Classic ou seus parceiros de integração devem fornecer as regras de negócios, o banco de dados e as habilidades de desenvolvimento para criar os aplicativos. Não há um aplicativo de modelo &quot;integrado&quot;; os designers configuram o modelo no Dynamic Media Classic e os desenvolvedores usam chamadas de URL para alterar as variáveis no modelo.

Ao final desta seção do tutorial, você saberá como:

- Faça upload de um PSD Photoshop no Dynamic Media Classic para usá-lo como a base de um modelo.
- Crie um Modelo básico de merchandising simples que consiste em camadas de imagem.
- Adicione camadas de texto e torne-as variáveis por meio do uso de parâmetros.
- Construa um URL de modelo e manipule a imagem dinamicamente por meio do navegador da Web.

>[!NOTE]
>
>Todos os URLs neste capítulo são apenas para fins ilustrativos; não são links ativos.

## Visão geral dos modelos básicos

A definição de um modelo básico (ou apenas &quot;modelo&quot;, para abreviar) é uma imagem em camadas endereçável por URL. O resultado final é uma imagem, mas que pode ser alterada pelo URL. Ele pode consistir em fotos, texto ou gráficos — qualquer combinação de ativos TIFF P no Dynamic Media Classic.

Os modelos são mais semelhantes aos arquivos PSD do Photoshop, pois têm um fluxo de trabalho semelhante e recursos semelhantes.

- Ambos consistem em camadas que são como folhas de acetato empilhado. É possível compor imagens parcialmente transparentes e ver através das áreas transparentes de uma camada até as camadas abaixo.
- As camadas podem ser movidas e giradas para reposicionar o conteúdo, e a opacidade e os modos de mesclagem podem ser alterados para tornar o conteúdo parcialmente transparente.
- É possível criar camadas baseadas em texto. A qualidade pode ser muito alta porque o Servidor de imagens usa o mesmo mecanismo de texto que o Photoshop e o Illustrator.
- Estilos de camada simples podem ser aplicados a cada camada para criar efeitos especiais, como sombras projetadas ou brilho.

No entanto, diferentemente dos PSD do Photoshop, as camadas podem ser totalmente dinâmicas e controladas por meio de um URL no Servidor de imagens.

- É possível adicionar variáveis a todas as propriedades do modelo, facilitando a alteração da composição em tempo real.
- As variáveis chamadas de parâmetros permitem expor apenas a parte do modelo que você deseja alterar.

Basta adicionar um espaço reservado para cada camada que varia, em vez de colocar todas as camadas em um único arquivo, como faz no Photoshop, e exibi-las e ocultá-las (embora também seja possível fazer isso, se preferir).

Usando um espaço reservado, você pode alternar dinamicamente o conteúdo de uma camada com outro ativo publicado, e ele assumirá automaticamente as mesmas propriedades (como tamanho e rotação) da camada substituída.

Como os Modelos básicos são normalmente projetados no Photoshop, mas implantados por meio de um URL, um projeto de modelo requer uma mistura de habilidades técnicas e de design. Geralmente, presumimos que a pessoa que faz o trabalho de modelo criativo é um designer do Photoshop e que a pessoa que implementa o modelo é um desenvolvedor da Web. As equipes de criação e desenvolvimento devem trabalhar em estreita colaboração para que o modelo seja bem-sucedido.

Os projetos de modelo podem ser relativamente simples ou extremamente complexos, dependendo das regras de negócios e das necessidades do aplicativo. Os modelos básicos são chamados a partir do Servidor de imagens, no entanto, devido à flexibilidade do ambiente do Dynamic Media Classic, é possível até aninhar modelos dentro de outros modelos, permitindo criar imagens razoavelmente complexas que podem ser vinculadas por variáveis comumente nomeadas.

- Saiba mais sobre [Noções básicas do modelo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/quick-start-template-basics.html?lang=pt-BR).
- Saiba como criar um [Modelo básico](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html?lang=pt-BR#creating_a_template).

## Criação de um modelo básico

Ao trabalhar com um modelo básico, você geralmente segue as etapas do fluxo de trabalho no diagrama abaixo. As etapas marcadas com linhas pontilhadas serão opcionais se estiver usando camadas de texto dinâmicas e serão indicadas nas instruções abaixo como &quot;Fluxo de trabalho de texto&quot;. Se não estiver usando texto, siga somente o caminho principal.

![imagem](assets/basic-templates/basic-templates-1.png)

_O fluxo de trabalho do Modelo Básico._

1. Projete e crie seus ativos. A maioria dos usuários faz isso no Adobe Photoshop. Projete ativos no tamanho exato necessário — se for uma imagem de 200 pixels para uma página em miniatura, projete-a com 200 pixels. Se você precisar ampliar, crie-o com um tamanho de aproximadamente 2000 pixels. Use o Photoshop (e/ou o Illustrator salvo como bitmap) para criar os ativos e use o Dynamic Media Classic para reunir as partes, gerenciar as camadas e adicionar variáveis.
2. Depois de criar ativos gráficos, carregue-os no Dynamic Media Classic. Em vez de carregar ativos individuais do PSD, recomendamos que você carregue todo o arquivo de PSD em camadas e que o Dynamic Media Classic crie um arquivo por camada usando a opção **Manter camadas** no carregamento (veja mais detalhes abaixo). _Fluxo de Trabalho do Texto: Se estiver criando texto dinâmico, carregue suas fontes também. O texto dinâmico é variável e controlado pelo URL. Se o texto for estático ou tiver apenas algumas frases curtas que não serão alteradas — por exemplo, tags que dizem &quot;Novo&quot; ou &quot;Venda&quot;, em vez de &quot;X% Desativado&quot;, com o X sendo um número variável — recomendamos renderizar previamente o texto no Photoshop e fazer upload como camadas rasterizadas como imagens. É mais fácil e você pode estilizar o texto exatamente como desejar._
3. Crie o modelo no Dynamic Media Classic usando o editor de Noções básicas de modelo do menu Criar e adicione camadas de imagem. Fluxo de trabalho de texto: crie camadas de texto no mesmo editor. Essa etapa é necessária ao criar um modelo manualmente no Dynamic Media Classic. Escolha um tamanho de tela de desenho que corresponda ao seu design, arraste e solte imagens na tela de desenho e defina as propriedades da camada (tamanho, rotação, opacidade etc.). Você não está colocando todas as camadas possíveis no modelo, apenas um espaço reservado por camada de imagem. _Fluxo de trabalho do texto: crie camadas de texto com a ferramenta Texto, de modo semelhante à criação de camadas de texto no Photoshop. É possível escolher uma fonte e estilizá-la usando as mesmas opções disponíveis com a ferramenta Tipo de Photoshop._ Outro fluxo de trabalho é carregar um PSD e fazer com que o Dynamic Media Classic gere um modelo &quot;gratuito&quot;, podendo até mesmo recriar camadas de texto. Isso será discutido posteriormente com mais detalhes.
4. Depois que as camadas são criadas, adicione parâmetros (variáveis) a qualquer propriedade de qualquer camada que você gostaria de controlar por meio do URL, incluindo a origem da camada (a própria imagem ). _Fluxo de Trabalho de Texto: Você também pode adicionar parâmetros às camadas de texto, tanto para controlar o conteúdo do texto quanto o tamanho e a posição da própria camada, bem como todas as opções de formatação, como cor da fonte, tamanho da fonte, rastreamento horizontal etc._
5. Crie uma predefinição de imagem que corresponda ao tamanho do seu modelo. Recomendamos fazer isso para que o modelo seja sempre chamado no tamanho 1:1 e também para adicionar nitidez a qualquer camada de imagem grande que seja redimensionada para se ajustar ao modelo. Se você estiver criando um modelo para aplicar zoom, essa etapa não será necessária.
6. Publish, copie o URL da visualização do Dynamic Media Classic e teste-o em um navegador.

## Preparação e upload do seu modelo do Assets para o Dynamic Media Classic

Antes de fazer upload dos ativos do modelo para o Dynamic Media Classic, será necessário concluir algumas etapas preparatórias.

### Preparando o PSD para fazer upload

Antes de fazer upload do arquivo Photoshop para o Dynamic Media Classic, simplifique as camadas no Photoshop para facilitar o trabalho e ter maior compatibilidade com o Servidor de imagens. Seu arquivo PSD geralmente consistirá em muitos elementos que a Dynamic Media Classic não reconhece e você também pode acabar com muitas pequenas partes que são difíceis de gerenciar. Certifique-se de salvar um backup do PSD principal caso precise editar o original posteriormente. Você fará upload da cópia simplificada, e não da principal.

![imagem](assets/basic-templates/basic-templates-2.jpg)

1. Simplifique a estrutura da camada mesclando/nivelando camadas relacionadas que precisam ser ativadas/desativadas juntas em uma única camada. Por exemplo, o rótulo &quot;NOVO&quot; e o banner azul são mesclados em uma única camada para que você possa exibi-los ou ocultá-los com um único clique.
   ![imagem](assets/basic-templates/basic-templates-3.jpg)
2. Alguns tipos e efeitos de camada não são suportados pelo Dynamic Media Classic ou pelo Servidor de imagens e precisam ser rasterizados antes do upload. Caso contrário, os efeitos podem ser ignorados ou as camadas podem ser descartadas. Rasterizar uma camada significa convertê-la de editável para não editável. Para rasterizar efeitos de camada ou camadas de texto, crie uma camada vazia, selecione e mescle usando **Camadas > Mesclar camadas** ou CTRL + E/CMD + E.

   - O Dynamic Media Classic não pode agrupar ou vincular camadas. Todas as camadas em um grupo ou conjunto vinculado são convertidas em camadas separadas que não são mais agrupadas/vinculadas.
   - As máscaras de camada são convertidas em transparência no upload.
   - As camadas de ajuste não são compatíveis e são descartadas.
   - As camadas de preenchimento, como as camadas de Cor Sólida, são rasterizadas.
   - As camadas de objetos inteligentes e as camadas de vetor são rasterizadas em imagens normais no upload e os Filtros inteligentes são aplicados e rasterizados.
   - As camadas de texto também serão rasterizadas, a menos que você use a opção Extrair texto. Veja mais informações abaixo.
   - A maioria dos efeitos de camada é ignorada e somente alguns modos de mesclagem são suportados. Em caso de dúvida, adicione efeitos simples no Dynamic Media Classic (como sombras internas ou projetadas, brilhos internos ou externos) ou use uma camada em branco para mesclar e rasterizar o efeito no Photoshop.

### Trabalhar com fontes

Você também fará upload e publicará suas fontes se precisar gerar texto dinâmico. A única fonte incluída com o Dynamic Media Classic é Arial.

Cada empresa é responsável por obter uma licença para usar uma fonte na Web — simplesmente ter uma fonte instalada em seu computador não lhe dá o direito de usá-la comercialmente na Web, e sua empresa pode enfrentar uma ação judicial do editor de fontes, se usada sem permissão. Além disso, os termos da licença variam — você pode precisar de licenças separadas para impressão versus exibição em tela, por exemplo.

O Dynamic Media Classic é compatível com fontes padrão OpenType (OTF), TrueType (TTF) e Type 1 Postscript. Mac - apenas fontes de maletas, arquivos de coleção de tipos, fontes do sistema Windows e fontes de máquinas proprietárias (como fontes usadas por máquinas de gravação ou bordado) não são suportados — será necessário convertê-los em um dos formatos de fonte padrão ou substituir uma fonte semelhante para usar no Dynamic Media Classic e no Servidor de imagens.

Depois que as fontes forem carregadas no Dynamic Media Classic, como qualquer outro ativo, elas também deverão ser publicadas no Servidor de imagens. Um erro de modelo muito comum é esquecer de publicar suas fontes, o que resultará em um erro de imagem — o Servidor de imagens não substituirá outra fonte em seu lugar. Além disso, se você quiser usar a opção **Extrair Texto** durante o upload, será necessário carregar os arquivos de fontes antes de carregar o PSD que usa essas fontes. O recurso **Extrair Texto** tentará recriar seu texto como uma camada de texto editável e colocá-lo dentro de um modelo Dynamic Media Classic. Isso é discutido no próximo tópico, Opções de PSD.

Observe que as fontes têm vários nomes internos que geralmente são diferentes de seus nomes de arquivo externos. Você pode ver todos os seus diferentes nomes na página Detalhes desse ativo no Dynamic Media Classic. Estes são os nomes da fonte Adobe Caslon Pro Semibold, listados na guia Metadados no Dynamic Media Classic:

![imagem](assets/basic-templates/basic-templates-4.jpg)

_Guia Metadados na página Detalhes para uma fonte no Dynamic Media Classic._

O Dynamic Media Classic usa o nome de arquivo dessa fonte (ACaslonPro-Semibold) como sua ID de ativo, no entanto, esse não é o nome usado pelo modelo. O modelo usa o Nome em formato Rich Text (RTF), listado na parte inferior. RTF é o &quot;idioma&quot; nativo do mecanismo de texto do Servidor de imagens.

Se precisar alterar fontes por meio da URL, chame o nome RTF da fonte (não a ID do ativo), ou ocorrerá um erro. Nesse caso, o nome correto dessa fonte seria &quot;Adobe Caslon Pro&quot;. Abordaremos mais sobre fontes e RTF no tópico RTF e Parâmetros de texto, abaixo.

Os formatos de arquivo de fontes mais comuns encontrados em sistemas Windows e Mac são OpenType e TrueType. O OpenType tem uma extensão .OTF, enquanto TrueType é .TTF. Ambos os formatos funcionam igualmente bem no Dynamic Media Classic.

### Selecionar Opções Ao Fazer Upload Do PSD

Não é necessário carregar um arquivo Photoshop (PSD) para criar um modelo; um modelo pode ser criado a partir de qualquer ativo de imagem no Dynamic Media Classic. No entanto, fazer o upload de um PSD pode facilitar a criação, pois esses ativos normalmente já estarão em um PSD em camadas. Além disso, o Dynamic Media Classic gerará automaticamente um modelo ao fazer upload de um PSD em camadas.

- **Manter camadas.** Esta é a opção mais importante. Isso instrui o Dynamic Media Classic a criar um ativo de imagem por camada do Photoshop. Se desmarcada, todas as outras opções são desativadas e o PSD é nivelado em uma única imagem.
- **Criar** **Modelo.** Essa opção pega as várias camadas geradas e cria automaticamente um modelo ao combiná-las novamente. Uma desvantagem de usar o modelo gerado automaticamente é que o Dynamic Media Classic coloca todas as camadas em um arquivo, enquanto só precisamos de um único espaço reservado por camada. É fácil o suficiente excluir as camadas extras, mas se você tiver muitas camadas, é mais rápido recriá-las. Renomeie o novo modelo; caso contrário, ele será substituído na próxima vez que você fizer upload novamente do mesmo PSD.
- **Extrair Texto.** Isso recria camadas de texto no PSD como camadas de texto no modelo usando a fonte que você carregou. Essa etapa será necessária se o texto estiver em um caminho no Photoshop e você quiser manter esse caminho no modelo. Este recurso requer que você use a opção **Criar Modelo**, pois o texto extraído só pode ser criado por um modelo gerado no carregamento.
- **Estender camadas para o tamanho do plano de fundo.** Essa configuração faz com que cada camada tenha o mesmo tamanho que a tela de PSD geral. Isso é muito útil para camadas que sempre permanecerão fixas na posição: caso contrário, ao trocar imagens na mesma camada, talvez seja necessário reposicioná-las.
- **Nomeação da Camada.** Isso informa ao Dynamic Media Classic como nomear cada ativo gerado por camada. Recomendamos o **Photoshop** **e a Camada** **Nome** ou o Photoshop e a **Camada** **Número**. Ambas as opções usam o nome do PSD como a primeira parte do nome e adicionam o nome ou o número da camada no final. Por exemplo, se você tiver um PSD chamado &quot;shirt.psd&quot; e ele tiver camadas chamadas &quot;front&quot;, &quot;sleeves&quot; e &quot;collar&quot;, se carregar usando a opção **Photoshop e** Camada **Name**, o Dynamic Media Classic gerará as IDs de ativo &quot;shirt_front&quot;, &quot;shirt_sleeves&quot; e &quot;shirt_collar&quot;. Usar uma dessas opções ajuda a garantir que o nome seja exclusivo no Dynamic Media Classic.

## Criar um modelo com camadas de imagem

Embora o Dynamic Media Classic possa criar automaticamente um modelo a partir de um PSD em camadas, você deve saber como criar o modelo manualmente. Como explicado acima, há momentos em que você não deseja usar o template criado pelo Dynamic Media Classic.

### A interface de Noções básicas do modelo

Primeiro, vamos nos familiarizar com a interface de edição.

No centro esquerdo está sua área de trabalho mostrando uma pré-visualização do modelo final. No lado direito estão os painéis Camadas e Propriedades da camada. Essas são as áreas em que você está trabalhando mais.

![imagem](assets/basic-templates/basic-templates-5.jpg)

_Página de Noções Básicas do Modelo de Compilação._

- **Área de Trabalho/Visualização.** Esta é a janela principal. Aqui você pode mover, redimensionar e girar camadas com o mouse. Contornos de camada são mostrados como linhas tracejadas.
- **Camadas.** Isso é semelhante ao painel de camadas do Photoshop. À medida que você adiciona camadas ao modelo, elas são exibidas aqui. As camadas são empilhadas de cima para baixo — a camada superior no painel Camadas é vista acima das outras abaixo dela na lista.
- **Propriedades da camada.** Aqui você pode ajustar todas as propriedades de uma camada usando controles numéricos. Primeiro, selecione uma camada e ajuste suas propriedades.
- **Composto** **URL.** Na parte inferior da interface do usuário está a área de URL composta. Isso não será discutido nesta seção do tutorial, no entanto, aqui você verá seu modelo desconstruído como uma série de modificadores de URL do Servidor de imagens. Esta área pode ser editada — se você conhece bem os comandos do Servidor de imagens, pode editar o modelo manualmente aqui. No entanto, você também pode quebrá-lo. Assim como o Photoshop, a numeração de camadas começa em 0. A Tela de Pintura é a camada 0 e a primeira camada adicionada por você mesmo é a camada 1. Os modos de mesclagem determinam como os pixels de uma camada se mesclam com os pixels abaixo dela. Você pode criar diversos efeitos especiais usando os modos de mesclagem.

#### Uso do Editor de noções básicas de modelo

Estas são as etapas do fluxo de trabalho para iniciar seu modelo básico:

1. No Dynamic Media Classic, vá para **Build > Noções básicas do modelo**. Você pode não ter nada selecionado ou começar selecionando uma imagem, que se torna a primeira camada do modelo.
2. Escolha um Tamanho e pressione **OK**. Esse tamanho deve corresponder ao tamanho que você projetou no Photoshop. O editor de modelo será carregado.
3. Se você não tiver uma imagem selecionada na etapa 1, procure ou procure uma imagem no painel de ativos à esquerda e arraste-a para a área de trabalho.

   - A imagem será automaticamente redimensionada para ajustar-se ao tamanho da tela de desenho. Se você planeja trocar suas imagens de alta resolução, normalmente traria uma de suas imagens de TIFF P grandes (2000 px) e a usaria como um espaço reservado.
   - Essa deve ser a camada mais inferior do modelo, no entanto, é possível reordená-las posteriormente.

4. Redimensione ou reposicione a camada diretamente na área de trabalho ou ajustando as configurações no painel Propriedades da camada.
5. Arraste camadas de imagem adicionais, conforme necessário. Adicione efeitos de camadas se desejar também. Consulte o tópico _Adicionando efeitos de camada_, abaixo.
6. Clique em **Salvar**, escolha um local e nomeie o modelo. Você pode visualizar, no entanto, nesse ponto, o modelo será exatamente igual a uma imagem nivelada do Photoshop, não sendo possível alterá-lo ainda.

### Adicionar efeitos de camada

O Servidor de imagens aceita alguns efeitos de camada programáticos — efeitos especiais que alteram a aparência do conteúdo de uma camada. Elas funcionam de forma semelhante aos efeitos de camada no Photoshop. Eles são anexados a uma camada, mas controlados independentemente da camada. É possível ajustá-las ou removê-las sem fazer uma alteração permanente na própria camada.

- **Sombra**. Aplica uma sombra fora dos limites da camada, posicionada por um deslocamento de pixel x e y.
- **Sombra Interna**. Aplica uma sombra dentro dos limites da camada, posicionada por um deslocamento de pixels x e y.
- **Brilho Externo**. Aplica um efeito de brilho uniformemente em todas as bordas da camada.
- **Brilho Interno**. Aplica um efeito de brilho uniformemente dentro de todas as bordas da camada.

![imagem](assets/basic-templates/basic-templates-6.jpg)

_Uma camada com e sem sombra projetada_

Para adicionar um efeito, clique em **Adicionar Efeito** e escolha um efeito no menu. Como as camadas normais, você pode selecionar um efeito no painel Camadas e usar o painel Propriedades da camada para ajustar suas configurações.

Os efeitos de sombra são deslocados horizontal ou verticalmente em relação à camada, enquanto os efeitos de brilho são aplicados uniformemente em todas as direções. Os efeitos internos atuam sobre as partes opacas da camada, enquanto os efeitos externos afetam apenas as áreas transparentes.

Saiba mais sobre[Adicionando efeitos de camada](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html?lang=pt-BR#using-shadow-and-glow-effects-on-layers).

### Adição de parâmetros

Se apenas você combinar camadas e salvá-las, o resultado final não será diferente de uma imagem nivelada do Photoshop. O que torna os modelos especiais é a capacidade de adicionar parâmetros às propriedades de cada camada para que eles possam ser alterados dinamicamente por meio do URL.

Em termos de Dynamic Media Classic, um parâmetro é uma variável que pode ser vinculada a uma propriedade de modelo para que possa ser manipulada por meio de um URL. Ao adicionar um parâmetro a uma camada, o Dynamic Media Classic expõe essa propriedade no URL, prefixando o nome do parâmetro com um cifrão ($) — por exemplo, se você criar um parâmetro chamado &quot;size&quot; para alterar o tamanho de uma camada, o Dynamic Media Classic renomeará o parâmetro $size.

Se você não adicionar um parâmetro para uma propriedade, ela permanecerá oculta no banco de dados do Dynamic Media Classic e não aparecerá no URL.

![imagem](assets/basic-templates/parameters.png)

Sem parâmetros, seus URLs normalmente seriam muito mais longos, especialmente se você também estivesse usando texto dinâmico. O texto adiciona muitas dezenas de caracteres extras a cada URL.

Finalmente, seu conjunto inicial de parâmetros se torna os valores padrão das propriedades no modelo. Se você criar seu modelo, adicionar parâmetros e, em seguida, chamar o URL sem seus parâmetros, o Servidor de imagens criará a imagem com todos os padrões salvos no modelo. Os parâmetros só são necessários se você quiser alterar uma propriedade. Se uma propriedade não precisar ser alterada, não será necessário definir um parâmetro.

#### Criando Parâmetros

Este é o fluxo de trabalho para criar parâmetros:

1. Clique no botão **Parâmetros** ao lado do nome da camada para a qual deseja criar parâmetros. A tela Parâmetros é aberta. Ela lista cada propriedade na camada e seu valor.
1. Selecione a opção **Em** ao lado do nome de cada propriedade que você deseja transformar em um parâmetro. Um nome de parâmetro padrão será exibido. Você só pode adicionar parâmetros a propriedades que tenham sido alteradas em relação ao estado padrão.

   - Por exemplo, se você adicionar uma camada e mantê-la na posição xy padrão de 0,0, o Dynamic Media Classic não exporá uma propriedade **Position**. Para corrigir, mova a camada pelo menos um pixel. Agora o Dynamic Media Classic vai expor **Position** como uma propriedade que você pode parametrizar.
   - Para adicionar um parâmetro à propriedade show/hide (que ativa e desativa a camada ), clique no ícone **Mostrar** ou **Ocultar Camada** para desativar a camada (você pode ativá-la novamente depois, se desejar). O Dynamic Media Classic agora exibirá uma propriedade **Hide** que pode ser parametrizada.

1. Renomeie os nomes de parâmetro padrão para algo que seja mais fácil de identificar no URL. Por exemplo, se você quiser adicionar um parâmetro para alterar a camada de banner na parte superior de uma imagem, altere o nome padrão de &quot;layer_2_src&quot; para &quot;banner&quot;.
1. Pressione **Fechar** para sair da tela Parâmetros.
1. Repita esse processo para outras camadas clicando no botão **Parâmetros** e adicionando e renomeando parâmetros.
1. Salve as alterações ao concluir.

>[!TIP]
>
>Renomeie seus parâmetros para algo significativo e desenvolva uma convenção de nomenclatura para padronizar esses nomes. Certifique-se de que a convenção de nomenclatura seja acordada antecipadamente pelas equipes de design e desenvolvimento.
>
>Não é possível adicionar um parâmetro porque você não vê a propriedade? Basta alterar a propriedade da camada em relação ao padrão (movendo, redimensionando, ocultando etc.). Agora você deve ver essa propriedade exposta.

Saiba mais sobre [Parâmetros do modelo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html?lang=pt-BR).

## Criação de um modelo com camadas de texto

Agora você aprenderá a criar um Modelo básico que inclua camadas de texto.

### Noções básicas sobre texto dinâmico

Agora você sabe como criar um Modelo básico usando camadas de imagem. Para muitos aplicativos, isso é tudo de que você precisa. Como você viu no exercício anterior, as camadas com texto simples (como &quot;Venda&quot; e &quot;Novo&quot;) podem ser rasterizadas e tratadas como imagens, pois o texto não precisa ser alterado.

No entanto, e se você precisasse:

- Adicione um rótulo para dizer &quot;25% de desconto&quot;, com o valor de 25% sendo variável
- Adicione um rótulo de texto com o nome do produto na parte superior da imagem
- Localize suas camadas em diferentes idiomas, dependendo do país no qual seu modelo é visto

Nesse caso, convém adicionar algumas camadas de texto dinâmico com parâmetros para controlar o texto e/ou a formatação.

Para criar texto, você precisa fazer upload de algumas fontes; caso contrário, o Dynamic Media Classic assumirá Arial como padrão. As fontes também devem ser publicadas no Servidor de imagens, caso contrário, um erro será gerado no momento em que o servidor tentar renderizar qualquer texto que use essa fonte.

### Parâmetros de RTF e Texto

Para adicionar variáveis ao texto usando a ferramenta Noções básicas de modelo, você deve entender como o texto é renderizado. O Servidor de imagens gera texto usando o Mecanismo de texto Adobe, o mesmo mecanismo usado pelo Photoshop e pelo Illustrator, e o compõe como uma camada na imagem final. Para se comunicar com o mecanismo, o Servidor de imagens usa Rich Text Format, ou RTF.

RTF é uma especificação de formato de arquivo desenvolvida pela Microsoft para especificar a formatação de documentos. É uma linguagem de marcação padrão usada pela maioria dos softwares de processamento de texto e de email. Se você escreveu em um URL &amp;text=\b1 Hello, o Servidor de imagens geraria uma imagem com a palavra &quot;Hello&quot; em negrito, porque \b1 é o comando RTF para colocar o texto em negrito.

A boa notícia é que o Dynamic Media Classic gera o RTF para você. Sempre que você digita texto em um modelo e adiciona formatação, o Dynamic Media Classic grava silenciosamente o código RTF no modelo automaticamente. O motivo para mencionarmos isso é porque você está adicionando parâmetros diretamente ao RTF em si, portanto, é importante que você esteja um pouco familiarizado com ele.

#### Criar camadas de texto

Você pode criar camadas de texto em um modelo no Dynamic Media Classic das duas seguintes maneiras:

1. Ferramenta de texto no Dynamic Media Classic. Discutiremos esse método abaixo. O editor de Noções básicas de modelo tem uma ferramenta que permite criar uma caixa de texto, inserir texto e formatar o texto. O Dynamic Media Classic gera o RTF conforme necessário e o coloca em uma camada separada.
2. Extrair texto (no upload). O outro método é criar a camada de texto no Photoshop e salvá-la no PSD como uma camada de texto normal (em vez de rasterizá-la como uma camada de imagem). Em seguida, você faz o upload do arquivo para o Dynamic Media Classic e usa a opção **Extrair Texto**. O Dynamic Media Classic converterá cada camada de texto do Photoshop em uma camada de texto do Servidor de imagens usando comandos RTF. Se você usar esse método, primeiro faça upload das fontes no Dynamic Media Classic, caso contrário, o Dynamic Media Classic substituirá uma fonte padrão no upload e não há uma maneira fácil de substituir a fonte correta.

### O Editor de texto

Você insere texto usando o Editor de texto. O Editor de texto é uma interface WYSIWYG que permite inserir e formatar o texto usando controles de formatação semelhantes aos do Photoshop ou Illustrator.

![imagem](assets/basic-templates/basic-templates-9.jpg)

_Editor de Texto de Noções Básicas do Modelo._

Você realizará a maior parte de seu trabalho na guia **Visualização**, que permite inserir texto e vê-lo exibido como ele aparecerá no modelo. Há também a guia **Source**, que é usada para editar manualmente o RTF, se necessário.

O fluxo de trabalho geral é usar a guia **Visualização** para digitar texto.

Em seguida, selecione o texto e escolha alguma formatação, como cor da fonte, tamanho da fonte ou justificação, usando os controles na parte superior. Depois que o texto for estilizado como você deseja, clique em **Aplicar** para vê-lo atualizado na visualização da área de trabalho. Em seguida, você fecha o Editor de texto para retornar à janela principal Noções básicas do modelo.

#### Uso do Editor de texto

Estas são as etapas do fluxo de trabalho para adicionar texto dentro da página de build de Noções básicas do modelo:

1. Clique no botão de ferramenta **Texto**, na parte superior da página de compilação.
2. Arraste para fora uma caixa de texto onde você deseja que o texto apareça. A janela Editor de texto será aberta em uma janela modal. No plano de fundo, você verá seu modelo, no entanto, ele não será editável até que você conclua a edição do texto.
3. Digite o texto de amostra que você deseja exibir quando o modelo for carregado pela primeira vez. Por exemplo, se estiver criando uma caixa de texto para uma imagem de email personalizada, seu texto poderá dizer &quot;Olá, nome. Agora é a hora de economizar!&quot; Posteriormente, você adicionaria um parâmetro de texto para substituir Nome por um valor enviado no URL. O texto não aparecerá no modelo abaixo da janela até que você clique em **Aplicar**.
4. Para formatar o texto, selecione-o arrastando com o mouse e escolha um controle de formatação na interface.

   - Há muitas opções de formatação. Algumas das mais comuns são fonte (face), tamanho da fonte e cor da fonte, bem como justificação esquerda/centro/direita.
   - Não se esqueça de selecionar o texto primeiro. Caso contrário, você não poderá aplicar nenhuma formatação.
   - Para escolher uma fonte diferente, selecione o texto e abra o menu Fonte. O editor mostrará uma lista de todas as fontes carregadas no Dynamic Media Classic. Se uma fonte também estiver instalada no computador, ela aparecerá em preto. Se não estiver instalado no computador, ele aparecerá em vermelho. No entanto, ele ainda será renderizado na janela de visualização quando você clicar em **Aplicar**. Você só precisa carregar fontes no Dynamic Media Classic para torná-las disponíveis a qualquer pessoa que use o Dynamic Media Classic. Depois de publicar, o Servidor de imagens usará essas fontes para gerar o texto — os usuários não precisam instalar fontes para ver o texto criado, pois ele faz parte de uma imagem.
   - Diferentemente do Photoshop e do Illustrator, o Servidor de imagens pode alinhar o texto verticalmente na caixa de texto. O padrão é alinhamento superior. Para alterar isso, selecione o texto e escolha **Meio** ou **Inferior** no menu **Alinhamento Vertical**.
   - Se você tornar o texto muito grande para a caixa (ou se a caixa de texto for muito pequena), ela será recortada e desaparecerá total ou parcialmente. Reduza o tamanho da fonte ou aumente a caixa.

5. Clique em **Aplicar** para ver suas alterações em vigor na janela da área de trabalho. Você deve clicar em **Aplicar**, caso contrário, perderá suas edições.
6. Quando terminar, clique em **Fechar**. Se quiser voltar para o modo de edição, clique duas vezes na camada de texto para reabrir o Editor de texto.

O editor de texto visualiza exatamente o tamanho da fonte se você tiver a fonte instalada localmente em seu sistema.

### Sobre a adição de parâmetros a camadas de texto

Agora, seguimos um processo semelhante para adicionar parâmetros de texto, como fizemos para parâmetros de camada. As camadas de texto também podem receber parâmetros de camada para tamanho, posição e assim por diante; no entanto, elas podem receber parâmetros adicionais que permitem controlar qualquer aspecto do RTF.

Diferentemente dos parâmetros de camada, você só seleciona o valor que deseja alterar e adiciona um parâmetro a ele, em vez de adicionar um parâmetro a toda a propriedade.

![imagem](assets/basic-templates/basic-templates-10.jpg)

Exemplo de RTF:

![imagem](assets/basic-templates/sample-rtf.png)

Ao examinar o RTF, você precisa descobrir onde está cada configuração que deseja alterar. No RTF acima, algumas delas podem fazer algum sentido e você pode ver de onde a formatação vem.

Vocês podem ver a frase Chocolate Mint Sandal — esse é o próprio texto.

- Existe uma referência à fonte Poor Richard - é aqui que as fontes são selecionadas.
- Você pode ver um valor de RGB: \red56\green53\blue4 — essa é a cor do texto.
- Embora o tamanho da fonte seja 20, você não verá o número 20. Entretanto, você verá um comando \fs40 — por alguma razão estranha, RTF mede fontes como pontos médios. Assim, \fs40 é o tamanho da fonte!

Você tem informações suficientes para criar seus parâmetros, no entanto, há uma referência completa de todos os comandos RTF na documentação do Servidor de imagens. Visite a [Documentação do Servidor de Imagens](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html?lang=pt-BR#concept-0d3136db7f6f49668274541cd4b6364c).

#### Adicionar parâmetros às camadas de texto

Estas são as etapas para adicionar parâmetros às camadas de texto.

1. Clique no botão **Parâmetros** (um &quot;P&quot;) ao lado do nome da camada de texto para a qual você deseja criar parâmetros. A tela Parâmetros é aberta. A guia **Comum** lista cada propriedade na camada e seu valor. Aqui você pode adicionar parâmetros de camada regulares.
1. Clique na guia **Texto**. Aqui você pode ver o RTF na parte superior; os parâmetros adicionados estão abaixo dele.
1. Para adicionar um parâmetro, primeiro destaque o valor que deseja alterar e clique no botão **Adicionar Parâmetro**. Selecione apenas os valores para comandos e não o próprio comando inteiro. Por exemplo, para definir um parâmetro para o nome da fonte no RTF de amostra acima, eu destacaria apenas &quot;Poor Richard&quot; e adicionaria um parâmetro a isso, mas não também o &quot;\f0.&quot; Quando você clica em **Adicionar Parâmetro**, ele aparece na lista abaixo e o valor do parâmetro aparece em vermelho no RTF enquanto ainda está selecionado. Se você precisar remover um parâmetro, clique na caixa de seleção ao lado de **Ligado** para desativar esse parâmetro e ele desaparecerá.
1. Clique em para renomear o parâmetro com um nome mais significativo.
1. Quando terminar, seu RTF estará realçado em verde, onde existem parâmetros, e seus nomes e valores de parâmetro serão listados abaixo.
1. Clique em **Fechar** para sair da tela Parâmetros. Em seguida, pressione **Salvar** , para salvar o modelo. Se você terminar a edição, pressione **Fechar** para sair da página de Noções básicas do modelo.
1. Clique em **Visualizar** para testar seu modelo no Dynamic Media Classic. Para testar os parâmetros de texto, digite o novo texto ou os novos valores na janela de pré-visualização. Para alterar a fonte, você deve digitar o nome RTF exato da fonte.

>[!TIP]
>
>Para adicionar parâmetros à cor do texto, adicione separadamente parâmetros para vermelho, verde e azul. Por exemplo, se o RTF for `\red56\green53\blue46`, você adicionaria parâmetros vermelhos, verdes e azuis separados para os valores 56, 53 e 46. Na URL, você alteraria a cor chamando todos os três: `&$red=56&$green=53&$blue=46`.

Saiba como [Criar Parâmetros de Texto Dinâmicos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html?lang=pt-BR#creating-dynamic-text-parameters).

## Publicação e criação de URLs de modelo

### Criar uma predefinição de imagem

Criar uma predefinição para o modelo não é uma etapa necessária. Recomendamos como prática recomendada para que o modelo seja sempre chamado no tamanho 1:1 e também para adicionar nitidez a qualquer camada de imagem grande que seja redimensionada para se ajustar ao modelo. Se uma imagem for chamada sem uma predefinição, o Servidor de imagens poderá redimensionar arbitrariamente a imagem para o tamanho padrão (cerca de 400 pixels) e não aplicará a nitidez padrão.

Não há nada de especial em uma Predefinição de imagem para um modelo. Se você já tiver uma predefinição para uma imagem estática do mesmo tamanho, poderá usá-la no lugar dela.

### Publicação

Será necessário executar uma publicação para ver suas alterações enviadas ao vivo para o Servidor de imagens. Lembre-se do que precisa ser publicado: as várias camadas de ativos de imagem, as fontes para texto dinâmico e o próprio modelo. Semelhante a outros ativos de mídia avançada do Dynamic Media Classic, como Conjuntos de imagens e Conjuntos de rotação, um Modelo básico é uma construção artificial — é um item de linha no banco de dados que faz referência às imagens e fontes usando uma série de comandos do Servidor de imagens. Assim, quando você publica o modelo, tudo o que você está fazendo é atualizar os dados no Servidor de imagens.

Saiba mais sobre [Publicando seu Modelo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/publishing-templates.html?lang=pt-BR).

### Construção do URL de modelo

Um modelo básico tem a mesma sintaxe de URL essencial que uma chamada de imagem normal, conforme explicado anteriormente. Um modelo normalmente terá mais modificadores — comandos separados por um E comercial (&amp;) — como parâmetros com valores. No entanto, a principal diferença é que você chama o modelo como a imagem principal, em vez de chamar para uma imagem estática.

![imagem](assets/basic-templates/basic-templates-11.jpg)

Ao contrário das Predefinições de imagem, que têm um cifrão ($) em cada lado do nome predefinido, os parâmetros têm um único cifrão no início. A colocação desses cifrões é importante.

**Correto:**

`$text=46-inch LCD HDTV`

**Incorreto:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Como observado anteriormente, os parâmetros são usados para alterar o template. Se você chamar o modelo sem parâmetros, ele será revertido para as configurações padrão, conforme projetado na ferramenta de criação Noções básicas do modelo. Se uma propriedade não precisar ser alterada, não será necessário definir um parâmetro.

![imagem](assets/basic-templates/sandals-without-with-parameters.png)
_Exemplos de um modelo sem definir parâmetros (acima) e com parâmetros (abaixo)._
