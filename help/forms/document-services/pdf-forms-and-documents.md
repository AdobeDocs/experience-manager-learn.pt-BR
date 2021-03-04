---
title: Entender os diferentes tipos de formulários e documentos PDF
description: PDF é na verdade uma família de formatos de arquivo, e este artigo descreve os tipos de PDFs que são importantes e relevantes para desenvolvedores de formulários.
solution: Experience Manager Forms
product: aem
type: Documentação
role: Desenvolvedor
level: Iniciante,Intermediário
version: 6.3,6.4,6.5
feature: Serviços de documento
topic: Desenvolvimento
kt: 7071
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---


# Formatos PDF

O Portable Document Format (PDF) é, na verdade, uma família de formatos de arquivo, e este artigo detalha os mais relevantes para desenvolvedores de formulários. Muitos dos detalhes técnicos e padrões de diferentes tipos de PDF estão evoluindo e mudando. Alguns desses formatos e especificações são normas da Organização Internacional de Normalização (ISO) e outros são propriedade intelectual específica da Adobe.

Este artigo mostra como criar vários tipos de PDFs. Isso ajudará você a entender como e por que usar cada um. Todos esses tipos funcionam melhor na principal ferramenta de cliente para visualizar e trabalhar com PDFs — Adobe Acrobat DC.

Este é um exemplo de arquivo PDF/A no Acrobat DC.
![pdfa](assets/pdfa-file-in-acrobat.png)

Arquivos de exemplo podem ser [baixados aqui](assets/pdf-file-types.zip)

## PDF XFA

A Adobe usa o termo formulário PDF para se referir aos formulários interativos e dinâmicos criados com o AEM Forms Designer. É importante observar que há outro tipo de formulário PDF, chamado Acroform, que é diferente dos formulários PDF criados no AEM Forms Designer. Os formulários e arquivos criados com o Designer são baseados na Arquitetura de formulários XML (XFA) da Adobe. De muitas formas, o formato de arquivo PDF XFA está mais próximo de um arquivo HTML do que de um arquivo PDF tradicional. Por exemplo, o código a seguir mostra a aparência de um objeto de texto simples em um arquivo PDF XFA.

![campo de texto](assets/text-field.JPG)

Como você pode ver, os formulários XFA são baseados em XML. Esse formato bem estruturado e flexível permite que um servidor AEM Forms transforme seus arquivos Designer em diferentes formatos, incluindo PDF tradicional, PDF/A e HTML. Você pode ver a estrutura XML completa de seus formulários no Designer, selecionando a guia Origem XML no Editor de layout. Você pode criar formulários XFA estáticos e dinâmicos no AEM Forms Designer.

## PDF estático

Os formulários PDF XFA estáticos não alteram seu layout no tempo de execução, mas podem ser interativos para o usuário. Veja a seguir algumas vantagens de formulários PDF XFA estáticos:

* Os formulários PDF XFA estáticos não alteram seu layout no tempo de execução, mas podem ser interativos para o usuário.
* Os formulários estáticos são compatíveis com as ferramentas Comentário e Marcação do Acrobat.
* Os formulários estáticos permitem importar e exportar comentários do Acrobat.
* Os formulários estáticos suportam a subconfiguração de fonte, que é uma técnica que pode ser feita em um servidor do AEM Forms.
* Os formulários estáticos podem ser renderizados usando o visualizador de pdf integrado que vem com os navegadores modernos.

>[!NOTE]
> Você pode criar pdfs estáticos usando o AEM Forms Designer, salvando o XDP como Formulário PDF estático da Adobe

## Formulários dinâmicos

Os PDFs XFA dinâmicos podem alterar o layout no tempo de execução, de modo que os recursos de comentário e marcação não são suportados. No entanto, os PDFs XFA dinâmicos oferecem as seguintes vantagens:

* Os formulários dinâmicos são compatíveis com scripts de cliente que alteram o layout e a paginação do formulário. Por exemplo, o Purchase Order.xdp será expandido e paginado para acomodar uma quantidade infinita de dados se você salvá-lo como um formulário dinâmico
* Os formulários dinâmicos oferecem suporte a todas as propriedades do formulário no tempo de execução, enquanto os formulários estáticos oferecem suporte somente a um subconjunto


>[!NOTE]
> Você pode criar pdfs dinâmicos usando o AEM Forms Designer salvando o XDP como Formulário XML dinâmico Adobe

>[!NOTE]
> Os formulários dinâmicos não podem ser renderizados usando os visualizadores pdf incorporados dos navegadores modernos.


## Arquivo PDF (PDF tradicional)

O formato PDF mais popular e abrangente é o arquivo PDF tradicional. Há muitas maneiras de criar um arquivo PDF tradicional, incluindo o uso do Acrobat e de muitas ferramentas de terceiros. O Acrobat fornece todas as seguintes maneiras de criar arquivos PDF tradicionais. Se você não tiver o Acrobat instalado, talvez não veja essas opções no computador.

* Capturando o fluxo de impressão de um aplicativo de desktop: Escolha o comando Imprimir de um aplicativo de criação e selecione o ícone da impressora Adobe PDF. Em vez de uma cópia impressa do seu documento, você terá criado um arquivo PDF do seu documento
* Usando o plug-in Acrobat PDFMaker com aplicativos do Microsoft Office: Ao instalar o Acrobat, ele adiciona um menu Adobe PDF aos aplicativos do Microsoft Office e um ícone à faixa do Office. Você pode usar esses recursos adicionados para criar arquivos PDF diretamente no Microsoft Office
* Ao usar o Acrobat Distiller para converter arquivos Postscript e Encapsulated Postscript (EPS) em PDFs: O Distiller é normalmente usado na publicação de impressão e em outros fluxos de trabalho que exigem uma conversão do formato Postscript para o formato PDF
* Sob o capô, um PDF tradicional é muito diferente de um PDF XFA. Ele não tem a mesma estrutura XML e, como é criado ao capturar o fluxo de impressão de um arquivo, um PDF tradicional é um arquivo estático e somente leitura.

Um Documento certificado fornece documentos PDF e formulários a recipients com mais garantias de autenticidade e integridade.

## Acroformes

Acroformes são a mais antiga tecnologia de formulários interativos da Adobe; elas datam de Acrobat versão 3. A Adobe fornece o [Acrobat Forms API Reference](assets/FormsAPIReference.pdf), datado de maio de 2003, para fornecer os detalhes técnicos para essa tecnologia. Acroformes são uma combinação do
seguintes itens:

* Um PDF tradicional que define o layout estático e os gráficos do formulário.
* Campos de formulário interativo que estão presos sobre as ferramentas de formulário do programa Adobe Acrobat. Observe que essas ferramentas de formulário são um pequeno subconjunto do que está disponível no AEM Forms Designer.

## PDF/A (PDFs para arquivamento)

O PDF/A (PDF for Archives) baseia-se nos benefícios de armazenamento de documentos dos PDFs tradicionais com muitos detalhes específicos que aprimoram o arquivamento de longo prazo. O formato de arquivo PDF tradicional oferece muitos benefícios para o armazenamento de documentos de longo prazo. A natureza compacta do PDF facilita a fácil transferência e conserva espaço, e sua natureza bem estruturada permite recursos poderosos de indexação e pesquisa. O PDF tradicional tem amplo suporte para metadados e o PDF tem um longo histórico de suporte para diferentes ambientes de computador.

Como PDF, PDF/A é uma especificação padrão ISO. Ele foi desenvolvido por uma task force que incluiu AIIM (Association for Information and Image Management, Associação para a Gestão de Informações e Imagens), NPES (National Printing Equipment Association, Associação Nacional de Equipamento de Impressão) e o Gabinete Administrativo dos Tribunais dos Estados Unidos. Como o objetivo da especificação PDF/A é fornecer um formato de arquivo de longo prazo, muitos recursos de PDF são omitidos para que os arquivos possam ser autocontidos. Veja a seguir alguns pontos-chave sobre a especificação que aumenta a reprodutibilidade a longo prazo do arquivo PDF/A:

* Todo o conteúdo deve estar contido no arquivo e não pode haver dependências de fontes externas como hiperlinks, fontes ou programas de software.
* Todas as fontes devem ser incorporadas e precisam ser fontes com uma licença de uso ilimitado para documentos eletrônicos.
* JavaScript não é permitido
* A transparência não é permitida
* A criptografia não é permitida
* O conteúdo de áudio e vídeo não é permitido
* Os espaços de cores devem ser definidos de forma independente do dispositivo
* Todos os metadados devem seguir determinados padrões

## Como visualizar um arquivo PDF/A

Dois arquivos nos arquivos de amostra foram criados a partir do mesmo arquivo do Microsoft Word. Um foi criado como um PDF tradicional e o outro como um arquivo PDF/A. Abra estes dois arquivos no Acrobat Professional:

simpleWordFile.pdf
simpleWordFilePDFA.pdf

Embora os documentos tenham a mesma aparência, o arquivo PDF/A é aberto com uma barra azul na parte superior, indicando que você está visualizando esse documento no modo PDF/A. Essa barra azul é a barra de mensagens do documento do Acrobat, que você verá ao abrir determinados tipos de arquivos PDF.

![pdf-img](assets/pdfa-message.png)

A barra de mensagens do documento inclui instruções e possivelmente botões para ajudar você a concluir uma tarefa. Ele é codificado por cores e você verá a cor azul ao abrir tipos especiais de PDFs (como esse arquivo PDF/A), bem como PDFs certificados e assinados digitalmente. A barra muda para violeta para formulários PDF e para amarelo quando você participa de uma revisão de PDF.

>[!NOTE]
> Se clicar em Ativar edição, você tirará este documento da conformidade com PDF/A.




