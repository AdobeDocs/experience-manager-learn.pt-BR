---
title: Fluxo de trabalho principal do Dynamic Media Classic e visualização de ativos
description: Saiba mais sobre o fluxo de trabalho principal no Dynamic Media Classic, que inclui as três etapas - Criar (e fazer upload), Autor (e publicar) e Entrega. Em seguida, saiba como visualizar ativos no Dynamic Media Classic.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Gerenciamento de conteúdo
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2737'
ht-degree: 1%

---


# Fluxo de trabalho principal do Dynamic Media Classic e visualização de ativos {#main-workflow}

O Dynamic Media é compatível com um processo de fluxo de trabalho Criar (e fazer upload), Autor (e Publicação) e Deliver . Você começa fazendo upload de ativos e, em seguida, algo com esses ativos, como criar um Conjunto de imagens e, por fim, publicar para torná-los ativos. A etapa Criar é opcional para alguns fluxos de trabalho. Por exemplo, se o objetivo for fazer apenas dimensionamento dinâmico e ampliar imagens ou converter e publicar vídeos para transmissão, não haverá etapas de criação necessárias.

![imagem](assets/main-workflow/create-author-deliver.jpg)

O fluxo de trabalho nas soluções Dynamic Media Classic consiste em três etapas principais:

1. Criar (e fazer upload) SourceContent
2. Criar (e publicar) ativos
3. Entregar ativos

## Etapa 1: Criar (e fazer upload)

Este é o início do workflow. Nesta etapa, você coleta ou cria o conteúdo de origem que se encaixa no fluxo de trabalho que está usando e o carrega no Dynamic Media Classic. O sistema oferece suporte a vários tipos de arquivo para imagens, vídeo e fontes, mas também para PDF, Adobe Illustrator e Adobe InDesign.

Consulte a lista completa de [Tipos de Arquivo Suportados](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Você pode fazer upload do conteúdo de origem de várias maneiras diferentes:

- Diretamente da área de trabalho ou rede local. [Saiba como](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- De um servidor FTP do Dynamic Media Classic. [Saiba como](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

O modo padrão é A partir da área de trabalho, onde você procura arquivos na rede local e inicia o upload.

![imagem](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Não adicione manualmente as pastas. Em vez disso, execute um upload do FTP e use a opção **Include Subfolders** para recriar a estrutura da pasta no Dynamic Media Classic.

As duas opções de upload mais importantes são ativadas por padrão — **Marcar para publicação**, o que discutimos anteriormente, e **Substituir**. Substituir significa que, se o arquivo que está sendo carregado tiver o mesmo nome de um arquivo já no sistema, o novo arquivo substituirá a versão existente. Se você desmarcar esta opção, talvez o arquivo não seja carregado.

### Opções de Substituição Ao Fazer Upload De Imagens

Há quatro variações da opção Substituir imagem que podem ser definidas para toda a sua empresa e que geralmente são mal compreendidas. Resumindo, você define as regras de modo que deseja que os ativos com o mesmo nome sejam substituídos com mais frequência, ou deseja que as substituições ocorram com menos frequência (nesse caso, a nova imagem será renomeada com uma extensão &quot;-1&quot; ou &quot;-2&quot;).

- **Substituir na pasta atual, mesmo nome/extensão** de imagem base.
Essa opção é a regra mais rígida para substituição. Ela requer que você carregue a imagem de substituição na mesma pasta do original e que a imagem de substituição tenha a mesma extensão de nome de arquivo do original. Se esses requisitos não forem atendidos, uma duplicata será criada.

- **Substituir na pasta atual, o mesmo nome do ativo base independentemente da extensão**.
Requer que você carregue a imagem de substituição na mesma pasta do original, no entanto, a extensão do nome do arquivo pode ser diferente do original. Por exemplo, chair.tif substitui chair.jpg.

- **Substituir em qualquer pasta, o mesmo nome/extensão** do ativo base.
Requer que a imagem de substituição tenha a mesma extensão de nome de arquivo que a imagem original (por exemplo, chair.jpg deve substituir chair.jpg, não chair.tif ). No entanto, é possível fazer upload da imagem de substituição para uma pasta diferente da original. A imagem atualizada reside na nova pasta; o arquivo não pode mais ser encontrado em seu local original.

- **Substitua em qualquer pasta, o mesmo nome do ativo base, independentemente da extensão**.
Essa é a regra de substituição mais inclusiva. Você pode fazer upload de uma imagem de substituição para uma pasta diferente do original, fazer upload de um arquivo com uma extensão de nome de arquivo diferente e substituir o arquivo original. Se o arquivo original estiver em uma pasta diferente, a imagem de substituição residirá na nova pasta para a qual foi carregada.

Saiba mais sobre a [Opção de Substituição de Imagens](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Embora não seja necessário, ao fazer upload usando qualquer um dos dois métodos acima, você pode especificar as Opções de trabalho para esse upload em particular — por exemplo, para agendar um upload recorrente, definir as opções de corte no upload e muitos outros. Eles podem ser valiosos para alguns fluxos de trabalho, portanto, vale a pena considerar se podem ser para os seus.

Saiba mais sobre [Opções de trabalho](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

O upload é a primeira etapa necessária em qualquer fluxo de trabalho, pois o Dynamic Media Classic não pode funcionar com nenhum conteúdo que ainda não esteja no sistema. Em segundo plano durante o upload, o sistema registra cada ativo carregado no banco de dados centralizado do Dynamic Media Classic, atribui uma ID e a copia para o armazenamento. Além disso, o sistema converte arquivos de imagem em um formato que permite redimensionamento dinâmico e zoom e converte arquivos de vídeo para o formato compatível com a Web MP4.

### Conceito: Veja o que acontece com as imagens quando você as carrega no Dynamic Media Classic

Ao fazer upload de uma imagem de qualquer tipo para o Dynamic Media Classic, ela é convertida em um formato de imagem mestre chamado Pyramid TIFF ou P-TIFF. Um P-TIFF é semelhante ao formato de uma imagem de bitmap TIFF em camadas, exceto que, em vez de camadas diferentes, o arquivo contém vários tamanhos (resoluções) da mesma imagem.

![imagem](assets/main-workflow/pyramid-p-tiff.png)

À medida que a imagem é convertida, o Dynamic Media Classic captura um &quot;instantâneo&quot; do tamanho total da imagem, dimensiona-o pela metade e salva-o, dimensiona-o pela metade novamente e o salva, e assim por diante, até ser preenchido com múltiplos do tamanho original. Por exemplo, um P-TIFF de 2000 pixels terá tamanhos de 1000, 500, 250 e 125 pixels (e menores) no mesmo arquivo. O arquivo P-TIFF é o formato do que é chamado de &quot;imagem mestre&quot; no Dynamic Media Classic.

Quando você solicita uma imagem de determinado tamanho, a criação do P-TIFF permite que o Servidor de imagem do Dynamic Media Classic encontre rapidamente o próximo tamanho maior e o dimensione. Por exemplo, se você carregar uma imagem de 2000 pixels e solicitar uma imagem de 100 pixels, o Dynamic Media Classic encontrará a versão de 125 pixels e a dimensionará para 100 pixels em vez de dimensionar de 2000 para 100 pixels. Isso torna a operação muito rápida. Além disso, ao ampliar uma imagem, isso permite que o visualizador de zoom solicite apenas um bloco da imagem que está sendo ampliada, em vez de toda a imagem de resolução completa. É assim que o formato da imagem mestre, o arquivo P-TIFF, suporta dimensionamento dinâmico e zoom.

Da mesma forma, você pode fazer upload do vídeo de origem mestre no Dynamic Media Classic e, ao fazer upload do Dynamic Media Classic, redimensioná-lo automaticamente e convertê-lo no formato compatível com a Web MP4.

### Regras de polegar para determinar o tamanho ideal para as imagens que você carrega

**Carregue imagens no maior tamanho necessário.**

- Se precisar aplicar zoom, carregue uma imagem de alta resolução de um intervalo de 1500-2500 pixels na dimensão mais longa. Considere quantos detalhes deseja fornecer, a qualidade das imagens de origem e o tamanho do produto sendo exibido. Por exemplo, carregue uma imagem de 1000 pixels para um minúsculo anel, mas uma imagem de 3000 pixels para uma cena de salas inteiras.
- Se não precisar aplicar zoom, faça o upload no tamanho exato em que será visto. Por exemplo, se você tiver logotipos ou imagens de abertura/banner para colocar em suas páginas, carregue-os exatamente no tamanho 1:1 e chame-os exatamente nesse tamanho.

**Nunca faça upload ou exploda suas imagens antes de fazer upload para o Dynamic Media Classic.** Por exemplo, não faça o upsample de uma imagem menor para torná-la uma imagem de 2000 pixels. Não vai ficar bom. Faça suas imagens o mais perto possível da perfeição antes de fazer upload.

**Não há tamanho mínimo para zoom, mas, por padrão, os visualizadores não terão mais zoom do que 100%.** Se a imagem for muito pequena, não terá zoom algum ou apenas aumentará o zoom de uma pequena quantidade para evitar que pareça ruim.

**Embora não haja um mínimo para o tamanho da imagem, não recomendamos fazer upload de imagens gigantes.** Uma imagem gigante pode ser considerada mais de 4000 pixels. Fazer upload de imagens desse tamanho pode mostrar possíveis falhas, como grãos de pó ou pêlos na imagem. Essas imagens também ocuparão mais espaço no servidor do Dynamic Media Classic, o que pode fazer com que você ultrapasse os limites de armazenamento contratado.

Saiba mais sobre o [Upload de arquivos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Etapa 2: Autor (e publicação)

Após criar e carregar seu conteúdo, você criará novos ativos de mídia avançada a partir dos ativos carregados, executando um ou mais sub-workflows. Isso inclui todos os diferentes tipos de coleções de conjuntos — Imagem, Amostra, Rotação e Conjuntos de mídias mistas, bem como Modelos. Também inclui vídeo. Posteriormente, entraremos em detalhes muito maiores sobre cada tipo de conjunto de coleção de imagens e mídia avançada de vídeo. No entanto, em quase todos os casos, você começa selecionando um ou mais ativos (ou não tem nenhum ativo selecionado) e escolhendo o tipo de ativo que deseja criar. Por exemplo, você pode selecionar uma imagem principal e algumas visualizações dessa imagem e optar por criar um Conjunto de imagens, uma coleção de visualizações alternativas do mesmo produto.

>[!IMPORTANT]
>
>Certifique-se de que todos os ativos estejam marcados para publicação. Embora por padrão todos os ativos sejam marcados automaticamente para publicação no upload, todos os ativos recém-criados do conteúdo carregado também precisarão ser marcados para publicação.

Depois de criar seu novo ativo, você executará um trabalho de publicação. Você pode fazer isso manualmente ou agendar um trabalho de publicação executado automaticamente. A publicação copia todo o conteúdo da esfera privada do Dynamic Media Classic para o público, publicando a esfera do servidor da equação. O produto de um trabalho de publicação do Dynamic Media é um URL exclusivo para cada ativo publicado.

O servidor para o qual você publica depende do tipo de conteúdo e fluxo de trabalho. Por exemplo, todas as imagens vão para o Servidor de imagem e fazem streaming de vídeo para o Servidor FMS. Para maior comodidade, falaremos de &quot;publicar&quot; como um único evento em um único servidor.

A publicação publica todo o conteúdo marcado para publicação — não apenas o seu conteúdo. Um único administrador geralmente publica em nome de todos, em vez de usuários individuais que executam uma publicação. O administrador pode publicar conforme necessário ou configurar um trabalho recorrente diário, semanal ou até mesmo a cada 10 minutos que será publicado automaticamente. Publique em um agendamento que faça sentido para sua empresa.

>[!TIP]
>
>Automatize seus trabalhos de publicação e agende uma publicação completa para ser executada todos os dias às 12h ou a qualquer momento tarde da noite.

### Conceito: Como entender o URL do Dynamic Media Classic

O produto final de um fluxo de trabalho do Dynamic Media Classic é um URL que aponta para o ativo (seja o conjunto de imagens ou o conjunto de vídeos adaptáveis). Esses URLs são muito previsíveis e seguem o mesmo padrão. No caso de imagens, cada imagem é gerada a partir da imagem principal P-TIFF.

Esta é a sintaxe do URL de uma imagem com alguns exemplos:

![imagem](assets/main-workflow/dmc-url.jpg)

Na URL, tudo à esquerda do ponto de interrogação é o caminho virtual para uma imagem específica. Tudo à direita do ponto de interrogação é um modificador de Servidor de Imagem, uma instrução para como processar a imagem. Quando você tem vários modificadores, eles são separados por &quot;E&quot; comercial (&amp;).

No primeiro exemplo, o caminho virtual para a imagem &quot;Backpack_A&quot; é `http://sample.scene7.com/is/image/s7train/BackpackA`. Os modificadores do Servidor de Imagem redimensionam a imagem para uma largura de 250 pixels (de wid=250) e exemplificam novamente a imagem usando o algoritmo de interpolação Lanczos, que aumenta a nitidez à medida que redimensiona (de resMode=shark2).

O segundo exemplo aplica o que é conhecido como &quot;predefinição de imagem&quot; à mesma imagem Backpack_A, conforme indicado por $!_template300$. Os símbolos $ em qualquer lado da expressão indicam que uma predefinição de imagem, um conjunto empacotado de modificadores de imagem, está sendo aplicada à imagem.

Assim que você entender como os URLs do Dynamic Media Classic são reunidos, você saberá como alterá-los de forma programática e como integrá-los mais profundamente em seu site e sistemas de back-end.

### Conceito: Noções básicas sobre o atraso do armazenamento em cache

Os ativos recém-carregados e publicados serão visualizados imediatamente, enquanto os ativos atualizados podem estar sujeitos ao atraso de armazenamento em cache de 10 horas. Por padrão, todos os ativos publicados têm um mínimo de 10 horas antes da expiração. Dizemos mínimo, porque toda vez que a imagem é visualizada, ele inicia um relógio que não expirará até que 10 horas tenham decorrido, no qual ninguém visualizou a imagem. Esse período de 10 horas é o &quot;Tempo de vida&quot; de um ativo. Quando o cache expira para esse ativo, a versão atualizada pode ser entregue.

Normalmente, isso não é um problema a menos que um erro tenha ocorrido e a imagem/ativo tenha o mesmo nome da versão publicada anteriormente, mas há um problema com a imagem. Por exemplo, você carregou acidentalmente uma versão de baixa resolução ou o diretor de arte não aprovou a imagem. Nesse caso, você deseja lembrar a imagem original e substituí-la por uma nova versão usando a mesma ID de ativo.

Saiba como [Limpar manualmente o cache para os URLs que precisam ser atualizados](https://docs.adobe.com/content/help/en/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html).

>[!TIP]
>
>Para evitar problemas de atraso de armazenamento em cache, sempre trabalhe com antecedência — uma noite, um dia, duas semanas etc. Crie tempo para controle de qualidade/aceitação para que as partes internas comprovem seu trabalho antes de divulgá-lo ao público. Mesmo trabalhando uma noite antes, você pode fazer alterações e republicar essa noite. Pela manhã, as 10 horas expiraram e o cache é atualizado com a imagem correta.

- Saiba mais sobre como [Criar um trabalho de publicação](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Saiba mais sobre [Publicação](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Etapa 3: Delivery

Lembre-se de que o produto final de um fluxo de trabalho do Dynamic Media Classic é um URL que aponta para o ativo. O URL pode apontar para uma imagem individual, um Conjunto de imagens, um Conjunto de rotação ou qualquer outra coleção ou vídeo do Conjunto de imagens. Você precisa pegar essa URL e fazer algo com ela, como editar seu HTML para que as tags `<IMG>` apontem para a imagem do Dynamic Media Classic em vez de apontar para uma imagem que vem do seu site atual.

Na etapa Deliver , você deve integrar esses URLs ao seu site, aplicativo móvel, campanha por email ou qualquer outro ponto de contato digital no qual deseja exibir o ativo.

Exemplo de integração do URL do Dynamic Media Classic para uma imagem em um site:

![imagem](assets/main-workflow/example-url-1.png)

O URL em vermelho é o único elemento específico do Dynamic Media Classic.

Sua equipe de TI ou parceiro de integração pode assumir a liderança ao escrever e alterar o código para integrar URLs do Dynamic Media Classic ao site. A Adobe tem uma equipe de consultoria que pode ajudar nesse esforço, fornecendo orientação técnica, criativa ou geral.

Para soluções mais complexas, como visualizadores de zoom, ou visualizadores que combinam zoom com visualizações alternativas, o URL normalmente aponta para um visualizador hospedado pelo Dynamic Media Classic, e também dentro desse URL é uma referência a uma ID de ativo.

Exemplo de um link (em vermelho) que abrirá um Conjunto de imagens em um visualizador em uma nova janela pop-up:

![imagem](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Você precisa integrar os URLs do Dynamic Media Classic no seu site, aplicativo móvel, email e outros pontos de contato digitais — o Dynamic Media Classic não pode fazer isso para você!

## Visualizar ativos

Você provavelmente desejará visualizar os ativos que carregou ou que estão sendo criados ou editados para garantir que eles sejam exibidos da maneira desejada quando seus clientes os visualizarem. Você pode acessar a janela Visualização clicando em qualquer botão **Visualizar**, na miniatura do ativo, na parte superior do **Procurar/Criar painel**, ou acessando **Arquivo > Visualizar**. Em uma janela do navegador, ele visualizará o ativo que estiver no painel, seja uma imagem, um vídeo ou um ativo criado, como um Conjunto de imagens.

### Visualização do tamanho dinâmico (predefinições da imagem)

Você pode visualizar suas imagens em vários tamanhos usando a visualização **Tamanhos**. Essa ação carrega uma lista de suas Predefinições de imagem disponíveis. Posteriormente, discutiremos as Predefinições de imagem, mas considere-as como &quot;receitas&quot; para carregar sua imagem em um tamanho nomeado com quantidades específicas de nitidez e qualidade de imagem.

### Visualização de zoom

Você também pode usar a opção **Zoom** para visualizar sua imagem em uma de várias predefinições de zoom pré-criadas, que são baseadas em diferentes visualizadores de zoom incluídos.

Saiba mais sobre [Visualização de ativos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/previewing-asset.html).
