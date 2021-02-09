---
title: Entenda os diferentes tipos de PDF forms e documentos
description: O PDF é, na verdade, uma família de formatos de arquivo, e este artigo descreve os tipos de PDFs que são importantes e relevantes para desenvolvedores de formulários.
solution: Experience Manager Forms
product: aem
type: Documentation
role: Developer
level: Beginner,Intermediate
version: 6.3,6.4,6.5
feature: Document Services
topic: development
kt: 7071
translation-type: tm+mt
source-git-commit: 1e945afddda3d7735005029952a9d7ec46828bc6
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---


# Formatos PDF

O formato PDF (Portable Documento Format) é, na verdade, uma família de formatos de arquivos, e este artigo detalha os mais relevantes para desenvolvedores de formulários. Muitos dos detalhes técnicos e padrões de diferentes tipos de PDF estão evoluindo e mudando. Alguns desses formatos e especificações são padrões ISO (International Organization for Standardization, Organização Internacional de Normalização) e outros são propriedade intelectual específica da Adobe.

Este artigo mostra como criar vários tipos de PDFs. Isso ajudará você a entender como e por que usar cada um. Todos esses tipos funcionam melhor na ferramenta cliente premier para visualizar e trabalhar com PDFs — Adobe Acrobat DC.

Este é um exemplo de um arquivo PDF/A no Acrobat DC.
![pdfa](assets/pdfa-file-in-acrobat.png)

Arquivos de amostra podem ser [baixados daqui](assets/pdf-file-types.zip)

## PDF XFA

O Adobe usa o termo formulário PDF para fazer referência aos formulários interativos e dinâmicos criados com o AEM Forms Designer. É importante observar que há outro tipo de formulário PDF, chamado de Acroform, que é diferente dos PDF forms criados no AEM Forms Designer. Os formulários e arquivos criados com o Designer são baseados na Arquitetura Forms XML (XFA) do Adobe. De muitas formas, o formato de arquivo PDF XFA está mais próximo de um arquivo HTML do que de um arquivo PDF tradicional. Por exemplo, o código a seguir mostra a aparência de um objeto de texto simples em um arquivo PDF XFA.

![campo de texto](assets/text-field.JPG)

Como você pode ver, os formulários XFA têm base em XML. Esse formato bem estruturado e flexível permite que um servidor AEM Forms transforme seus arquivos Designer em diferentes formatos, incluindo PDF, PDF/A e HTML tradicionais. É possível ver a estrutura XML completa dos formulários no Designer selecionando a guia Origem XML do Editor de layout. Você pode criar formulários XFA estáticos e dinâmicos no AEM Forms Designer.

## PDF estático

PDF forms XFA estáticos não mudarão seu layout no tempo de execução, mas podem ser interativos para o usuário. Veja a seguir algumas vantagens dos PDF forms XFA estáticos:

* PDF forms XFA estáticos não mudarão seu layout no tempo de execução, mas podem ser interativos para o usuário.
* Formulários estáticos suportam as ferramentas Comentário e Marcação da Acrobat.
* Formulários estáticos permitem importar e exportar comentários do Acrobat.
* Formulários estáticos suportam a subdefinição de fonte, que é uma técnica que pode ser feita em um servidor AEM Forms.
* Os formulários estáticos podem ser renderizados usando o visualizador incorporado pdf que acompanha os navegadores modernos.

>[!NOTE]
> Você pode criar PDFs estáticos usando o AEM Forms Designer salvando o XDP como Formulário PDF estático Adobe

## Forms dinâmico

Os PDFs XFA dinâmicos podem alterar seu layout em tempo de execução, de modo que os recursos de comentário e marcação não sejam suportados. No entanto, os PDFs XFA dinâmicos apresentam as seguintes vantagens:

* Os formulários dinâmicos são compatíveis com scripts de cliente que alteram o layout e a paginação do formulário. Por exemplo, o Purchase Order.xdp expandirá e paginará para acomodar uma quantidade infinita de dados se você salvá-los como um formulário dinâmico
* Os formulários dinâmicos suportam todas as propriedades do formulário no tempo de execução, enquanto os formulários estáticos suportam apenas um subconjunto


>[!NOTE]
> Você pode criar PDFs dinâmicos usando o AEM Forms Designer salvando o XDP como Formulário XML dinâmico Adobe

>[!NOTE]
> Os formulários dinâmicos não podem ser renderizados usando os visualizadores PDF incorporados dos navegadores modernos.


## Arquivo PDF (PDF tradicional)

O formato PDF mais popular e abrangente é o arquivo PDF tradicional. Há muitas maneiras de criar um arquivo PDF tradicional, inclusive o uso do Acrobat e de várias ferramentas de terceiros. A Acrobat oferece todas as seguintes maneiras de criar arquivos PDF tradicionais. Se você não tiver o Acrobat instalado, talvez essas opções não apareçam no computador.

* Capturando o fluxo de impressão de um aplicativo de desktop: Escolha o comando Imprimir de um aplicativo de criação e selecione o ícone da impressora Adobe PDF. Em vez de uma cópia impressa do seu documento, você terá criado um arquivo PDF do seu documento
* Usando o plug-in Acrobat PDFMaker com aplicativos do Microsoft Office: Quando você instala o Acrobat, ele adiciona um menu do Adobe PDF aos aplicativos do Microsoft Office e um ícone à faixa Office. Você pode usar esses recursos adicionados para criar arquivos PDF diretamente no Microsoft Office
* Ao usar o Acrobat Distiller para converter arquivos Postscript e Encapsulated Postscript (EPS) em PDFs: A Distiller é normalmente usada em publicações impressas e outros workflows que exigem uma conversão do formato Postscript para o formato PDF
* Sob o capô, um PDF tradicional é muito diferente de um PDF XFA. Ele não tem a mesma estrutura XML e, como é criado pela captura do fluxo de impressão de um arquivo, um PDF tradicional é um arquivo estático e somente leitura.

Um Documento certificado fornece aos recipient de formulários e documentos de PDF mais garantias de autenticidade e integridade.

## Acroformes

Os Acroformes são a mais antiga tecnologia de formulários interativos da Adobe; eles remontam à versão 3 do Acrobat. O Adobe fornece o [Acrobat Forms API Reference](assets/FormsAPIReference.pdf), datado de maio de 2003, para fornecer os detalhes técnicos desta tecnologia. As formas são uma combinação de
seguintes itens:

* Um PDF tradicional que define o layout estático e os gráficos do formulário.
* Campos de formulário interativos que são parafusos na parte superior com as ferramentas de formulário do programa Adobe Acrobat. Observe que essas ferramentas de formulário são um pequeno subconjunto do que está disponível no AEM Forms Designer.

## PDF/A (PDFs para arquivamento)

O PDF/A (PDF for Archives) baseia-se nos benefícios do armazenamento dos PDFs tradicionais com muitos detalhes específicos que aprimoram o arquivamento de longo prazo. O formato de arquivo PDF tradicional oferta muitos benefícios para armazenamentos de documento de longo prazo. A natureza compacta do PDF facilita a fácil transferência e conserva o espaço, e sua natureza bem estruturada permite recursos avançados de indexação e pesquisa. O PDF tradicional tem amplo suporte para metadados e o PDF tem um longo histórico de suporte para diferentes ambientes de computador.

Como o PDF, o PDF/A é uma especificação padrão ISO. Foi desenvolvida por uma força tarefa que incluía AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) e o Gabinete Administrativo dos Tribunais dos Estados Unidos. Como o objetivo da especificação PDF/A é fornecer um formato de arquivo de longo prazo, muitos recursos do PDF são omitidos para que os arquivos possam ser autocontidos. Estes são alguns pontos chave sobre a especificação que aprimoram a reprodutibilidade a longo prazo do arquivo PDF/A:

* Todo o conteúdo deve estar contido no arquivo e não pode haver dependências em fontes externas como hiperlinks, fontes ou programas de software.
* Todas as fontes devem ser incorporadas e precisam ser fontes com uma licença de uso ilimitado para documentos eletrônicos.
* JavaScript não é permitido
* Transparência não é permitida
* A criptografia não é permitida
* O conteúdo de áudio e vídeo não é permitido
* Os espaços de cor devem ser definidos de forma independente do dispositivo
* Todos os metadados devem seguir determinados padrões

## Como visualizar um arquivo PDF/A

Dois arquivos de amostra foram criados a partir do mesmo arquivo do Microsoft Word. Um foi criado como um PDF tradicional e o outro como um arquivo PDF/A. Abra estes dois arquivos no Acrobat Professional:

simpleWordFile.pdf
simpleWordFilePDFA.pdf

Embora os documentos tenham a mesma aparência, o arquivo PDF/A é aberto com uma barra azul na parte superior, indicando que você está visualizando esse documento no modo PDF/A. Essa barra azul é a barra de mensagens do documento da Acrobat, que você verá quando abrir certos tipos de arquivos PDF.

![pdf-img](assets/pdfa-message.png)

A barra de mensagens do documento inclui instruções e possivelmente botões para ajudá-lo a completar uma tarefa. Ele é codificado por cores e você verá a cor azul ao abrir tipos especiais de PDFs (como este arquivo PDF/A), bem como PDFs certificados e assinados digitalmente. A barra muda para violeta para PDF forms e amarelo quando você participa de uma revisão de PDF.

>[!NOTE]
> Se você clicar em Ativar edição, tirará esse documento da conformidade com PDF/A.




