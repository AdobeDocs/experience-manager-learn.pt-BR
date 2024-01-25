---
title: Fluxo de trabalho principal do Dynamic Media Classic e visualização de ativos
description: 'Saiba mais sobre o fluxo de trabalho principal do Dynamic Media Classic, que inclui as três etapas: Criar (e fazer upload), Autor (e Publicar) e Entregar. Em seguida, saiba como visualizar ativos no Dynamic Media Classic.'
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
duration: 614
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2658'
ht-degree: 0%

---

# Fluxo de trabalho principal do Dynamic Media Classic e visualização de ativos {#main-workflow}

O Dynamic Media é compatível com um processo de fluxo de trabalho Criar (e fazer upload), Autor (e Publicar) e Entregar. Você começa fazendo upload de ativos e, em seguida, fazendo algo com esses ativos, como criar um Conjunto de imagens e finalmente publicar para ativá-los. A etapa Criar é opcional para alguns fluxos de trabalho. Por exemplo, se o objetivo for apenas fazer o dimensionamento dinâmico e o zoom em imagens ou converter e publicar vídeo para transmissão, não há etapas de criação necessárias.

![imagem](assets/main-workflow/create-author-deliver.jpg)

O fluxo de trabalho nas soluções da Dynamic Media Classic consiste em três etapas principais:

1. Criar (e fazer upload) conteúdo de origem
2. Ativos do autor (e publicação)
3. Entregar ativos

## Etapa 1: criar (e fazer upload)

Este é o início do workflow. Nesta etapa, colete ou crie o conteúdo original que se encaixa no fluxo de trabalho que está sendo usado e faça upload dele para o Dynamic Media Classic. O sistema suporta vários tipos de arquivos para imagens, vídeo e fontes, mas também para PDF, Adobe Illustrator e Adobe InDesign.

Veja a lista completa de [Tipos de arquivo suportados](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Você pode fazer upload do conteúdo original de várias maneiras diferentes:

- Diretamente do desktop ou da rede local. [Saiba como](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- De um servidor FTP da Dynamic Media Classic. [Saiba como](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

O modo padrão é Da área de trabalho, onde você procura por arquivos em sua rede local e inicia o upload.

![imagem](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Não adicione as pastas manualmente. Em vez disso, execute um upload do FTP e use o **Incluir subpastas** opção para recriar a estrutura de pastas dentro do Dynamic Media Classic.

As duas opções de upload mais importantes são ativadas por padrão — **Marcar para publicação**, que discutimos anteriormente, e **Substituir**. Substituir significa que, se o arquivo que está sendo carregado tiver o mesmo nome de um arquivo que já está no sistema, o novo arquivo substituirá a versão existente. Se você desmarcar essa opção, talvez o arquivo não seja carregado.

### Substituir opções ao carregar imagens

Há quatro variações da opção Substituir imagem que podem ser definidas para toda a empresa e geralmente são mal compreendidas. Resumindo, você pode definir as regras de modo que deseje que ativos com o mesmo nome sejam substituídos com mais frequência ou que substituições ocorram com menos frequência (nesse caso, a nova imagem será renomeada com uma extensão &quot;-1&quot; ou &quot;-2&quot;).

- **Substituir na pasta atual, mesmo nome/extensão da imagem base**.
Essa opção é a regra mais rígida para substituição. Ela requer que você faça upload da imagem de substituição para a mesma pasta da original e que a imagem de substituição tenha a mesma extensão de nome de arquivo da original. Se esses requisitos não forem atendidos, uma duplicata será criada.

- **Substituir na pasta atual, mesmo nome de ativo base independentemente da extensão**.
Requer que você faça upload da imagem de substituição para a mesma pasta da original, no entanto, a extensão do nome do arquivo pode ser diferente da original. Por exemplo, chair.tif substitui chair.jpg.

- **Substituir em qualquer pasta, mesmo nome/extensão do ativo base**.
Exige que a imagem de substituição tenha a mesma extensão de nome de arquivo que a imagem original (por exemplo, chair.jpg deve substituir chair.jpg, não chair.tif ). Entretanto, é possível fazer upload da imagem de substituição em uma pasta diferente da original. A imagem atualizada está na nova pasta; o arquivo não pode mais ser encontrado em seu local original.

- **Substituir em qualquer pasta, mesmo nome de ativo base independentemente da extensão**.
Essa opção é a regra de substituição mais inclusiva. É possível fazer upload de uma imagem de substituição para uma pasta diferente da original, fazer upload de um arquivo com uma extensão de nome de arquivo diferente e substituir o arquivo original. Se o arquivo original estiver em uma pasta diferente, a imagem de substituição ficará localizada na nova pasta para a qual foi carregada.

Saiba mais sobre o [Opção de substituição de imagens](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Embora não seja obrigatório, ao fazer upload usando um dos dois métodos acima, você pode especificar Opções de trabalho para esse upload específico — por exemplo, para agendar um upload recorrente, definir opções de recorte no upload e muitos outros. Eles podem ser valiosos para alguns fluxos de trabalho, portanto, vale a pena considerar se podem ser para o seu.

Saiba mais sobre [Opções de trabalho](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

O upload é a primeira etapa necessária em qualquer fluxo de trabalho porque o Dynamic Media Classic não pode trabalhar com qualquer conteúdo que ainda não esteja em seu sistema. Em segundo plano, durante o upload, o sistema registra cada ativo carregado no banco de dados centralizado do Dynamic Media Classic, atribui uma ID e a copia para o armazenamento. Além disso, o sistema converte arquivos de imagem para um formato que permite redimensionamento dinâmico e zoom e converte arquivos de vídeo para o formato MP4 compatível com a Web.

### Conceito: veja o que acontece com as imagens ao carregá-las no Dynamic Media Classic

Ao fazer upload de uma imagem de qualquer tipo para o Dynamic Media Classic, ela é convertida em um formato de imagem principal chamado TIFF de Pirâmide ou TIFF P. Um TIFF P é semelhante ao formato de uma imagem de TIFF em camadas, exceto que, em vez de camadas diferentes, o arquivo contém vários tamanhos (resoluções) da mesma imagem.

![imagem](assets/main-workflow/pyramid-p-tiff.png)

À medida que a imagem é convertida, o Dynamic Media Classic captura uma &quot;captura&quot; do tamanho total da imagem, dimensiona esse tamanho pela metade e o salva, dimensiona pela metade novamente e o salva, e assim por diante, até que seja preenchido com até mesmo múltiplos do tamanho original. Por exemplo, um P-TIFF de 2000 pixels tem tamanhos de 1000, 500, 250 e 125 pixels (e menores) no mesmo arquivo. O arquivo P-TIFF é o formato do que é chamado de &quot;imagem mestre&quot; no Dynamic Media Classic.

Quando você solicita uma imagem de determinado tamanho, a criação do TIFF P permite que o Servidor de imagens do Dynamic Media Classic localize rapidamente o próximo tamanho maior e o dimensione. Por exemplo, se você carregar uma imagem de 2000 pixels e solicitar uma imagem de 100 pixels, o Dynamic Media Classic encontrará a versão de 125 pixels e a dimensionará para 100 pixels, em vez de dimensionar de 2000 para 100 pixels. Isso torna a operação muito rápida. Além disso, ao aplicar zoom em uma imagem, isso permite que o visualizador de zoom solicite apenas um bloco da imagem que está sendo ampliada, em vez da imagem com resolução total. É assim que o formato de imagem principal, o arquivo P-TIFF, suporta o dimensionamento dinâmico e o zoom.

Da mesma forma, você pode fazer upload do vídeo principal de origem no Dynamic Media Classic e, ao fazer upload, o Dynamic Media Classic pode redimensioná-lo automaticamente e convertê-lo para o formato MP4 compatível com a Web.

### Regras básicas para determinar o tamanho ideal das imagens que você carrega

**Carregue imagens no maior tamanho que precisar.**

- Se precisar aplicar o zoom, carregue uma imagem de alta resolução com um intervalo de 1500 a 2500 pixels na dimensão mais longa. Considere quantos detalhes você deseja fornecer, a qualidade das imagens de origem e o tamanho do produto que está sendo mostrado. Por exemplo, carregue uma imagem de 1000 pixels para um anel pequeno, mas uma imagem de 3000 pixels para uma cena inteira da sala.
- Se você não precisar aplicar o zoom, carregue-o no tamanho exato em que é exibido. Por exemplo, se você tiver logotipos ou imagens de abertura/banner para colocar em suas páginas, carregue-as exatamente no tamanho 1:1 e chame-as exatamente nesse tamanho.

**Nunca faça upsample ou exploda suas imagens antes de fazer upload para o Dynamic Media Classic.** Por exemplo, não remova a resolução de uma imagem menor para transformá-la em uma imagem de 2000 pixels. Não vai parecer bom. Faça suas imagens o mais próximo possível da perfeição antes de carregá-las.

**Não há tamanho mínimo para o zoom, mas, por padrão, os visualizadores não aplicarão o zoom além de 100%.** Se a imagem for muito pequena, ela não fará nenhum zoom ou apenas ampliará uma pequena quantidade para evitar que pareça ruim.

**Embora não haja um mínimo para o tamanho da imagem, não recomendamos carregar imagens gigantes.** Uma imagem gigante pode ser considerada com mais de 4000 pixels. O upload de imagens desse tamanho pode mostrar falhas potenciais, como grãos de poeira ou pelos na imagem. Essas imagens ocupam mais espaço no servidor do Dynamic Media Classic, o que pode fazer com que você ultrapasse os limites de armazenamento contratados.

Saiba mais sobre [Upload de arquivos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Etapa 2: Autor (e publicação)

Depois de criar e carregar seu conteúdo, você criará novos ativos de mídia avançada a partir dos ativos carregados executando um ou mais sub-workflows. Isso inclui todos os diferentes tipos de coleções de conjuntos — Imagem, Amostra, Rotação e Conjuntos de mídia mista, bem como Modelos. Também inclui vídeo. Posteriormente, entraremos em maiores detalhes sobre cada tipo de conjunto de coleção de imagens e mídia avançada de vídeo. No entanto, em quase todos os casos, você começa selecionando um ou mais ativos (ou sem ativos selecionados) e escolhendo o tipo de ativo que deseja criar. Por exemplo, você pode selecionar uma imagem principal e algumas exibições dessa imagem e optar por criar um Conjunto de imagens, uma coleção de exibições alternativas do mesmo produto.

>[!IMPORTANT]
>
>Verifique se todos os ativos estão marcados para publicação. Embora, por padrão, todos os ativos sejam marcados automaticamente para publicação ao carregar, todos os ativos recém-criados do conteúdo carregado também precisarão ser marcados para publicação.

Depois de criar o novo ativo, você executará um trabalho de publicação. Você pode fazer isso manualmente ou agendar um trabalho de publicação que é executado automaticamente. Publicar copia todo o conteúdo da esfera privada, da esfera do Dynamic Media Classic para a esfera pública e do servidor de publicação da equação. O produto de um trabalho de publicação do Dynamic Media é um URL exclusivo para cada ativo publicado.

O servidor no qual você publica depende do tipo de conteúdo e fluxo de trabalho. Por exemplo, todas as imagens vão para o Servidor de imagens e o vídeo de fluxo contínuo para o Servidor FMS. Para maior comodidade, falaremos de &quot;publicar&quot; como um único evento em um único servidor.

Publicar publica todo o conteúdo marcado para publicação, não apenas o seu conteúdo. Um único administrador normalmente publica em nome de todos, em vez de usuários individuais que executam uma publicação. O administrador pode publicar conforme necessário ou configurar um trabalho recorrente diário, semanal ou até mesmo a cada 10 minutos que será publicado automaticamente. Publique de acordo com uma programação adequada para sua empresa.

>[!TIP]
>
>Automatize os trabalhos de publicação e agende uma Publicação completa para ser executada todos os dias às 12h ou a qualquer momento depois da noite.

### Conceito: entender o URL do Dynamic Media Classic

O produto final de um fluxo de trabalho do Dynamic Media Classic é um URL que aponta para o ativo (seja conjunto de imagens ou conjunto de vídeos adaptáveis). Esses URLs são muito previsíveis e seguem o mesmo padrão. No caso de imagens, cada imagem é gerada a partir da imagem principal P-TIFF.

Esta é a sintaxe para o URL de uma imagem com alguns exemplos:

![imagem](assets/main-workflow/dmc-url.jpg)

No URL, tudo à esquerda do ponto de interrogação é o caminho virtual para uma imagem específica. Tudo à direita do ponto de interrogação é um modificador do Servidor de imagens, uma instrução para processar a imagem. Quando você tem vários modificadores, eles são separados por &quot;E&quot; comercial.

No primeiro exemplo, o caminho virtual para a imagem &quot;Backpack_A&quot; é `http://sample.scene7.com/is/image/s7train/BackpackA`. Os modificadores do Servidor de imagens redimensionam a imagem para uma largura de 250 pixels (a partir de wid=250) e reexemplifica a imagem usando o algoritmo de interpolação Lanczos, que aumenta a nitidez à medida que redimensiona (a partir de resMode=sharp2).

O segundo exemplo aplica o que é conhecido como &quot;predefinição de imagem&quot; à mesma imagem Backpack_A, conforme indicado por $!_template300$. Os símbolos $ em ambos os lados da expressão indicam que uma predefinição de imagem, um conjunto empacotado de modificadores de imagem, está sendo aplicada à imagem.

Assim que você entender como os URLs do Dynamic Media Classic são organizados, entenderá como alterá-los de forma programática e como integrá-los mais profundamente ao seu site e sistemas de back-end.

### Conceito: Entendendo o atraso do armazenamento em cache

Os ativos recém-carregados e publicados são vistos imediatamente, enquanto os ativos atualizados podem estar sujeitos ao atraso de armazenamento em cache de 10 horas. Por padrão, todos os ativos publicados têm um mínimo de 10 horas antes de expirarem. Dizemos mínimo, porque cada vez que a imagem é visualizada, ela inicia um relógio que não expira até que 10 horas tenham decorrido, no qual ninguém visualizou a imagem. Esse período de 10 horas é o &quot;Tempo de vida&quot; de um ativo. Depois que o cache expirar para esse ativo, a versão atualizada poderá ser entregue.

Normalmente, isso não é um problema, a menos que ocorra um erro e a imagem/ativo tenha o mesmo nome da versão publicada anteriormente, mas há um problema com a imagem. Por exemplo, você carregou acidentalmente uma versão de baixa resolução ou o diretor de arte não aprovou a imagem. Nesse caso, é necessário recuperar a imagem original e substituí-la por uma nova versão usando a mesma ID do ativo.

Saiba como [Limpe manualmente o cache para os URLs que precisam ser atualizados](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=pt-BR).

>[!TIP]
>
>Para evitar problemas com atraso de armazenamento em cache, sempre trabalhe com antecedência — uma noite, um dia, duas semanas etc. Crie em tempo hábil para garantir a qualidade/aceitação para que as partes internas testem seu trabalho antes de lançá-lo ao público. Mesmo trabalhar uma noite antes do permite fazer alterações e republicar essa noite. Pela manhã, decorreram 10 horas e o cache é atualizado com a imagem correta.

- Saiba mais sobre [Criação de um trabalho de publicação](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Saiba mais sobre [Publicação](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Etapa 3: entrega

Lembre-se de que o produto final de um fluxo de trabalho do Dynamic Media Classic é um URL que aponta para o ativo. O URL pode apontar para uma imagem individual, um Conjunto de imagens, um Conjunto de rotação ou alguma outra coleção ou vídeo de Conjunto de imagens. Você precisa pegar esse URL e fazer algo com ele, como editar seu HTML, para que a variável `<IMG>` as tags apontam para a imagem do Dynamic Media Classic em vez de apontar para uma imagem vinda do site atual.

Na etapa Entrega, você deve integrar esses URLs ao seu site, aplicativo móvel, campanha de email ou qualquer outro ponto de contato digital no qual deseja exibir o ativo.

Exemplo de integração do URL do Dynamic Media Classic para uma imagem em um site:

![imagem](assets/main-workflow/example-url-1.png)

O URL em vermelho é o único elemento específico do Dynamic Media Classic.

Sua equipe de TI ou parceiro de integração pode assumir a liderança na gravação e alteração do código para integrar URLs do Dynamic Media Classic ao seu site. A Adobe tem uma equipe de consultoria que pode ajudar nesse esforço, fornecendo orientação técnica, criativa ou geral.

Para soluções mais complexas, como visualizadores de zoom ou visualizadores que combinam zoom com visualizações alternativas, normalmente o URL aponta para um visualizador hospedado pelo Dynamic Media Classic. Além disso, nesse URL, há uma referência a uma ID do ativo.

Exemplo de um link (em vermelho) que abrirá um Conjunto de imagens em um visualizador em uma nova janela pop-up:

![imagem](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>É necessário integrar os URLs do Dynamic Media Classic ao seu site, aplicativo móvel, email e outros pontos de contato digitais. A Dynamic Media Classic não pode fazer isso para você.

## Visualização de ativos

Você provavelmente desejará visualizar os ativos carregados ou que estão sendo criados ou editados para garantir que eles apareçam como você deseja quando os clientes os visualizarem. É possível acessar a janela Visualizar clicando em qualquer **Visualizar** na miniatura do ativo, na parte superior do menu **Painel Procurar/Construir**, ou acessando **Arquivo > Visualizar**. Em uma janela do navegador, ela visualizará qualquer ativo que esteja atualmente no painel, seja uma imagem, vídeo ou ativo criado, como um Conjunto de imagens.

### Visualização do tamanho dinâmico (predefinições de imagem)

É possível visualizar imagens em vários tamanhos usando o **Tamanhos** visualização. Isso carrega uma lista de suas Predefinições de imagem disponíveis. Posteriormente, discutiremos as Predefinições de imagem, mas as consideraremos &quot;receitas&quot; para carregar sua imagem em um tamanho nomeado, com quantidades específicas de nitidez e qualidade de imagem.

### Visualização do zoom

Você também pode usar a variável **Zoom** opção para visualizar a imagem em uma das muitas predefinições de zoom pré-criadas, que são baseadas em diferentes visualizadores de zoom incluídos.

Saiba mais sobre [Visualização de ativos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
