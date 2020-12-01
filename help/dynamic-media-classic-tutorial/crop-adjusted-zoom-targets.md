---
title: Recorte, imagens ajustadas e Públicos alvos de zoom
description: A imagem principal do Dynamic Media Classic suporta a criação de versões cortadas separadas de cada imagem para mostrar detalhes ou para amostras sem precisar criar versões cortadas separadas de cada imagem. Saiba como recortar imagens no Dynamic Media Classic e salvá-las como um novo arquivo principal ou uma imagem virtual, salvar imagens ajustadas virtuais e usá-las no lugar de ativos principais e criar Públicos alvos de zoom em suas imagens para mostrar detalhes destacados.
sub-product: dynamic-media
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '2665'
ht-degree: 0%

---


# Recorte, imagens ajustadas e Públicos alvos de zoom {#crop-adjusted-zoom-targets}

Uma das principais forças do conceito de imagem principal do Dynamic Media Classic é que você pode reaproveitar o ativo de imagem para muitos usos. Tradicionalmente, você teria que criar versões separadas de cada imagem para mostrar detalhes ou para amostras. Ao usar o Dynamic Media Classic, você pode fazer as mesmas tarefas em seu único principal e salvar essas versões cortadas como novos arquivos físicos ou como derivados virtuais que não ocupam espaço no armazenamento.

Ao final desta seção do tutorial, você saberá como:

- Recorte imagens no Dynamic Media Classic e salve como novos arquivos principais ou como imagens virtuais. [Saiba mais](https://docs.adobe.com/help/en/dynamic-media-classic/using/master-files/cropping-image.html).
- Salve imagens ajustadas virtuais e use-as no lugar de ativos principais. [Saiba mais](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/adjusting-image.html).
- Crie Públicos alvos de zoom em suas imagens para exibir seus destaques. [Saiba mais](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Cortar

O Dynamic Media Classic tem algumas ferramentas de edição de imagens convenientemente disponíveis na interface do usuário, incluindo a ferramenta Recortar. Você pode querer recortar sua imagem principal no Dynamic Media Classic por vários motivos. Por exemplo:

- Você não tem acesso ao arquivo original. Você deseja exibir a imagem com um corte ou proporção diferente, mas não tem o arquivo original no computador ou está trabalhando em casa. Nesse caso, você pode acessar o Dynamic Media Classic, encontrar a imagem, recortá-la e salvá-la ou salvá-la como uma nova versão.
- Para remover o excesso de espaço em branco. A imagem foi fotografada com muito espaço em branco, o que faz o produto parecer pequeno. Você deseja que suas imagens em miniatura preencham a tela o máximo possível.
- Para criar imagens ajustadas, as cópias virtuais de imagens que não ocupam espaço em disco. Algumas empresas têm regras comerciais que exigem que elas mantenham cópias separadas da mesma imagem, mas com um nome diferente. Ou talvez você queira uma versão recortada e não recortada da mesma imagem.
- Para criar novas imagens a partir de uma imagem de origem. Por exemplo, você pode querer criar amostras de cores ou um detalhe da imagem principal. Você pode fazer isso no Adobe Photoshop e fazer upload separadamente ou usar a ferramenta Recortar no Dynamic Media Classic.

>[!NOTE]
>
>Todos os URLs nas discussões a seguir de Recorte são apenas para fins ilustrativos; não são ligações ao vivo.

### Uso da ferramenta Corte demarcado

Você pode acessar a ferramenta Recortar no Dynamic Media Classic na página Detalhes de um ativo ou clicando no botão **Editar**. Você pode usar a ferramenta para cortar de duas formas:

- O modo de corte padrão no qual você arrasta as alças da janela de corte ou digita valores na caixa Tamanho. Saiba como [Recortar manualmente](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Aparar. Use essa opção para remover espaços em branco extras ao redor da imagem, calculando o número de pixels que não correspondem à imagem. Saiba como [Recortar ao aparar](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Corte manual_

Quando você salva uma versão cortada manualmente, parece que a imagem está cortada permanentemente; O Dynamic Media Classic está ocultando os pixels ao adicionar um modificador de URL interno para cortar a imagem. Ao publicar, todos verão que a imagem está cortada, no entanto, você pode retornar ao Editor de corte e remover o corte posteriormente.

Você pode optar por salvar como uma Nova imagem Principal ou como uma Visualização adicional do principal. Um novo principal é um novo arquivo físico (como TIFF ou JPEG) que ocupa espaço no armazenamento. Uma visualização adicional é uma imagem virtual que não ocupa nenhum espaço no servidor. Não recomendamos que você escolha Substituir original, pois isso substituirá seu principal e tornará a colheita permanente. Se você salvar como uma nova principal ou como uma visualização adicional, deverá escolher uma nova ID de ativo. Como outras IDs de ativos, esse nome deve ser exclusivo no Dynamic Media Classic.

### _Cortar recorte_

Se você carregar uma imagem com muito espaço em branco (tela extra) em torno do assunto principal da imagem, ela parecerá muito menor na Web quando redimensionada. Isto é especialmente verdadeiro para imagens em miniatura com 150 pixels ou menos. o assunto da foto pode se perder em todo o espaço extra ao seu redor.

Compare essas duas versões da mesma imagem.

![imagem](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

A imagem à direita é muito mais destacada ao remover o espaço extra ao redor do produto. A apara pode ser feita uma imagem por vez, usando a ferramenta Recortar, ou pode ser executada como um processo em lote durante o upload. Recomendamos executar como um processo em lote se desejar que todas as imagens sejam cortadas de forma consistente nos limites do assunto principal. Aparar colheitas à caixa delimitadora. o retângulo ao redor da imagem.

>[!NOTE]
>
>Aparar não cria transparência em torno da imagem. Para isso, seria necessário incorporar um caminho de recorte na imagem e usar a opção de carregamento **Criar máscara do caminho do clipe**.
>
>Além disso, para restaurar uma imagem para seu estado original depois de cortá-la quando você tiver usado a opção **Salvar**, exiba a imagem na tela do Editor de corte e selecione o botão **Redefinir**.

### _Recortar no upload_

Como mencionado anteriormente, você também pode optar por recortar as imagens à medida que as carrega. Para usar recorte de aparagem no upload, clique no botão **Opções de trabalho** e, em Opções de corte, escolha **Aparar**.

O Dynamic Media Classic lembrará essa opção para o próximo upload. Embora você queira que ele recorte imagens para este upload, talvez você não queira que elas sejam cortadas para cada upload. Outra opção seria definir um trabalho de upload FTP programado especial e colocar as opções de recorte lá. Dessa forma, você só executaria o trabalho quando precisasse cortar suas imagens.

>[!IMPORTANT]
>
>Se você definir um recorte para o upload, o Dynamic Media Classic colocará um cookie para lembrar essa configuração da próxima vez. Como prática recomendada, clique no botão **Redefinir para padrões de Empresa** antes do próximo upload para limpar quaisquer opções de recorte restantes do último upload; caso contrário, você pode recortar acidentalmente o próximo lote de imagens.

### Recortar por URL

Embora não seja óbvio no Dynamic Media Classic, também é possível cortar apenas pelo URL (ou até mesmo adicionar recorte a uma predefinição de imagem).

Sempre que usar a ferramenta Recortar, você verá valores de URL no campo na parte inferior. Você pode usar esses valores e aplicá-los diretamente a uma imagem como modificadores de URL.

![modificadores de comando ](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_imageCrop na parte inferior do Editor de corte_

![imagem](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Como o tamanho tem de ser calculado com base em imagem por imagem quando você usa recorte por apara, ele não pode ser automatizado pelo URL. O corte de aparas só pode ser executado no upload ou aplicando uma imagem por vez.

### _Recortar na predefinição de imagem_

As predefinições de imagens têm um campo onde você pode adicionar comandos adicionais de disponibilização de imagens. Para adicionar o mesmo recorte acima à predefinição de imagem, edite a predefinição e cole ou digite os valores no campo Modificadores de URL e salve e publique.

![](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_imageAdicione comandos de corte (ou qualquer comando) aos Modificadores de URL da Predefinição de imagem._

O corte fará parte dessa Predefinição de imagem e será aplicado automaticamente toda vez que for usado. Claro, esse método depende de todas as imagens que precisam da mesma quantidade de colheita. Se suas imagens não forem todas filmadas da mesma maneira, este método não funcionará para você.

## Imagens ajustadas

Ao usar a ferramenta Recortar, você tem a opção de **Salvar como Visualização adicional de Principal**. Quando salvo, isso cria um novo tipo de ativo do Dynamic Media Classic. uma imagem ajustada. Uma imagem ajustada, também chamada de derivada, é uma imagem virtual. Na verdade não é uma imagem. é uma referência de banco de dados (como um alias ou atalho) para a imagem principal física.

### A imagem real está posicionada`?`

Você pode dizer qual é a principal, e qual é a imagem ajustada?

![imagem](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

Não é possível identificar sem procurar no Dynamic Media Classic e ver o tipo de ativo &quot;Imagem ajustada&quot; para SBR_MAIN2.

Uma Imagem Ajustada não usa espaço em disco, pois só existe como um item de linha no banco de dados. Está também permanentemente ligada ao ativo original; se o original for excluído, a Imagem ajustada também será excluída. Pode consistir em uma imagem inteira e não cortada ou apenas em parte de uma imagem (um corte).

![imagem](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Geralmente, é possível criar imagens ajustadas com a ferramenta Recortar; no entanto, também podem ser criados com outros editores de imagem. as ferramentas Ajustar e aumentar a nitidez.

Imagens ajustadas exigem uma ID de ativo exclusiva. Quando publicados (você deve publicar como qualquer outro ativo), eles atuam como qualquer outra imagem e são chamados em um URL pela ID do ativo. Na página Detalhes, é possível visualização de Imagens ajustadas associadas a uma imagem principal na guia **Criar e derivados**.

![visualizações ](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_imageAjustadas para imagem principal ASIAN_BR_MAIN_

## Públicos alvos de zoom

Públicos alvos de zoom também são encontrados no menu **Editar** e na página **Detalhes** de uma imagem. Eles permitem que você defina &quot;pontos de acesso&quot; para realçar recursos de comercialização específicos de uma imagem de zoom. Em vez de criar imagens separadas ao recortar uma principal grande, o visualizador de zoom pode servir aos detalhes na parte superior da imagem, juntamente com um pequeno rótulo que você cria.

![imagem](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Como os Públicos alvos de zoom são essencialmente um recurso de merchandising e exigem conhecimento dos pontos de venda de um produto, eles normalmente seriam criados por uma pessoa na equipe de merchandising ou Produto de uma empresa.

O processo é muito fácil. clique no recurso, dê um nome descritivo e salve-o. Públicos alvos podem ser copiados de uma imagem para outra se forem semelhantes, no entanto, o processo é manual. Não há como no Dynamic Media Classic automatizar a criação de Públicos alvos de zoom, pois cada imagem é diferente e tem recursos diferentes.

Outro fator para decidir se deseja usar Públicos alvos de zoom é a sua escolha do visualizador. Nem todos os tipos de visualizador podem exibir Públicos alvos de zoom (por exemplo, o visualizador de fallout não oferece suporte para eles).

Saiba como [Criar Públicos alvos de zoom](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![imagem](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Uso da ferramenta Público alvo de zoom

Este é o fluxo de trabalho para criar públicos alvos no Dynamic Media Classic.

1. Navegue até a imagem, clique no botão **Editar** e escolha **Públicos alvos de zoom**.
2. O Editor de Públicos alvos de zoom será carregado. Você verá sua imagem no meio, alguns botões no topo e um painel público alvo vazio à direita. Na parte inferior esquerda, você verá uma predefinição do visualizador selecionada. O padrão é &quot;Zoom1-Guiado&quot;.
3. Mova a caixa vermelha com o mouse e clique para criar um novo público alvo.

   - A caixa vermelha é a área do público alvo. Quando um usuário clica nesse público alvo, o zoom é aplicado na área dentro da caixa.
   - O tamanho do público alvo é determinado pelo tamanho da visualização dentro da predefinição do visualizador. Isso determina o tamanho da imagem de zoom principal. Consulte _Definição do tamanho da Visualização_, abaixo.

4. Você verá o público alvo que acabou de criar ficar azul, e à direita você verá uma versão em miniatura desse público alvo, bem como o nome padrão &quot;público alvo 0&quot;.
5. Para renomear seu público alvo, clique em sua miniatura, digite um novo **Nome** e clique em **Enter** ou **Tab** — se você clicar fora, seu nome não será salvo.
6. Enquanto o público alvo é selecionado, a caixa tem linhas tracejadas verdes ao seu redor, e você pode redimensioná-la e movê-la. Arraste os cantos para redimensionar ou arraste a caixa de público alvo para movê-la.

   - Isso carregará a imagem dentro do visualizador de zoom personalizado padrão. Certifique-se de que a predefinição do visualizador suporta Públicos alvos de zoom — em geral, todas as predefinições padrão que têm a palavra &quot;guiado&quot; foram projetadas para uso com Públicos alvos de zoom. Para usar os públicos alvos, passe o mouse sobre a miniatura do público alvo (ou ícone de ponto de acesso) para ver o rótulo e clique nele para ver o zoom do visualizador no recurso.
   - Assim como todos os outros trabalhos do Dynamic Media Classic, você deve publicar para que seus Públicos alvos de zoom sejam colocados online. Se você já estiver usando um visualizador compatível com públicos alvos, eles serão exibidos imediatamente (depois que o cache for limpo). Entretanto, se você não estiver usando um visualizador habilitado para Público alvo de zoom, eles permanecerão ocultos.

      ![imagem](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Além disso, se precisar remover um público alvo, selecione-o clicando em sua miniatura e pressione o botão **Excluir Público alvo** ou pressione a tecla DELETE no teclado.
8. Continue clicando para adicionar novos públicos alvos, renomeando e/ou redimensionando após a adição.
9. Quando terminar, clique no botão **Salvar** e, em seguida, **Pré-visualização**.

### Configuração do tamanho da Visualização na predefinição do visualizador de zoom

Vamos falar um momento sobre de onde vem o tamanho dos Públicos alvos de Zoom. Dentro da predefinição do visualizador para o visualizador de zoom há uma configuração chamada tamanho da visualização. O tamanho da visualização é o tamanho da imagem de zoom no visualizador. É diferente do tamanho do palco, que é o tamanho total do seu visualizador, incluindo os componentes da interface e o trabalho artístico.

Quando você cria um novo público alvo, ele deriva seu tamanho e sua proporção do tamanho da visualização. Por exemplo, se o tamanho da sua visualização for 200 x 200, você só poderá fazer públicos alvos quadrados, com uma área máxima de zoom de 200 pixels. Seus públicos alvos podem ter mais de 200 pixels, mas sempre quadrados. Mas isso também significa que a imagem dentro do seu visualizador de zoom é de apenas 200 pixels. o tamanho do público alvo de zoom tem uma relação direta com o tamanho do visualizador. Assim, você deve decidir primeiro sobre o design do seu visualizador antes de definir públicos alvos.

Entretanto, por padrão, o tamanho da visualização fica em branco (definido como 0 x 0), pois o tamanho da imagem da visualização principal é dinâmico e é automaticamente derivado de acordo com o tamanho do palco. O problema é que se você não definir explicitamente um tamanho de visualização na sua predefinição, a ferramenta Zoom Público alvo não saberá qual tamanho fazer os públicos alvos.

Quando a ferramenta Público alvo de zoom é carregada, o tamanho da visualização é exibido ao lado do nome da predefinição. Compare o tamanho da visualização entre a predefinição Zoom1 guiada integrada e a predefinição ZT_AUTHORING personalizada.

![imagem](assets/crop-adjusted-zoom-targets/view-size.jpg)

Você pode ver que a predefinição integrada tem um tamanho de 900 x 550, o que significa que o público alvo nunca pode ficar menor que aquele tamanho muito grande. Isso é provavelmente muito grande. se você tiver uma imagem de 2000 pixels, poderá chamar apenas um recurso que tenha no mínimo 900 pixels de diâmetro. O usuário pode aplicar mais zoom manualmente, mas não é possível orientá-los mais de perto. Definir um tamanho de visualização como 350 x 350 permite que os públicos alvos aumentem o zoom de perto ou sejam redimensionados para maiores dimensões. Mas se quiser uma imagem maior de zoom no visualizador, é necessário criar uma nova predefinição porque a sua está bloqueada em 350 pixels.

### Criar ou editar uma predefinição do visualizador que suporte Públicos alvos de zoom

Para definir o tamanho da visualização, crie ou edite uma predefinição do visualizador compatível com Públicos alvos de zoom.

1. Na predefinição do visualizador, vá para a opção **Configurações de zoom**.
2. Defina Largura e Altura.
3. Salve a predefinição e feche-a. Se você quiser usar essa predefinição em seu site ativo, será necessário publicar também mais tarde.
4. Vá para a ferramenta Público alvo de zoom e escolha a predefinição editada na parte inferior esquerda. Você verá imediatamente o novo tamanho da visualização refletido em seus públicos alvos.
