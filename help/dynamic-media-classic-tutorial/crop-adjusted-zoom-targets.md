---
title: Recortar, imagens ajustadas e metas de zoom
description: A imagem mestre do Dynamic Media Classic oferece suporte à criação de versões cortadas separadas de cada imagem para mostrar detalhes ou para amostras sem precisar criar versões cortadas separadas de cada imagem. Saiba como recortar imagens no Dynamic Media Classic e salvá-las como um novo arquivo mestre ou uma imagem virtual, salvar imagens ajustadas virtuais e usá-las no lugar de ativos mestre e criar Metas de zoom em suas imagens para exibir detalhes destacados.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2673'
ht-degree: 0%

---


# Recortar, imagens ajustadas e metas de zoom {#crop-adjusted-zoom-targets}

Um dos principais pontos fortes do conceito principal de imagem do Dynamic Media Classic é que você pode redefinir a finalidade do ativo de imagem para muitos usos. Tradicionalmente, você teria que criar versões separadas de cada imagem para mostrar detalhes ou para amostras. Ao usar o Dynamic Media Classic, você pode realizar as mesmas tarefas no seu único mestre e salvar essas versões cortadas como novos arquivos físicos ou como derivados virtuais que não ocupam espaço de armazenamento.

Ao final desta seção do tutorial, você saberá:

- Recorte imagens no Dynamic Media Classic e salve como novos arquivos mestre ou como imagens virtuais. [Saiba mais](https://docs.adobe.com/help/en/dynamic-media-classic/using/master-files/cropping-image.html).
- Salve imagens ajustadas virtuais e use-as no lugar de ativos mestre. [Saiba mais](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/adjusting-image.html).
- Crie metas de zoom em suas imagens para exibir seus destaques. [Saiba mais](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Cortar

O Dynamic Media Classic tem algumas ferramentas de edição de imagens convenientemente disponíveis na interface do usuário, incluindo a ferramenta Recortar. Talvez você queira recortar a imagem principal no Dynamic Media Classic por vários motivos. Por exemplo:

- Você não tem acesso ao arquivo original. Você deseja exibir a imagem com um corte ou proporção diferente, mas não tem o arquivo original no computador ou está trabalhando em casa. Nesse caso, você pode acessar o Dynamic Media Classic, encontrar a imagem, recortá-la e salvá-la, ou salvá-la como uma nova versão.
- Para remover o excesso de espaço em branco. A imagem foi fotografada com muito espaço em branco, o que faz o produto parecer pequeno. Você deseja que suas imagens em miniatura preencham a tela o máximo possível.
- Para criar imagens ajustadas, cópias virtuais de imagens que não ocupam espaço em disco. Algumas empresas têm regras de negócios que exigem que elas mantenham cópias separadas da mesma imagem, mas com um nome diferente. Ou talvez você queira uma versão cortada e não cortada da mesma imagem.
- Para criar novas imagens a partir de uma imagem de origem. Por exemplo, talvez você queira criar amostras de cores ou um detalhe da imagem principal. Você pode fazer isso no Adobe Photoshop e fazer upload separadamente ou usar a ferramenta Corte no Dynamic Media Classic.

>[!NOTE]
>
>Todos os URLs nas discussões de Corte a seguir são apenas para fins ilustrativos; eles não são links ao vivo.

### Uso da ferramenta Corte demarcado

Você pode acessar a ferramenta Recortar no Dynamic Media Classic na página Detalhes de um ativo ou clicando no botão **Editar**. Você pode usar a ferramenta para cortar de duas formas:

- O modo de corte padrão no qual você arrasta as alças da janela de corte ou digita valores na caixa Tamanho. Saiba como [Recortar manualmente](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Aparar. Use isso para remover espaços em branco extra ao redor da imagem, calculando o número de pixels que não correspondem à imagem. Saiba como [Recortar ao Aparar](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Corte manual_

Quando você salva uma versão cortada manualmente, a imagem é exibida como permanentemente cortada; O Dynamic Media Classic na verdade está ocultando os pixels adicionando um modificador de URL interno para cortar a imagem. Ao publicar, todos verão que a imagem está cortada. No entanto, é possível retornar ao Editor de corte e remover o corte posteriormente.

Em seguida, você pode escolher se deseja salvar como uma Nova Imagem Mestre ou como uma Exibição Adicional do Mestre. Um novo mestre é um novo arquivo físico (como um TIFF ou JPEG) que ocupa espaço de armazenamento. Uma exibição adicional é uma imagem virtual que não ocupa espaço no servidor. Não recomendamos que você escolha Substituir original, pois isso substituirá o mestre e tornará o corte permanente. Se você salvar como um novo mestre ou como uma visualização adicional, deverá escolher uma nova ID de ativo. Como outras IDs de ativo, esse deve ser um nome exclusivo no Dynamic Media Classic.

### _Corte_

Se você carregar uma imagem com muito espaço em branco (tela extra) ao redor do assunto principal, ela ficará muito menor na Web quando for redimensionada. Isso é especialmente verdadeiro para imagens em miniatura com 150 pixels ou menos — o assunto da foto pode se perder em todo o espaço extra ao redor.

Compare essas duas versões da mesma imagem.

![imagem](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

A imagem à direita é muito mais destacada pela remoção do espaço extra ao redor do produto. O corte pode ser feito uma imagem de cada vez, usando a ferramenta Corte demarcado, ou ser executado como um processo em lote durante o upload. Recomendamos executar como um processo em lote se você quiser que todas as imagens sejam cortadas consistentemente para os limites do assunto principal. Cortar recorta para a caixa delimitadora — o retângulo ao redor da imagem.

>[!NOTE]
>
>Cortar não cria transparência ao redor da imagem. Para isso, você precisaria incorporar um traçado de recorte na imagem e usar a opção de upload **Criar máscara a partir do caminho do clipe** .
>
>Além disso, para restaurar uma imagem ao seu estado original depois de cortá-la quando tiver usado a opção **Salvar**, exiba a imagem na tela Editor de corte e selecione o botão **Redefinir**.

### _Recortar no Upload_

Como mencionado anteriormente, também é possível optar por recortar as imagens conforme você as carrega. Para usar o corte de aparas no upload, clique no botão **Opções de trabalho** e, em Opções de recorte, escolha **Cortar**.

O Dynamic Media Classic se lembrará dessa opção para o próximo upload. Embora você queira que ele corte imagens para este upload, talvez não queira que elas sejam cortadas para cada upload. Outra opção seria definir um trabalho de upload FTP agendado especial e colocar as opções de corte lá. Dessa forma, você só executaria o trabalho quando precisava recortar suas imagens.

>[!IMPORTANT]
>
>Se você definir um recorte para seu upload, o Dynamic Media Classic colocará um cookie para lembrar dessa configuração na próxima vez. Como prática recomendada, clique no botão **Reset to Company Defaults** antes do próximo upload para limpar as opções de corte restantes do último upload; caso contrário, você poderá cortar acidentalmente o próximo lote de imagens.

### Recortar por URL

Embora não seja óbvio no Dynamic Media Classic, você também pode recortar apenas pelo URL (ou até mesmo adicionar recorte a uma predefinição de imagem).

Sempre que usar a ferramenta Corte demarcado, você verá valores de URL no campo na parte inferior. Você pode pegar esses valores e aplicá-los diretamente a uma imagem como modificadores de URL.

![](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_modificadores de comando imageCrop na parte inferior do Editor de corte_

![imagem](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Como o tamanho deve ser calculado com base na imagem ao usar o corte por corte, ele não pode ser automatizado pelo URL. O corte pode ser executado somente no upload ou ao aplicá-lo a uma imagem de cada vez.

### _Recortar na predefinição de imagem_

As predefinições de imagem têm um campo onde você pode adicionar comandos adicionais de Exibição de imagem. Para adicionar o mesmo corte acima à sua Predefinição de imagem, edite sua predefinição e cole ou digite os valores no campo Modificadores de URL e, em seguida, salve e publique.

![](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_imageAdicione comandos de corte (ou qualquer comando) aos modificadores de URL da predefinição de imagem._

O corte agora fará parte dessa Predefinição de imagem e será aplicado automaticamente toda vez que for usado. É claro que esse método depende de todas as imagens que precisam da mesma quantidade de corte. Se suas imagens não fossem todas filmadas da mesma maneira, esse método não funcionaria para você.

## Imagens ajustadas

Ao usar a ferramenta Cortar, você tem a opção de **Salvar como Exibição Adicional do Mestre**. Quando salvo, isso cria um novo tipo de ativo do Dynamic Media Classic — uma imagem ajustada. Uma Imagem Ajustada, também chamada de derivada, é uma imagem virtual. Na verdade não é uma imagem. é uma referência de banco de dados (como um alias ou atalho) para a imagem principal física.

### A imagem real ficará ativa`?`?

Você sabe qual é o mestre e qual é a imagem ajustada?

![imagem](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

Não é possível informar sem procurar no Dynamic Media Classic e ver o tipo de ativo de &quot;Imagem ajustada&quot; para SBR_MAIN2.

Uma Imagem Ajustada não usa espaço em disco, pois só existe como um item de linha no banco de dados. Está também permanentemente vinculado ao ativo original; se o original for excluído, a Imagem ajustada também será excluída. Pode consistir em uma imagem inteira e não cortada ou apenas parte de uma imagem (um recorte).

![imagem](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Normalmente, você cria Imagens ajustadas com a ferramenta Corte demarcado; no entanto, eles também podem ser criados com os outros editores de imagem — as ferramentas Ajustar e Nitidez.

Imagens ajustadas exigem uma ID de ativo exclusiva. Quando publicados (você deve publicar como qualquer outro ativo), eles atuam como qualquer outra imagem e são chamados em um URL pela ID do ativo. Na página Detalhes, você pode exibir Imagens ajustadas associadas a uma imagem mestre na guia **Criar e derivados**.

![](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_imageAjusted Views para a imagem mestre ASIAN_BR_MAIN_

## Metas de zoom

Metas de zoom também são encontradas no menu **Editar** e na página **Detalhes** de uma imagem. Eles permitem definir &quot;pontos de acesso&quot; para destacar recursos de merchandising específicos de uma imagem de zoom. Em vez de criar imagens separadas ao recortar um mestre grande, o visualizador de zoom pode disponibilizar os detalhes sobre a imagem, juntamente com um pequeno rótulo que você cria.

![imagem](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Como as metas de zoom são essencialmente um recurso de merchandising e exigem conhecimento dos pontos de venda de um produto, elas normalmente são criadas por uma pessoa na equipe de merchandising ou produto de uma empresa.

O processo é muito fácil — clique no recurso, dê um nome descritivo e salve. Os direcionamentos podem ser copiados de uma imagem para outra se forem semelhantes, no entanto, o processo é manual. Não há como no Dynamic Media Classic automatizar a criação de metas de zoom, pois cada imagem é diferente e tem recursos diferentes.

Outro fator para decidir se usará as Metas de zoom é a escolha do visualizador. Nem todos os tipos de visualizador podem exibir Metas de zoom (por exemplo, o visualizador de Fly-out não oferece suporte a elas).

Saiba como [Criar metas de zoom](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![imagem](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Usar a ferramenta de direcionamento de zoom

Este é o fluxo de trabalho para criar destinos no Dynamic Media Classic.

1. Navegue até a imagem, clique no botão **Edit** e escolha **Zoom Targets**.
2. O Editor de direcionamento de zoom será carregado. Você verá sua imagem no meio, alguns botões na parte superior e um painel de destino vazio à direita. Na parte inferior esquerda, você verá uma Predefinição do visualizador selecionada. O padrão é &quot;Zoom1-Guiado&quot;.
3. Mova a caixa vermelha com o mouse e clique em para criar um novo destino.

   - A caixa vermelha é a área de destino. Quando um usuário clica nesse destino, o zoom é ampliado para a área dentro da caixa.
   - O tamanho da meta é determinado pelo tamanho da exibição dentro da Predefinição do visualizador. Isso determina o tamanho da imagem de zoom principal. Consulte _Definindo o Tamanho de Exibição_, abaixo.

4. Você verá a meta que acabou de criar ficar azul, e à direita você verá uma versão em miniatura dessa meta, bem como o nome padrão &quot;target-0&quot;.
5. Para renomear seu destino, clique em sua miniatura, digite um novo **Nome** e clique em **Enter** ou **Tab** — se você clicar novamente, seu nome não será salvo.
6. Enquanto o público-alvo é selecionado, a caixa tem linhas tracejadas verdes ao seu redor e você pode redimensioná-lo e movê-lo. Arraste os cantos para redimensionar ou arraste a caixa de destino para movê-la.

   - Isso carregará a imagem dentro do visualizador de zoom personalizado padrão. Certifique-se de que a predefinição do visualizador seja compatível com as metas de zoom — em geral, todas as predefinições padrão que têm a palavra &quot;guiado&quot; foram projetadas para uso com as metas de zoom. Para usar os destinos, passe o mouse sobre a miniatura do target (ou ícone de ponto de acesso) para ver o rótulo e clique nele para ver o zoom do visualizador no recurso.
   - Assim como qualquer outro trabalho no Dynamic Media Classic, você deve publicar para que suas metas de zoom estejam ativas na Web. Se você já estiver usando um visualizador compatível com destinos, eles serão exibidos imediatamente (após a limpeza do cache). No entanto, se você não estiver usando um visualizador habilitado para Zoom do Target, ele permanecerá oculto.

      ![imagem](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Além disso, se precisar remover um destino, selecione-o clicando em sua miniatura e pressione o botão **Delete Target** ou pressione a tecla DELETE no teclado.
8. Continue clicando em para adicionar novos destinos, renomeando e/ou redimensionando após a adição.
9. Quando terminar, clique no botão **Save** e, em seguida, em **Preview**.

### Configuração do tamanho de exibição na predefinição do visualizador de zoom

Vamos falar um pouco sobre de onde vem o tamanho das metas de zoom. Dentro da Predefinição do visualizador do seu visualizador de zoom, há uma configuração chamada tamanho da visualização. Tamanho da visualização é o tamanho da imagem de zoom no visualizador. É diferente do tamanho do palco, que é o tamanho total do visualizador, incluindo os componentes da interface do usuário e o trabalho artístico.

Quando você cria um novo target, ele deriva seu tamanho e proporção do tamanho da exibição. Por exemplo, se o tamanho da sua exibição for 200 x 200, você só poderá fazer alvos quadrados, com uma área de zoom máxima de 200 pixels. Seus alvos podem ser maiores que 200 pixels, mas sempre quadrados. Mas isso também significa que a imagem dentro do visualizador de zoom tem apenas 200 pixels — o tamanho do alvo de zoom tem uma relação direta com o tamanho do visualizador. Assim, você decide primeiro sobre o design do visualizador antes de definir metas.

No entanto, por padrão, o tamanho da exibição fica em branco (definido como 0 x 0), pois o tamanho da imagem da exibição principal é dinâmico e é automaticamente derivado de acordo com o tamanho do palco. O problema é que, se você não definir explicitamente um tamanho de exibição na sua predefinição, a ferramenta de Direcionamento de zoom não saberá qual tamanho fazer os destinos.

Quando você carrega a ferramenta Zoom do Target, o tamanho da exibição é exibido ao lado do nome da predefinição. Compare o tamanho da exibição entre a predefinição Zoom1 guiada integrada e a predefinição ZT_AUTHORING personalizada.

![imagem](assets/crop-adjusted-zoom-targets/view-size.jpg)

Você pode ver que a predefinição incorporada tem um tamanho de 900 x 550, o que significa que o target nunca pode ficar menor do que esse tamanho muito grande. Isso provavelmente é muito grande — se você tem uma imagem de 2000 pixels, você só pode chamar um recurso com no mínimo 900 pixels de diâmetro. O usuário pode aumentar o zoom manualmente, mas você não pode orientá-los mais de perto. Definir um tamanho de exibição para 350 x 350 permite que os destinos ampliem de perto ou sejam redimensionados maiores. Mas se você quiser uma imagem de zoom maior em seu visualizador, precisará criar uma nova predefinição porque a sua está bloqueada em 350 pixels.

### Criação ou edição de uma predefinição do visualizador compatível com metas de zoom

Para definir o tamanho da exibição, crie ou edite uma Predefinição do visualizador compatível com Metas de zoom.

1. Na Predefinição do visualizador, vá para a opção **Configurações de zoom**.
2. Defina Largura e Altura.
3. Salve a predefinição e feche. Se você quiser usar essa predefinição em seu site ativo, será necessário publicar também posteriormente.
4. Acesse a ferramenta Zoom Target e escolha a predefinição editada na parte inferior esquerda. Você verá imediatamente o novo tamanho de exibição refletido em seus destinos.
