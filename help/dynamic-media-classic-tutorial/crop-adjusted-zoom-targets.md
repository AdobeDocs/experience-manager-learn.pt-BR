---
title: Destinos de corte, imagens ajustadas e zoom
description: A imagem mestra do Dynamic Media Classic é compatível com a criação de versões cortadas separadas de cada imagem para mostrar detalhes ou para amostras sem precisar criar versões cortadas separadas de cada imagem. Saiba como recortar imagens no Dynamic Media Classic e salvar como um novo arquivo mestre ou uma imagem virtual, salvar imagens ajustadas virtuais e usá-las no lugar de ativos mestres e criar Destinos de zoom nas imagens para mostrar os detalhes destacados.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: a1d83c77-a9e4-4ed1-9b00-65fb002164c0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2653'
ht-degree: 0%

---

# Destinos de corte, imagens ajustadas e zoom {#crop-adjusted-zoom-targets}

Um dos principais pontos fortes do conceito de imagem principal do Dynamic Media Classic é que você pode redefinir a finalidade do ativo de imagem para muitos usos. Tradicionalmente, era necessário criar versões separadas e cortadas de cada imagem para exibir detalhes ou para amostras. Ao usar o Dynamic Media Classic, você pode fazer as mesmas tarefas em seu único Principal e salvar essas versões cortadas como novos arquivos físicos ou como derivados virtuais que não ocupam espaço de armazenamento.

Ao final desta seção do tutorial, você saberá como:

- Recorte imagens no Dynamic Media Classic e salve como novos arquivos mestres ou como imagens virtuais. [Saiba mais](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html).
- Salve as Imagens ajustadas virtuais e use-as no lugar dos ativos principais. [Saiba mais](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/adjusting-image.html).
- Criar Destinos de Zoom nas imagens para exibir seus destaques. [Saiba mais](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Cortar

O Dynamic Media Classic tem algumas ferramentas de edição de imagens convenientemente disponíveis na interface do usuário, incluindo a ferramenta Corte demarcado. Talvez você queira recortar a imagem principal dentro do Dynamic Media Classic por vários motivos. Por exemplo:

- Você não tem acesso ao arquivo original. Você deseja exibir a imagem com um corte diferente ou taxa de proporção, mas não tem o arquivo original no computador ou está trabalhando em casa. Nesse caso, você pode acessar o Dynamic Media Classic, localizar a imagem, recortá-la e salvá-la ou salvá-la como uma nova versão.
- Para remover o excesso de espaço em branco. A imagem foi fotografada com muito espaço em branco, o que faz com que o produto pareça pequeno. Você deseja que suas imagens em miniatura preencham a tela o máximo possível.
- Para criar imagens ajustadas, cópias virtuais de imagens que não ocupam espaço em disco. Algumas empresas têm regras de negócios que exigem que elas mantenham cópias separadas da mesma imagem, mas com um nome diferente. Ou talvez você queira uma versão cortada e não cortada da mesma imagem.
- Para criar novas imagens a partir de uma imagem de origem. Por exemplo, talvez você queira criar amostras de cores ou um detalhe da imagem principal. Você pode fazer isso no Adobe Photoshop e carregar separadamente ou usar a ferramenta Corte demarcado no Dynamic Media Classic.

>[!NOTE]
>
>Todos os URLs nas discussões de Corte a seguir são apenas para fins ilustrativos; eles não são links em tempo real.

### Uso da ferramenta Corte demarcado

É possível acessar a ferramenta Recorte no Dynamic Media Classic na página Detalhes de um ativo ou clicando no **Editar** botão. Você pode usar a ferramenta para recortar de duas maneiras:

- O modo de corte padrão no qual você arrasta as alças da janela de corte ou digita valores na caixa Tamanho. Saiba como [Cortar manualmente](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Aparar. Use essa opção para remover espaços em branco extras em torno da imagem, calculando o número de pixels que não correspondem a ela. Saiba como [Cortar por corte](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Corte manual_

Quando você salva uma versão cortada manualmente, parece que a imagem é cortada permanentemente; na verdade, o Dynamic Media Classic oculta os pixels adicionando um modificador de URL interno para cortar a imagem. Ao publicar, todos perceberão que a imagem foi cortada. No entanto, você poderá retornar ao Editor de corte e remover o corte posteriormente.

Em seguida, você pode escolher se deseja salvar como uma Nova Imagem-Mestre ou como uma View Adicional da Imagem-Mestre. Um novo mestre é um novo arquivo físico (como um TIFF ou JPEG) que ocupa espaço de armazenamento. Uma exibição adicional é uma imagem virtual que não ocupa espaço no servidor. Não recomendamos que você escolha Substituir original, pois isso substituirá seu original e tornará o recorte permanente. Se você salvar como um novo mestre ou como uma visualização adicional, deverá escolher uma nova ID do ativo. Como outras IDs de ativos, deve ser um nome exclusivo no Dynamic Media Classic.

### _Cortar corte_

Se você fizer upload de uma imagem com muito espaço em branco (tela extra) em torno do assunto principal da imagem, ela parecerá muito menor na Web quando redimensionada. Isso é especialmente verdadeiro para imagens em miniatura de 150 pixels ou menores — o assunto da foto pode se perder em todo o espaço extra ao redor dele.

Compare essas duas versões da mesma imagem.

![imagem](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

A imagem à direita é muito mais proeminente removendo o espaço extra ao redor do produto. O corte pode ser feito uma imagem por vez, usando a ferramenta Corte Demarcado, ou executado como um processo em lote, conforme você faz o upload. Recomendamos executar como um processo em lote se você quiser que todas as imagens sejam cortadas de forma consistente até os limites do assunto principal. Cortar cortes na caixa delimitadora — o retângulo ao redor da imagem.

>[!NOTE]
>
>Cortar não cria transparência ao redor da imagem. Para isso, seria necessário incorporar um demarcador de recorte à imagem e usar o **Criar máscara a partir do caminho do clipe** opção de upload.
>
>Além disso, para restaurar uma imagem ao seu estado original depois de cortá-la quando tiver usado o **Salvar** , exiba a imagem na tela Editor de corte e selecione a **Redefinir** botão.

### _Cortar no upload_

Como mencionado anteriormente, você também pode optar por cortar as imagens ao fazer upload delas. Para usar cortar recorte no upload, clique no link **Opções de trabalho** e, em Opções de corte, escolha **Cortar**.

O Dynamic Media Classic se lembrará dessa opção para o próximo upload. Embora você queira recortar as imagens para este upload, talvez você não queira recortá-las para cada upload. Outra opção seria definir um trabalho de upload de FTP especial agendado e colocar as opções de corte lá. Dessa forma, você só executaria o trabalho quando precisasse cortar suas imagens.

>[!IMPORTANT]
>
>Se você definir um recorte para o upload, o Dynamic Media Classic colocará um cookie para lembrar dessa configuração na próxima vez. Como prática recomendada, clique no link **Redefinir para os padrões da empresa** antes do próximo upload para limpar todas as opções de corte restantes do último upload. Caso contrário, você pode cortar acidentalmente o próximo lote de imagens.

### Cortar por URL

Embora isso não seja óbvio no Dynamic Media Classic, também é possível cortar puramente pelo URL (ou até mesmo adicionar o recorte a uma predefinição de imagem).

Sempre que usar a ferramenta Corte demarcado, você verá os valores de URL no campo na parte inferior. Você pode pegar esses valores e aplicá-los diretamente a uma imagem como modificadores de URL.

![imagem](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_Modificadores do comando Cortar na parte inferior do Editor de corte_

![imagem](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Como o tamanho deve ser calculado com base na imagem ao usar o corte por corte, não é possível automatizá-lo por meio do URL. O corte só pode ser executado durante o upload ou ao aplicá-lo uma imagem por vez.

### _Corte na predefinição de imagem_

As Predefinições de imagem têm um campo onde você pode adicionar comandos extras do Servidor de imagens. Para adicionar o mesmo recorte acima à Predefinição de imagem, edite a predefinição e cole ou digite os valores no campo Modificadores de URL e, em seguida, salve e publique.

![imagem](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_Adicione comandos de corte (ou qualquer comando) aos Modificadores de URL da Predefinição de imagem._

O corte agora fará parte dessa Predefinição de imagem e será aplicado automaticamente sempre que for usado. É claro que esse método depende de todas as imagens que precisam da mesma quantidade de corte. Se suas imagens não forem capturadas da mesma maneira, esse método não funcionará para você.

## Imagens ajustadas

Ao usar a ferramenta Cortar, você tem a opção de **Salvar como Exibição Adicional do Mestre**. Quando salvo, cria um novo tipo de ativo do Dynamic Media Classic — uma Imagem ajustada. Uma Imagem ajustada, também chamada de derivada, é uma imagem virtual. Na verdade, não é uma imagem; é uma referência de banco de dados (como um alias ou atalho) para a imagem mestra física.

### A Imagem Real, Por Favor, Levante-Se`?`

Você sabe qual é o mestre e qual é a Imagem ajustada?

![imagem](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

Você não deve conseguir dizer isso sem olhar no Dynamic Media Classic e ver o tipo de ativo de &quot;Imagem ajustada&quot; para SBR_MAIN2.

Uma Imagem ajustada não usa espaço em disco, pois existe apenas como um item da linha no banco de dados. Ela também está permanentemente vinculada ao ativo original; se o original for excluído, a Imagem ajustada também será excluída. Ela pode consistir em uma imagem inteira não cortada ou apenas em parte de uma imagem (um corte).

![imagem](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Normalmente, as Imagens ajustadas são criadas com a ferramenta Corte demarcado; no entanto, elas também podem ser criadas com outros editores de imagem: as ferramentas Ajustar e Nitidez.

As imagens ajustadas exigem uma ID de ativo exclusiva. Quando publicados (você deve publicar como qualquer outro ativo), eles atuam como qualquer outra imagem e são chamados em um URL pela ID do ativo. Na página Detalhes, é possível exibir as Imagens ajustadas associadas a uma imagem mestre na **Construído e derivados** guia.

![imagem](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_Exibições Ajustadas para a imagem mestra ASIAN_BR_MAIN_

## Destinos de zoom

As Metas de zoom também são encontradas no **Editar** menu e **Detalhes** página de uma imagem. Eles permitem definir &quot;pontos ativos&quot; para destacar recursos específicos de merchandising de uma imagem de zoom. Em vez de criar imagens separadas ao recortar uma grande página-mestre, o visualizador de zoom pode exibir os detalhes na parte superior da imagem, juntamente com um pequeno rótulo criado por você.

![imagem](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Como os destinos de zoom são essencialmente um recurso de merchandising e exigem conhecimento dos pontos de venda de um produto, eles normalmente seriam criados por uma pessoa no Merchandising ou equipe de produto em uma empresa.

O processo é muito fácil. Clique no recurso, dê a ele um nome descritivo e salve. Os públicos alvo podem ser copiados de uma imagem para outra se forem semelhantes, no entanto, o processo é manual. No Dynamic Media Classic, não há como automatizar a criação de Destinos de zoom, pois cada imagem é diferente e tem recursos diferentes.

Outro fator ao decidir se os Destinos de zoom devem ser usados é a escolha do visualizador. Nem todos os tipos de visualizador podem exibir Destinos de zoom (por exemplo, o visualizador Fly-out não os suporta).

Saiba como [Criar Destinos de Zoom](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![imagem](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Uso da Ferramenta de direcionamento de zoom

Este é o fluxo de trabalho para criar públicos-alvo no Dynamic Media Classic.

1. Navegue até a imagem e clique no link **Editar** e escolha **Destinos de zoom**.
2. O Editor de destino de zoom será carregado. Você verá sua imagem no meio, alguns botões na parte superior e um painel alvo vazio à direita. No canto inferior esquerdo, você verá uma Predefinição do visualizador selecionada. O padrão é &quot;Guiado por Zoom1&quot;.
3. Mova a caixa vermelha com o mouse e clique para criar um novo destino.

   - A caixa vermelha é a área de destino. Quando um usuário clica nesse destino, ele amplia a área dentro da caixa.
   - O tamanho de destino é determinado pelo tamanho de exibição dentro da Predefinição do visualizador. Isso determina o tamanho da imagem de zoom principal. Consulte _Definir o tamanho da exibição_, abaixo.

4. Você verá o destino que acabou de criar ficar azul e, à direita, uma versão em miniatura desse destino, bem como o nome padrão &quot;target-0&quot;.
5. Para renomear o público alvo, clique na miniatura, digite um novo **Nome** e clique em **Enter** ou **Guia** — se você apenas clicar fora, seu nome não será salvo.
6. Enquanto o alvo estiver selecionado, a caixa terá linhas tracejadas verdes ao redor dele, e você pode redimensioná-lo e movê-lo. Arraste os cantos para redimensionar ou arraste a caixa de destino para movê-la.

   - Isso carregará a imagem dentro do visualizador de zoom personalizado padrão. Verifique se a Predefinição do visualizador é compatível com Destinos de zoom — em geral, todas as predefinições padrão com a palavra &quot;-Guiado&quot; foram projetadas para uso com Destinos de zoom. Para usar os destinos, passe o mouse sobre a miniatura de destino (ou ícone de ponto de acesso) para ver o rótulo e clique nele para ver o zoom do visualizador nesse recurso.
   - Assim como qualquer outro trabalho que você faça no Dynamic Media Classic, é necessário publicar para que os Destinos de zoom estejam online. Se você já estiver usando um visualizador compatível com destinos, eles aparecerão imediatamente (depois que o cache for limpo). No entanto, se você não estiver usando um visualizador habilitado para o Zoom Target, eles permanecerão ocultos.

     ![imagem](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Além disso, se precisar remover um target, selecione-o clicando em sua miniatura e pressione a tecla **Excluir Destino** ou pressione a tecla DELETE no teclado.
8. Continue clicando em para adicionar novos destinos, renomeando e/ou redimensionando após adicionar.
9. Quando terminar, clique no link **Salvar** e depois **Visualizar**.

### Definição do tamanho de exibição na predefinição do visualizador de zoom

Vamos falar um pouco sobre de onde vem o tamanho dos Destinos de zoom. Dentro da Predefinição do visualizador para o seu visualizador de zoom há uma configuração chamada tamanho da visualização. O tamanho da exibição é o tamanho da imagem de zoom no visualizador. É diferente do tamanho do estágio, que é o tamanho total do visualizador, incluindo os componentes da interface e o trabalho artístico.

Quando você cria um novo destino, ele deriva seu tamanho e proporção do tamanho de exibição. Por exemplo, se o tamanho da visualização for 200 x 200, você só poderá criar alvos quadrados, com uma área máxima de zoom de 200 pixels. Suas metas podem ser maiores que 200 pixels, mas sempre quadradas. Mas isso também significa que a imagem dentro do seu visualizador de zoom tem somente 200 pixels — o tamanho do destino de zoom tem uma relação direta com o tamanho do seu visualizador. Assim, você primeiro decidiria sobre o design do seu visualizador antes de definir metas.

No entanto, por padrão, o tamanho de exibição fica em branco (definido como 0 x 0), pois o tamanho da imagem de exibição principal é dinâmico e deriva-se automaticamente de acordo com o tamanho do estágio. O problema é que, se você não definir explicitamente um tamanho de exibição na sua predefinição, a ferramenta &#39;Destino de zoom&#39; não saberá que tamanho criar os destinos.

Ao carregar a ferramenta Destino de zoom, o tamanho da exibição é exibido ao lado do nome da predefinição. Compare o tamanho da exibição entre a predefinição integrada guiada por Zoom1 e a predefinição personalizada de ZT_AUTHORING.

![imagem](assets/crop-adjusted-zoom-targets/view-size.jpg)

A predefinição integrada tem um tamanho de 900 x 550, o que significa que o destino nunca pode ficar menor que esse tamanho grande. Provavelmente muito grande. Se você tiver uma imagem de 2000 pixels, você só pode chamar um recurso que tenha no mínimo 900 pixels de diâmetro. O usuário pode aplicar mais zoom manualmente, mas você não pode orientá-lo mais. Definir um tamanho de exibição para 350 x 350 permite que os alvos aproximem o zoom ou sejam redimensionados em tamanho maior. Mas se você quiser uma imagem de zoom maior no seu visualizador, precisará criar uma nova predefinição, pois a sua está bloqueada em 350 pixels.

### Criação ou edição de uma predefinição do visualizador com suporte para destinos de zoom

Para definir o tamanho de exibição, crie ou edite uma Predefinição do visualizador que ofereça suporte a Destinos de zoom.

1. Na Predefinição do visualizador, vá para a **Configurações de zoom** opção.
2. Defina uma Largura e uma Altura.
3. Salve a predefinição e feche. Se você quiser usar essa predefinição no site que está no ar, também será necessário publicá-la posteriormente.
4. Vá para a ferramenta Destino de zoom e escolha a predefinição editada no canto inferior esquerdo. Você verá imediatamente o novo tamanho de exibição refletido em seus alvos.
