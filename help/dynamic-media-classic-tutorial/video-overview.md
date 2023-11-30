---
title: Visão geral do vídeo
description: O Dynamic Media Classic é fornecido com conversão automática de vídeo durante o upload, streaming de vídeo em dispositivos móveis e de desktop e conjuntos de vídeos adaptáveis otimizados para reprodução com base no dispositivo e na largura de banda. Saiba mais sobre vídeo no Dynamic Media Classic e receba uma introdução sobre conceitos e terminologia de vídeo. Aprofunde-se em saber como fazer upload e codificar vídeos, escolher predefinições de vídeo para fazer upload, adicionar ou editar uma predefinição de vídeo, visualizar vídeos em um visualizador de vídeo, implantar vídeo em sites da Web e móveis, adicionar legendas e marcadores de capítulo ao vídeo e publicar visualizadores de vídeo em usuários de desktop e móveis.
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '6118'
ht-degree: 0%

---

# Visão geral do vídeo {#video-overview}

O Dynamic Media Classic é fornecido com conversão automática de vídeo durante o upload, streaming de vídeo em dispositivos móveis e de desktop e conjuntos de vídeos adaptáveis otimizados para reprodução com base no dispositivo e na largura de banda. Uma das coisas mais importantes sobre vídeo é que o fluxo de trabalho é simples — ele é projetado para que qualquer pessoa possa usá-lo, mesmo que não esteja familiarizada com a tecnologia de vídeo.

Ao final desta seção do tutorial, você saberá como:

- Fazer upload e codificar (transcodificar) vídeos em diferentes tamanhos e formatos
- Escolha entre as predefinições de vídeo disponíveis para upload
- Adicionar ou editar uma predefinição de codificação de vídeo
- Visualizar vídeos em um visualizador de vídeo
- Implantar vídeo na Web e em sites móveis
- Adicionar legendas e marcadores de capítulo ao vídeo
- Personalizar e publicar visualizadores de vídeo para usuários de desktop e dispositivos móveis

>[!NOTE]
>
>Todos os URLs neste capítulo são apenas para fins ilustrativos; não são links ativos.

## Visão geral do Dynamic Media Classic Video

Primeiro, vamos entender melhor as possibilidades de vídeo com o Dynamic Media Classic.

### Recursos e capacidades

A plataforma de vídeo Dynamic Media Classic oferece todas as partes da solução de vídeo: o upload, a conversão e o gerenciamento de vídeos; a capacidade de adicionar legendas e marcadores de capítulo a um vídeo; e a capacidade de usar predefinições para uma reprodução fácil.

Ele facilita a publicação de vídeo adaptável de alta qualidade para transmissão em várias telas, incluindo desktop, iOS, Android™, BlackBerry® e dispositivos móveis Windows. Um Conjunto de vídeos adaptados agrupa versões do mesmo vídeo codificadas em taxas de bits e formatos diferentes, como 400 kbps, 800 kbps e 1000 kbps. O computador desktop ou dispositivo móvel detecta a largura de banda disponível.

Além disso, a qualidade do vídeo é comutada automaticamente de forma dinâmica se as condições da rede forem alteradas no desktop ou no dispositivo móvel. Além disso, se um cliente entrar no modo de tela cheia em um desktop, o Conjunto de vídeos adaptados responderá usando uma resolução melhor, melhorando assim a experiência de visualização do cliente. O uso dos Conjuntos de vídeos adaptados oferece a melhor reprodução possível para clientes que reproduzem vídeos do Dynamic Media Classic em várias telas e dispositivos.

### Gerenciamento de vídeo

Trabalhar com vídeo pode ser mais complexo do que trabalhar com imagens ainda digitais. Com o vídeo, você lida com vários formatos e padrões e com a incerteza sobre se seu público-alvo é capaz de reproduzir seus clipes. O Dynamic Media Classic facilita o trabalho com vídeo, fornecendo muitas ferramentas poderosas &quot;por baixo dos panos&quot;, mas eliminando a complexidade de trabalhar com elas.

O Dynamic Media Classic reconhece e pode trabalhar com vários formatos de origem diferentes disponíveis. No entanto, ler o vídeo é apenas uma parte do esforço. Você também deve converter o vídeo em um formato compatível com a Web. O Dynamic Media Classic cuida disso permitindo converter vídeos em vídeos H.264.

A conversão do vídeo por conta própria pode se tornar complicada usando as muitas ferramentas profissionais e de entusiastas disponíveis. O Dynamic Media Classic mantém a simplicidade oferecendo predefinições fáceis, otimizadas para diferentes configurações de qualidade. No entanto, se quiser algo mais personalizado, também poderá criar suas próprias predefinições.

Se você tiver muitos vídeos, apreciará a capacidade de gerenciar todos os seus ativos junto com suas imagens e outras mídias no Dynamic Media Classic. Você pode organizar, catalogar e pesquisar seus ativos, incluindo ativos de vídeo, com suporte robusto a metadados XMP.

### Reprodução de vídeo

Semelhante ao problema de conversão de vídeo para torná-lo amigável para a Web e acessível, é o problema de implementação e implantação de vídeo no seu site. Escolher comprar um reprodutor ou criar o seu próprio, torná-lo compatível com vários dispositivos e telas e, em seguida, manter seus reprodutores pode ser uma ocupação em tempo integral.

Novamente, a abordagem da Dynamic Media Classic é permitir que você escolha a predefinição e o visualizador que atendem às suas necessidades. Há várias opções diferentes de visualizador e uma biblioteca de várias predefinições disponíveis.

Você pode enviar vídeos facilmente para a Web e dispositivos móveis, já que o Dynamic Media Classic suporta vídeo HTML5, o que significa que você pode direcionar usuários que executam vários navegadores, bem como usuários de plataformas Android™ e iOS. A transmissão de vídeo permite uma reprodução suave de conteúdo mais longo ou de alta definição, enquanto o vídeo de HTML 5 progressivo tem predefinições otimizadas para a tela pequena.

As Predefinições do visualizador para vídeo são parcialmente configuráveis, dependendo do tipo de visualizador.

Assim como todos os visualizadores, a integração é feita por meio de um único URL do Dynamic Media Classic por visualizador ou vídeo.

>[!NOTE]
>
>Como prática recomendada, use visualizadores de vídeo Dynamic Media Classic HTML5. As predefinições usadas nos visualizadores de HTML5 Video são players de vídeo robustos. Ao combinar em um único player a capacidade de projetar os componentes de reprodução usando o HTML5 e o CSS, ter a reprodução incorporada e usar a transmissão adaptável e progressiva, dependendo da capacidade do navegador, você estende o alcance do seu conteúdo de mídia avançada para os usuários de desktop, tablet e dispositivos móveis e garante uma experiência de vídeo simplificada.

Uma última observação sobre o vídeo do Dynamic Media Classic que pode se aplicar a alguns clientes: nem todas as empresas podem ter a conversão automática, o fluxo contínuo ou as Predefinições de vídeo habilitados para suas contas. Se, por algum motivo, você não conseguir acessar os URLs para streaming de vídeo, esse pode ser o motivo. É possível fazer upload e publicar progressivamente o vídeo baixado e ter acesso a todos os visualizadores de vídeo. No entanto, para aproveitar todos os recursos de vídeo do Dynamic Media Classic, entre em contato com seu Gerente de conta ou Gerente de vendas para ativar esses recursos.

Saiba mais sobre [Vídeo no Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html).

## Vídeo 101

### Conceitos básicos de vídeo e terminologia

Antes de começarmos, vamos discutir alguns termos que você deve conhecer para trabalhar com vídeo. Esses conceitos não são específicos do Dynamic Media Classic e, se você for gerenciar vídeos para um site profissional, faria bem em obter mais educação sobre o assunto. Recomendamos alguns recursos no final desta seção.

- **Codificação/transcodificação.** A codificação é o processo de aplicação da compactação de vídeo para converter dados de vídeo brutos e descompactados em um formato que facilita o trabalho com eles. A transcodificação, embora semelhante, refere-se à conversão de um método de codificação para outro.

   - Os arquivos de vídeo principais criados com o software de edição de vídeo geralmente são muito grandes e não estão no formato adequado para entrega em destinos online. Normalmente, eles são codificados para reprodução rápida no desktop e para edição, mas não para entrega pela Web.
   - Para converter vídeo digital para o formato e as especificações adequados para reprodução em telas diferentes, os arquivos de vídeo são transcodificados para um tamanho de arquivo menor e eficiente, ideal para entrega na Web e em dispositivos móveis.

- **Compactação de vídeo.** Redução da quantidade de dados usados para representar imagens de vídeo digital, e é uma combinação de compactação de imagem espacial e compensação de movimento temporal.

   - A maioria das técnicas de compactação apresenta perdas, o que significa que eles descartam dados para atingir um tamanho menor.
   - Por exemplo, vídeo DV é compactado relativamente pouco e permite editar facilmente a filmagem de origem, no entanto, é muito grande para usar na web ou até mesmo colocar em um DVD.

- **Formatos de arquivo.** O formato é um container, semelhante a um arquivo ZIP, que determina como os arquivos são organizados no arquivo de vídeo, mas normalmente não como são codificados.

   - Formatos de arquivos comuns para vídeo de origem incluem Windows Media (WMV), QuickTime (MOV), Microsoft® AVI e MPEG, entre outros. Os formatos publicados pelo Dynamic Media Classic são MP4.
   - Um arquivo de vídeo geralmente contém várias faixas — uma faixa de vídeo (sem áudio) e uma ou mais faixas de áudio (sem vídeo) — que são inter-relacionadas e sincronizadas.
   - O formato do arquivo de vídeo determina como esses diferentes controles de dados e metadados são organizados.

- **Codec.** Um codec de vídeo descreve o algoritmo pelo qual um vídeo é codificado usando compactação. O áudio também é codificado por meio de um codec de áudio.

   - Os codecs minimizam a quantidade de informações necessárias para reproduzir vídeo. Em vez de informações sobre cada quadro individual, somente as informações sobre as diferenças entre um quadro e o próximo são armazenadas.
   - Como a maioria dos vídeos muda pouco de um quadro para o próximo, os codecs permitem altas taxas de compactação, o que resulta em tamanhos de arquivo menores.
   - Um player de vídeo decodifica o vídeo de acordo com seu codec e exibe uma série de imagens, ou quadros, na tela.
   - Codecs de vídeo comuns incluem H.264, On2 VP6 e H.263.

![imagem](assets/video-overview/bird-video.png)

- **Resolução.** Altura e largura do vídeo em pixels.

   - O tamanho do vídeo de origem é determinado pela câmera e pela saída do software de edição. Uma câmera de alta definição cria vídeos de 1920 x 1080 de alta resolução; no entanto, para reproduzir sem problemas na Web, você poderia reduzir a resolução (redimensioná-la) para uma resolução menor, como 1280 x 720, 640 x 480 ou menor.
   - A resolução tem um impacto direto no tamanho do arquivo e na largura de banda necessária para reproduzir o vídeo.

- **Exibir taxa de proporção.** Relação entre a largura e a altura de um vídeo. Quando a proporção do vídeo não corresponder à proporção do reprodutor, você poderá ver &quot;barras pretas&quot; ou espaço vazio. Duas taxas de aspecto comuns usadas para exibir vídeo são:

   - 4:3 (1.33:1). Usado para quase todo o conteúdo de transmissão de TV de definição padrão.
   - 16:9 (1.78:1). Usado para quase todos os filmes e conteúdos de TV de alta definição (HDTV) de tela ampla.

- **Taxa de bits/taxa de dados.** A quantidade de dados codificados para compor um segundo único de reprodução de vídeo (em quilobits por segundo).

   - Geralmente, quanto menor a taxa de bits, mais desejável será para a Web, pois ela pode ser baixada mais rapidamente. No entanto, também pode significar que a qualidade é baixa devido à perda de compactação.
   - Um bom codec deve equilibrar taxa de bits baixa com boa qualidade.

- **Taxa de quadros (quadros por segundo ou FPS).** O número de quadros, ou imagens estáticas, para cada segundo do vídeo. Normalmente, a TV norte-americana (NTSC) é transmitida em 29,97 FPS; a TV europeia e asiática (PAL) é transmitida em 25 FPS; e o filme (analógico e digital) são tipicamente em 24 (23,976) FPS.

   - Para tornar as coisas mais confusas, há também quadros progressivos e entrelaçados. Cada quadro progressivo contém um quadro de imagem inteiro, enquanto os quadros entrelaçados contêm linhas alternadas de pixels em um quadro de imagem. Os quadros são reproduzidos rapidamente e parecem se misturar. O filme usa um método de varredura progressiva, enquanto o vídeo digital é normalmente entrelaçado.
   - Em geral, não importa se a gravação de origem está entrelaçada ou não — o Dynamic Media Classic preservará o método de digitalização no vídeo convertido.
   - Entrega contínua/progressiva. O streaming de vídeo é o envio de mídia em um fluxo contínuo que pode ser reproduzido à medida que chega, enquanto o vídeo progressivamente baixado é baixado como qualquer outro arquivo de um servidor e armazenado em cache localmente em seu navegador.

Esperamos que esse manual o ajude a entender as várias opções envolvidas no uso do vídeo do Dynamic Media Classic.

## Fluxo de trabalho de vídeo

Ao trabalhar com vídeo no Dynamic Media Classic, você segue um fluxo de trabalho básico semelhante ao trabalho com imagens.

![imagem](assets/video-overview/video-overview-2.png)

1. Comece carregando arquivos de vídeo no Dynamic Media Classic. Para fazer isso, abra a variável **Menu Ferramentas** na parte inferior do painel extensão do Dynamic Media Classic e escolha **Faça upload para Dynamic Media Classic > Arquivos para o nome da pasta** ou **Faça upload para Dynamic Media Classic > Pastas para o nome da pasta**. &quot;Nome da pasta&quot; é qualquer pasta que você esteja navegando atualmente com a extensão. Os arquivos de vídeo podem ser grandes, portanto, recomendamos usar o FTP para carregar arquivos grandes. Como parte do upload, escolha uma ou mais Predefinições de vídeo para codificar seus vídeos. O vídeo pode ser transcodificado para vídeo MP4 no upload. Consulte o tópico Predefinições de vídeo abaixo para obter mais informações sobre o uso e a criação de predefinições de codificação. Saiba mais sobre [Carregamento e codificação de vídeos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Selecione ou selecione e modifique uma predefinição do visualizador de vídeo e visualize o vídeo. Você pode escolher uma Predefinição do visualizador pré-criada ou personalizar sua própria predefinição. Se você estiver direcionando usuários móveis, não é necessário fazer nada aqui, pois as plataformas móveis não exigem um visualizador ou uma predefinição. Saiba mais sobre [Visualização de vídeos em um visualizador de vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) e [Adicionar ou editar uma predefinição do visualizador de vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Execute uma Publicação de vídeo, obtenha o URL e integre o. A principal diferença entre essa etapa do fluxo de trabalho de vídeo e o fluxo de trabalho de imagem é que você executa uma Publicação de vídeo especial em vez da (ou talvez e) publicação padrão do Servidor de imagens. A integração do visualizador de vídeo no desktop funciona exatamente como a integração do visualizador de imagem. No entanto, para dispositivos móveis é ainda mais simples: tudo o que você precisa é o URL do próprio vídeo.

### Sobre a transcodificação

A transcodificação foi definida anteriormente como o processo de conversão de um método de codificação para outro. No caso do Dynamic Media Classic, é o processo de converter seu vídeo original do formato atual para MP4. Isso é necessário antes que seu vídeo apareça no navegador do desktop ou em um dispositivo móvel.

O Dynamic Media Classic pode manipular toda a transcodificação para você, um enorme benefício. Você mesmo pode transcodificar o vídeo e carregar os arquivos já convertidos em MP4, mas que pode ser um processo complexo que requer software sofisticado. A menos que você saiba o que está fazendo, normalmente não obterá bons resultados na primeira tentativa.

O Dynamic Media Classic não só converte os arquivos para você, como também facilita a conversão fornecendo predefinições fáceis de usar. Você realmente não precisa saber muito sobre o lado técnico desse processo — tudo o que você deve saber é aproximadamente o tamanho final que deseja obter do sistema e uma noção da largura de banda que seus usuários finais têm.

Embora as predefinições pré-criadas sejam práticas e cubram a maioria das necessidades, às vezes você deseja algo mais personalizado. Nesse caso, você pode criar sua própria predefinição de codificação. No Dynamic Media Classic, uma predefinição de codificação é chamada de Predefinição de vídeo. Isso é explicado mais adiante neste capítulo.

### Sobre a transmissão

Outro recurso importante a ser observado é o streaming de vídeo, um recurso padrão da plataforma de vídeo Dynamic Media Classic. A mídia de transmissão é constantemente recebida e apresentada a um usuário final durante a entrega. Isso é significativo e desejável por vários motivos.

A transmissão normalmente requer menos largura de banda do que o download progressivo, pois somente a parte do vídeo que é assistida é entregue. O servidor e os visualizadores de vídeo do Dynamic Media Classic usam a detecção automática de largura de banda para fornecer o melhor fluxo possível para a conexão de Internet de um usuário.

Com a transmissão, o vídeo começa a ser reproduzido antes do que com outros métodos. Ele também faz uso mais eficiente dos recursos de rede, pois somente as partes do vídeo visualizadas são enviadas ao cliente.

O outro método de delivery é o download progressivo. Comparado ao vídeo de streaming, há apenas um benefício consistente para o download progressivo — você não precisa de um servidor de streaming para entregar o vídeo. E é claro que é aí que entra o Dynamic Media Classic — a Dynamic Media Classic tem um servidor de transmissão integrado à plataforma, de modo que você não precisa da complicação ou do custo extra de manter esse hardware dedicado.

O vídeo de download progressivo pode ser distribuído a partir de qualquer servidor Web normal. Embora isso seja conveniente e potencialmente econômico, lembre-se de que os downloads progressivos têm recursos limitados de busca e navegação e que os usuários podem acessar e redefinir objetivos do seu conteúdo. Em algumas situações, como a reprodução por trás de firewalls de rede rigorosos, a entrega de transmissão pode ser bloqueada; nesses casos, a reversão para entrega progressiva pode ser desejável.

O download progressivo é uma boa opção para amadores ou sites que têm requisitos de tráfego baixos; se eles não se importam se o conteúdo é armazenado em cache no computador de um usuário; se eles só precisam fornecer vídeos de duração mais curta (menos de 10 minutos); ou se os visitantes não podem receber vídeo de transmissão por algum motivo.

Você precisa transmitir seu vídeo se precisar de recursos avançados e controle sobre a entrega de vídeos e/ou se precisar exibir vídeo a públicos maiores (por exemplo, vários visualizadores simultâneos), rastrear e relatar o uso ou as estatísticas de visualização, ou quiser oferecer aos visualizadores a melhor experiência de reprodução interativa.

Por fim, se você estiver preocupado em proteger sua mídia para problemas de propriedade intelectual ou gerenciamento de direitos, a transmissão contínua fornecerá uma entrega de vídeo mais segura, pois a mídia não é salva no cache do cliente quando transmitida.

## Predefinições do vídeo

Ao fazer upload do vídeo, você escolhe uma ou mais predefinições que contêm as configurações para converter o vídeo principal em um formato compatível com a Web por meio de codificação. As Predefinições de vídeo têm duas opções: Predefinições de vídeo adaptável e Predefinições de codificação única.

Consulte [Predefinições de vídeo disponíveis](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

As Predefinições de vídeo adaptável são ativadas por padrão, o que significa que estão disponíveis para codificação. Se quiser usar uma Predefinição de codificação única, o administrador precisará ativá-la para que ela apareça na lista de Predefinições de vídeo.

Saiba como [Ativar ou desativar predefinições de vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

Você pode escolher uma das muitas predefinições pré-criadas que acompanham o Dynamic Media Classic ou criar as suas próprias predefinições. No entanto, nenhuma predefinição é selecionada para upload por padrão. Em outras palavras, **se você não selecionar uma Predefinição de vídeo ao carregar, ele não será convertido e poderá não ser publicado**. No entanto, você mesmo pode converter o vídeo offline e carregá-lo e publicá-lo perfeitamente. As Predefinições de vídeo só são necessárias se você quiser que o Dynamic Media Classic faça a conversão para você.

Ao fazer upload, selecione uma Predefinição de vídeo escolhendo **Opções de vídeo** no painel Opções de trabalho. Em seguida, escolha se deseja codificar para Computador, Celular ou Tablet.

- O computador é para uso em desktop. Aqui você encontrará predefinições maiores (como HD) que consomem mais largura de banda.
- Mobile e Tablet criam vídeo MP4 para dispositivos como iPhones e smartphones Android™. A única diferença entre o Mobile e o Tablet é que as predefinições do Tablet normalmente têm uma largura de banda mais alta, pois são baseadas no uso do WiFi. As predefinições móveis são otimizadas para um uso mais lento de 3G.

### Perguntas que você deve fazer antes de escolher uma predefinição

Ao escolher uma predefinição, você deve conhecer seu público-alvo e sua filmagem de origem. O que você sabe sobre seu cliente? Como eles estão assistindo ao vídeo — com um monitor de computador ou um dispositivo móvel?

Qual é a resolução do seu vídeo? Se você escolher uma predefinição maior que a original, poderá obter um vídeo desfocado/pixelado. Não há problema se o vídeo for maior que a predefinição, mas não escolha uma predefinição maior que o vídeo de origem.

Qual é a proporção? Se você vir barras pretas ao redor do vídeo convertido, é porque escolheu a taxa de proporção errada. O Dynamic Media Classic não pode detectar automaticamente essas configurações porque primeiro seria necessário examinar o arquivo antes de fazer upload.

### Detalhamento das opções de vídeo

As Predefinições de vídeo determinam como o vídeo é codificado ao especificar essas configurações. Se você não estiver familiarizado com esses termos, consulte o tópico Conceitos básicos de vídeo e terminologia, acima.

![imagem](assets/video-overview/video-overview-3.jpg)

- **Taxa de proporção.** Padrão 4:3 ou widescreen 16:9.
- **Tamanho.** É igual à resolução do vídeo e é medida em pixels. Isso está relacionado à taxa de proporção. Em uma proporção de 16:9, um vídeo é de 432 x 240 pixels, enquanto em 4:3 é de 320 x 240 pixels.
- **FPS.** As taxas de quadros padrão são 30 quadros por segundo, 25 quadros por segundo ou 24 quadros por segundo (fps), dependendo do padrão de vídeo — NTSC, PAL ou Film. Essa configuração não importa, pois o Dynamic Media Classic sempre usa a mesma taxa de quadros da origem de vídeo.
- **Formato.** Este é o MP4.
- **Largura de banda.** Esta é a velocidade de conexão desejada de seu usuário alvo. Eles têm uma conexão de Internet rápida ou lenta? Eles normalmente estão usando computadores desktop ou dispositivos móveis? Isso também está relacionado à resolução (tamanho), pois quanto maior o vídeo, mais largura de banda ele exige.

### Determinar a taxa de dados ou a &quot;taxa de bits&quot; do vídeo

Calcular a taxa de bits do seu vídeo é um dos fatores menos conhecidos para veicular vídeos na Web, mas possivelmente o mais importante, pois afeta diretamente a experiência do usuário. Se você definir sua taxa de bits como alta demais, terá alta qualidade de vídeo, mas baixo desempenho. Os usuários com conexões de Internet mais lentas são forçados a aguardar enquanto o vídeo é constantemente pausado à medida que é reproduzido. No entanto, se você defini-lo como muito baixo, a qualidade será afetada. Dentro da predefinição de vídeo, o Dynamic Media Classic sugere uma variedade de dados dependendo da largura de banda de destino. Este é um bom ponto de partida.

Entretanto, se você quiser descobrir você mesmo, precisará de uma calculadora de taxa de bits. Essa é uma ferramenta comumente usada por profissionais de vídeo e entusiastas para estimar a quantidade de dados que cabe em um determinado fluxo ou mídia (como um DVD).

## Criação de uma predefinição de vídeo personalizada

Às vezes, você pode achar que precisa de uma Predefinição de vídeo especial que não corresponde às configurações das predefinições de codificação de vídeo incorporadas. Isso pode acontecer se você tiver um vídeo personalizado de um tamanho específico, como um vídeo criado a partir de um software de animação 3D ou um vídeo que tenha sido cortado de seu tamanho original. Talvez você queira experimentar configurações diferentes de largura de banda para exibir vídeos de qualidade mais alta ou mais baixa. Qualquer que seja o caso, crie uma predefinição de vídeo de codificação única personalizada.

### Fluxo de trabalho de predefinição do vídeo

1. As Predefinições de vídeo estão localizadas em **Configuração > Configuração do aplicativo > Predefinições de vídeo**. Aqui você encontrará uma lista de todas as predefinições de codificação disponíveis para sua empresa.

   - Cada conta de transmissão de vídeo tem dezenas de predefinições prontas para uso e, se você criar suas próprias predefinições personalizadas, também as verá aqui.
   - Você pode filtrar por tipo usando o menu suspenso. As predefinições são divididas em Computador, Dispositivo móvel e Tablet.
     ![imagem](assets/video-overview/video-overview-4.jpg)

2. A coluna Ativo permite escolher se deseja exibir todas as predefinições durante o upload ou apenas aquelas escolhidas. Se você estiver nos EUA, convém desmarcar as predefinições PAL europeias e, se estiver no Reino Unido/EMEA, desmarque as predefinições NTSC.
3. Clique em **Adicionar** botão para criar uma predefinição personalizada. Isso abre o painel Adicionar predefinição de vídeo. O processo aqui é semelhante à criação de uma Predefinição de imagem.
4. Primeiro, dê a ele um **Nome da predefinição** para aparecer na lista de predefinições. No exemplo acima, esta predefinição é para vídeos tutoriais de captura de tela.
5. A variável **Descrição** O é opcional, mas fornece aos usuários uma dica de ferramenta que descreve a finalidade dessa predefinição.
6. A variável **Codificar sufixo do arquivo** é anexado ao final do nome do vídeo que você está criando aqui. Lembre-se de que você terá um Vídeo principal e este vídeo codificado, que é um derivado do principal, e que dois ativos no Dynamic Media Classic não podem ter a mesma ID de ativo.
7. **Dispositivo de reprodução** é onde você escolhe qual formato de arquivo de vídeo deseja (Computador, Celular ou Tablet). Lembre-se de que o Mobile e o Tablet produzem o mesmo formato MP4. O Dynamic Media Classic só precisa saber em qual categoria colocar a predefinição; no entanto, a diferença teórica é que as predefinições de tablet são normalmente para uma conexão mais rápida com a Internet, pois todas oferecem suporte a WiFi.
8. **Taxa de dados de público alvo** é algo que você terá que descobrir por si mesmo, no entanto, você pode ver um intervalo sugerido na imagem abaixo. Como alternativa, você pode arrastar o controle deslizante até a largura de banda de destino aproximada. Para obter uma figura mais precisa, use uma calculadora de taxa de bits. Há um pouco de tentativa e erro envolvido.

   ![imagem](assets/video-overview/video-overview-5.jpg)

9. Defina o do arquivo de origem **Taxa de proporção**. Essa configuração é diretamente vinculada ao tamanho, abaixo de. Se você escolher _Personalizado_, você terá que inserir manualmente a largura e a altura.
10. Se você escolher uma taxa de proporção, defina um valor para **Tamanho da resolução** , e o Dynamic Media Classic preencherão o outro valor automaticamente. No entanto, para uma taxa de proporção personalizada, preencha ambos os valores. Seu tamanho deve estar de acordo com sua taxa de dados. Se você definir uma taxa de dados baixa e um tamanho grande, esperará que a qualidade seja ruim.
11. Clique em **Salvar** para salvar sua predefinição. Diferentemente de todas as outras predefinições, não é necessário publicar neste ponto, pois as predefinições são somente para fazer upload de arquivos. Posteriormente, será necessário publicar os vídeos codificados, mas as predefinições são somente para uso interno do Dynamic Media Classic.
12. Para verificar se sua predefinição de vídeo está na lista de upload, acesse **Carregar**. Escolher **Opções de trabalho** e expandir **Opções de vídeo**. Sua predefinição está listada na categoria do dispositivo de reprodução escolhido (Computador, Celular ou Tablet).

Saiba mais sobre [Adicionar ou editar uma predefinição de vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Adicionar legendas ao vídeo

Às vezes, pode ser útil adicionar legendas ao vídeo, por exemplo, quando é necessário fornecer o vídeo aos visualizadores em vários idiomas, mas você não deseja dublar o áudio em outro idioma ou gravar o vídeo novamente em idiomas separados. Além disso, a adição de legendas oferece maior acessibilidade para deficientes auditivos e usam as legendas ocultas. O Dynamic Media Classic facilita a adição de legendas aos vídeos.

Saiba como [Adicionar legendas ao vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html).

## Adicionar marcadores de capítulo ao vídeo

Para vídeos longos, seus visualizadores provavelmente apreciam a capacidade e a conveniência oferecidas pela navegação do vídeo com marcadores de capítulo. O Dynamic Media Classic permite adicionar facilmente marcadores de capítulo ao vídeo.

Saiba como [Adicionar marcadores de capítulo ao vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## Tópicos de implementação de vídeo

### Publicar e copiar URL

A última etapa do fluxo de trabalho do Dynamic Media Classic é publicar o conteúdo de vídeo. No entanto, o vídeo tem seu próprio trabalho de publicação, chamado Publicação do servidor de vídeo, encontrado em Avançado.

![imagem](assets/video-overview/video-overview-6.jpg)

Saiba como [Publicar seu vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Depois de executar uma publicação de vídeo, você pode obter um URL para acessar seus vídeos e quaisquer Predefinições do visualizador do Dynamic Media Classic prontas para uso em um navegador da Web. No entanto, se você personalizar ou criar sua própria Predefinição do visualizador de vídeo, será necessário executar uma publicação separada do Servidor de imagens.

- Saiba como [Vincular um URL a um site para dispositivos móveis ou site](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- Saiba como [Incorporar o visualizador de vídeo em uma página da Web](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

Você também pode implantar o vídeo usando um player de vídeo de terceiros ou personalizado.

Saiba como [Implantar vídeo usando um reprodutor de vídeo de terceiros](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

Além disso, se você também quiser usar as miniaturas do vídeo — a imagem extraída do vídeo — é necessário executar uma publicação do Servidor de imagens. Isso ocorre porque a imagem em miniatura do vídeo fica no Servidor de imagens, enquanto o vídeo em si está no Servidor de vídeo. As miniaturas de vídeo podem ser usadas em resultados de pesquisa de vídeo, listas de reprodução de vídeo e podem ser usadas como o &quot;quadro de pôster&quot; inicial exibido no visualizador de vídeo antes que o vídeo seja reproduzido.

Saiba mais sobre [Trabalhar com miniaturas de vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Seleção e personalização de uma predefinição do visualizador

O processo para selecionar e personalizar uma Predefinição do visualizador é igual ao processo para imagens. Crie uma predefinição ou modifique uma predefinição existente e salve com um novo nome, faça edições e execute uma publicação do Servidor de imagens. Todas as predefinições do visualizador são publicadas no Servidor de imagens, não apenas as predefinições de imagens e, portanto, é necessário executar uma publicação de imagem para ver suas predefinições novas ou modificadas.

>[!TIP]
>
>Execute um Servidor de imagens para publicar após a publicação do Servidor de vídeo para publicar imagens em miniatura associadas aos seus vídeos.

## Otimização do mecanismo de pesquisa de vídeo

A Otimização do mecanismo de pesquisa (SEO) é o processo de melhorar a visibilidade de um site ou de uma página da Web nos mecanismos de pesquisa. Embora os mecanismos de pesquisa se sobressaiam na coleta de informações sobre o conteúdo baseado em texto, eles não podem adquirir informações adequadamente sobre o vídeo, a menos que essas informações sejam fornecidas a eles. Usando o Dynamic Media Classic Video SEO, você pode usar metadados para fornecer aos mecanismos de pesquisa descrições de seus vídeos. O recurso Video SEO permite criar mapas de site de vídeo e feeds RSS de mídia (mRSS).

- **Mapa do site de vídeo**. Informa ao Google exatamente onde e qual é o conteúdo de vídeo em um site. Portanto, os vídeos são totalmente pesquisáveis no Google. Por exemplo, um mapa do site de vídeo pode especificar o tempo de execução e as categorias de vídeos.
- **RSS feed**. Usado pelos editores de conteúdo para alimentar arquivos de mídia no Yahoo! Pesquisa de vídeo. O Google é compatível com o protocolo de feed de Mídia RSS (mRSS) e Mapa do site de vídeo para envio de informações para mecanismos de pesquisa.

Ao criar mapas de site de vídeo e feeds mRSS, você decide quais campos de metadados de arquivos de vídeo incluir. Dessa forma, você descreve os vídeos nos mecanismos de pesquisa para que os mecanismos de pesquisa possam direcionar com mais precisão o tráfego para vídeos no seu site.

Depois que o Mapa do site ou feed for criado, o Dynamic Media Classic poderá publicá-lo automaticamente, publicá-lo manualmente ou simplesmente gerar um arquivo que poderá editar posteriormente. Além disso, o Dynamic Media Classic pode gerar e publicar automaticamente esse arquivo todos os dias.

No final do processo, você envia o arquivo ou o URL para o mecanismo de pesquisa. Essa tarefa é realizada fora do Dynamic Media Classic; no entanto, ela será discutida brevemente abaixo.

### Requisitos para arquivos Sitemap/mRSS

Para que o Google e outros mecanismos de pesquisa não rejeitem seus arquivos, eles devem estar no formato adequado e incluir determinadas informações. O Dynamic Media Classic gera um arquivo formatado corretamente; no entanto, se as informações não estiverem disponíveis para alguns dos vídeos, eles não serão incluídos no arquivo.

Os campos obrigatórios são Página inicial (o URL da página que está veiculando o vídeo, não o URL do vídeo em si), Título e Descrição. Cada vídeo deve ter uma entrada para esses itens, caso contrário, ele não será incluído no arquivo gerado. Os campos opcionais são Tags e Categoria.

Há dois outros campos obrigatórios: URL do conteúdo, o URL para o próprio ativo de vídeo e Miniatura, um URL para uma imagem em miniatura do vídeo. No entanto, o Dynamic Media Classic preencherá automaticamente esses valores para você.

O fluxo de trabalho recomendado é incorporar esses dados em seus vídeos antes de carregá-los usando metadados XMP, e o Dynamic Media Classic os extrairá após o carregamento. Você usaria um aplicativo como o Adobe Bridge — que está incluído em todos os aplicativos da Adobe Creative Cloud — para preencher os dados nos campos de metadados padrão.

Ao seguir esse método, não será necessário inserir manualmente esses dados usando o Dynamic Media Classic. No entanto, também é possível usar as Predefinições de metadados no Dynamic Media Classic, como uma maneira rápida de inserir os mesmos dados a cada vez.

Para obter mais informações sobre esse tópico, consulte [Exibição, Adição e Exportação de Metadados](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![imagem](assets/video-overview/video-overview-7.jpg)

Depois que os metadados forem preenchidos, você poderá vê-los na Exibição de detalhes desse ativo de vídeo. As palavras-chave também podem estar presentes, mas elas estão localizadas na guia Palavras-chave.

- Saiba mais sobre [Adicionar palavras-chave](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- Saiba mais sobre [SEO do vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Saiba mais sobre [Configurações para SEO de vídeo](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Configuração do SEO de vídeo

A configuração da SEO de vídeo começa com a escolha do tipo de formato desejado, do método de geração e de quais campos de metadados deve entrar no arquivo.

1. Ir para **Configuração > Configuração do aplicativo > SEO de vídeo > Configurações**.
2. No **Modo de geração** escolha um formato de arquivo. O padrão é Desativado; para ativá-lo, escolha Mapa do site de vídeo, mRSS ou Ambos.
3. Escolha entre gerar automática ou manualmente. Para simplificar, recomendamos que você defina como **Modo Automático**. Se você escolher Automático, defina também a variável **Marcar para publicação** ou os arquivos não serão ativados. Os arquivos Sitemap e RSS são tipos de um documento XML e devem ser publicados como qualquer outro ativo. Use um dos modos manuais se você não tiver todas as informações prontas agora ou se quiser apenas fazer uma geração única.
4. Preencha as tags de metadados usadas nos arquivos. Esta etapa não é opcional. No mínimo, você deve incluir os três campos marcados com um asterisco (\*): **Landing Page** , **Título**, e **Descrição**. Para usar seus metadados para essas tarefas, arraste e solte os campos do painel Metadados à direita em um campo correspondente no formulário. O Dynamic Media Classic preencherá automaticamente o campo de espaço reservado com dados reais de cada vídeo. Não é necessário usar campos de metadados. Em vez disso, você pode digitar um texto estático aqui, mas esse mesmo texto será exibido para cada vídeo.
5. Depois de inserir as informações nos três campos obrigatórios, o Dynamic Media Classic habilitará a **Salvar** e **Salvar e gerar** botões. Clique em um para salvar as configurações. Uso **Salvar** se você estiver no Modo Automático e quiser que o Dynamic Media Classic gere os arquivos posteriormente. Uso **Salvar e gerar** para criar o arquivo imediatamente.

### Teste e publicação do mapa do site de vídeo, feed mRSS ou ambos os arquivos

Os arquivos gerados aparecerão no diretório raiz (base) da sua conta.

![imagem](assets/video-overview/video-overview-8.jpg)

Esses arquivos devem ser publicados, pois a ferramenta de SEO em vídeo não pode executar uma publicação sozinha. Desde que estejam marcados para publicação, eles são enviados para os servidores de publicação na próxima vez que uma publicação for executada.

Após a publicação, os arquivos ficam disponíveis usando esse formato de URL.

![imagem](assets/video-overview/video-url-format.png)

Exemplo:

![imagem](assets/video-overview/video-format-example.png)

### Enviando para Mecanismos de Pesquisa

A etapa final do processo é enviar seus arquivos e/ou URLs para mecanismos de pesquisa. O Dynamic Media Classic não pode fazer essa etapa por você; no entanto, supondo que você envie o URL e não o arquivo XML propriamente dito, seu feed deverá ser atualizado na próxima vez que seu arquivo for gerado e ocorrer uma publicação.

O método de envio para seu mecanismo de pesquisa varia, no entanto, para o Google, você usa as ferramentas do Webmaster da Google. Uma vez lá, vá para **Configuração do site > Mapas de site** e clique no botão **Enviar um mapa do site** botão. Aqui você pode colocar o URL do Dynamic Media Classic para seu(s) arquivo(s) SEO.

### Relatório de vídeo SEO

O Dynamic Media Classic fornece um relatório para mostrar quantos vídeos foram incluídos com êxito nos arquivos e, mais importante, quais não foram incluídos devido a erros. Para acessar o relatório, acesse **Configuração > Configuração do aplicativo > SEO de vídeo > Relatório**.

![imagem](assets/video-overview/video-overview-9.jpg)

## Implementação móvel para vídeo MP4

O Dynamic Media Classic não inclui Predefinições de visualizador para dispositivos móveis, pois os visualizadores não são necessários para reproduzir vídeo em dispositivos móveis compatíveis. Desde que você codifique para o formato H.264 MP4 — convertendo no upload ou pré-codificando em seu desktop — os tablets e smartphones compatíveis podem reproduzir seus vídeos sem precisar de um visualizador. Isso é compatível com dispositivos Android e iOS (iPhone e iPad).

O motivo para que nenhum visualizador seja necessário é porque ambas as plataformas têm suporte para H.264 nativo. Você pode incorporar o vídeo em uma página da Web HTML5 ou incorporar o vídeo no próprio aplicativo, e os sistemas operacionais Android e iOS fornecerão um controlador para reproduzir o vídeo.

Por causa disso, o Dynamic Media Classic não fornece um URL para um visualizador de dispositivos móveis, mas fornece um URL diretamente para o vídeo. Na janela de Pré-visualização de um vídeo MP4, há links para desktop e dispositivos móveis. O URL móvel aponta para o vídeo publicado.

Um aspecto importante a ser observado sobre o vídeo publicado é que o URL lista o caminho completo para o vídeo, não apenas a ID do ativo. Ao lidar com imagens, você chama a imagem pela ID de ativo, independentemente da estrutura de pastas. No entanto, para vídeos, você também deve especificar a estrutura da pasta. Nos URLs acima, o vídeo é armazenado no caminho:

![imagem](assets/video-overview/mobile-implement-1.png)

Isso também pode ser expresso como nome da empresa/caminho da pasta/nome do vídeo.

### Método #1: Reprodução do navegador — Código HTML5

Para incorporar o vídeo MP4 em uma página da Web, use a tag de vídeo HTML5.

![imagem](assets/video-overview/browser-playback.png)

Esse método também funcionará para a Web do desktop, no entanto, você pode ter problemas com o suporte ao navegador — nem todos os navegadores da Web do desktop oferecem suporte nativo ao vídeo H.264, incluindo o Firefox.

### Método #2: reprodução do aplicativo no iOS — Estrutura do reprodutor de mídia

Como alternativa, você pode incorporar o vídeo Dynamic Media Classic MP4 no código do aplicativo móvel. Este é um exemplo genérico para o iOS usando a estrutura do Media Player, fornecida apenas para fins ilustrativos:

![imagem](assets/video-overview/app-playback.png)

## Recursos adicionais

Assista ao [Dynamic Media Skill Builder: vídeo no Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) webinário sob demanda para saber como usar os recursos de vídeo no Dynamic Media Classic.
