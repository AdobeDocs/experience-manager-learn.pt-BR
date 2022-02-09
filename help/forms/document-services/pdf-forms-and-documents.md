---
title: Entender os diferentes tipos de PDF forms e documentos
description: O PDF é, na verdade, uma família de formatos de arquivo, e este artigo descreve os tipos de PDF que são importantes e relevantes para desenvolvedores de formulários.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.3,6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

O Portable Document Format (PDF) é uma família de formatos de arquivo, e este artigo detalha os mais relevantes para desenvolvedores de formulários. Muitos dos detalhes técnicos e padrões de diferentes tipos de PDF estão evoluindo e mudando. Alguns desses formatos e especificações são normas da Organização Internacional de Normalização (ISO) e outros são propriedade intelectual específica da Adobe.

Este artigo mostra como criar vários tipos de PDF. Ajuda você a entender como e por usar cada um. Todos esses tipos funcionam melhor na principal ferramenta de cliente para visualização e trabalho com o PDF — Adobe Acrobat DC.

A seguir, um exemplo de um arquivo PDF/A no Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Arquivos de exemplo podem ser [baixado aqui](assets/pdf-file-types.zip)

## PDF da arquitetura Forms XML (PDF XFA)

O Adobe usa o termo formulário PDF XFA para fazer referência ao Forms interativo e dinâmico criado com o AEM Forms Designer. A Forms e os arquivos criados com o Designer são baseados na XFA (XML Forms Architecture) do Adobe. De muitas formas, o formato de arquivo PDF XFA está mais próximo de um arquivo HTML do que de um arquivo PDF tradicional. Por exemplo, o código a seguir mostra a aparência de um objeto de texto simples em um arquivo PDF XFA.

![Campo de texto](assets/text-field.JPG)

Forms XFA são baseadas em XML. Esse formato bem estruturado e flexível permite que um servidor AEM Forms transforme seus arquivos Designer em diferentes formatos, incluindo PDF, PDF/A tradicional e HTML. Você pode ver a estrutura XML completo do Forms no Designer, selecionando a guia Origem XML no Editor de layout. Você pode criar Forms XFA estática e dinâmica no AEM Forms Designer.

## PDF estático

O layout estático de PDF forms XFA nunca é alterado no tempo de execução, mas pode ser interativo para o usuário. Estas são algumas vantagens dos PDF forms XFA estáticos:

* O layout estático de PDF forms XFA nunca é alterado no tempo de execução, mas pode ser interativo para o usuário.
* O Forms estático suporta as ferramentas Comentário e Marcação da Acrobat.
* O Forms estático permite importar e exportar comentários do Acrobat.
* O Forms estático suporta a subconfiguração de fonte, que é uma técnica que pode ser feita em um Servidor AEM Forms.
* O Forms estático pode ser renderizado usando o visualizador de PDF integrado que vem com os navegadores modernos.

>[!NOTE]
>
> Você pode criar PDF estáticas usando o AEM Forms Designer, salvando o XDP como Formulário de PDF estático Adobe



### Forms dinâmico

Os PDF XFA dinâmicos podem alterar seu layout no tempo de execução, de modo que os recursos de comentário e marcação não sejam compatíveis. No entanto, os PDF dinâmicos de XFA oferecem as seguintes vantagens:

* Os formulários dinâmicos são compatíveis com scripts de cliente que alteram o layout e a paginação do formulário. Por exemplo, o Purchase Order.xdp será expandido e paginado para acomodar uma quantidade infinita de dados se você salvá-lo como um formulário dinâmico
* Os formulários dinâmicos oferecem suporte a todas as propriedades do formulário no tempo de execução, enquanto os formulários estáticos oferecem suporte somente a um subconjunto

>[!NOTE]
>
> Você pode criar pdfs dinâmicos usando o AEM Forms Designer salvando o XDP como Formulário XML dinâmico do Adobe

>[!NOTE]
>
> Os formulários dinâmicos não podem ser renderizados usando os visualizadores pdf incorporados dos navegadores modernos.

### Arquivo PDF (PDF tradicional)

Um documento certificado fornece ao PDF document e aos recipients da Forms mais garantias de autenticidade e integridade.

O formato de PDF mais popular e penetrante é o arquivo PDF tradicional. Há muitas maneiras de criar um arquivo PDF tradicional, incluindo o uso do Acrobat e de muitas ferramentas de terceiros. O Acrobat fornece todas as seguintes maneiras de criar arquivos PDF tradicionais. Se você não tiver o Acrobat instalado, talvez não veja essas opções no computador.

* Capturando o fluxo de impressão de um aplicativo de desktop: Escolha o comando Imprimir de um aplicativo de criação e selecione o ícone da impressora Adobe PDF. Em vez de uma cópia impressa do seu documento, você criará um arquivo PDF do seu documento
* Usando o plug-in Acrobat PDFMaker com aplicativos do Microsoft Office: Ao instalar o Acrobat, ele adiciona um menu do Adobe PDF aos aplicativos do Microsoft Office e um ícone à faixa do Office. Você pode usar esses recursos adicionados para criar arquivos PDF diretamente no Microsoft Office
* Ao usar o Acrobat Distiller para converter arquivos Postscript e Encapsulated Postscript (EPS) em PDF: O Distiller geralmente é usado na publicação de impressão e em outros fluxos de trabalho que exigem uma conversão do formato Postscript para o formato PDF.
* Sob o capuz, um PDF tradicional é muito diferente de um PDF XFA. Ela não tem a mesma estrutura XML e, como é criada ao capturar o fluxo de impressão de um arquivo, um PDF tradicional é um arquivo estático e somente leitura.

Um Documento certificado fornece documento PDF e forma destinatários com mais garantias de autenticidade e integridade.

### Acroformes

As formas são a tecnologia de forma interativa mais antiga da Adobe; elas datam de Acrobat versão 3. O Adobe fornece [Referência da API do Acrobat Forms](assets/FormsAPIReference.pdf), datada de maio de 2003, para fornecer os detalhes técnicos desta tecnologia. Acroformes são uma combinação dos seguintes itens:

* Um PDF tradicional que define o layout estático e os gráficos do formulário.
* Campos de formulário interativo que são vinculados às ferramentas de formulário do programa Adobe Acrobat. Essas ferramentas de formulário são um pequeno subconjunto do que está disponível no AEM Forms Designer.

### PDF/A (PDF para arquivamento)

O PDF/A (PDF para arquivos) baseia-se nos benefícios de armazenamento de documentos dos PDF tradicionais com muitos detalhes específicos que aprimoram o arquivamento de longo prazo. O formato de arquivo PDF tradicional oferece muitos benefícios para o armazenamento de documentos a longo prazo. A natureza compacta do PDF facilita a fácil transferência e conserva espaço, e sua natureza bem estruturada permite recursos poderosos de indexação e pesquisa. O PDF tradicional tem amplo suporte para metadados e o PDF tem um longo histórico de suporte a diferentes ambientes de computador.

Como o PDF, o PDF/A é uma especificação padrão ISO. Ele foi desenvolvido por uma task force que incluiu AIIM (Association for Information and Image Management, Associação para a Gestão de Informações e Imagens), NPES (National Printing Equipment Association, Associação Nacional de Equipamento de Impressão) e o Gabinete Administrativo dos Tribunais dos Estados Unidos. Como o objetivo da especificação PDF/A é fornecer um formato de arquivo de longo prazo, muitos recursos do PDF são omitidos para que os arquivos possam ser autocontidos. A seguir, alguns pontos-chave sobre a especificação que aumenta a reprodutibilidade a longo prazo do arquivo PDF/A:

* Todo o conteúdo deve estar contido no arquivo e não pode haver dependências de fontes externas como hiperlinks, fontes ou programas de software.
* Todas as fontes devem ser incorporadas e precisam ser fontes com uma licença de uso ilimitado para documentos eletrônicos.
* JavaScript não é permitido
* A transparência não é permitida
* A criptografia não é permitida
* O conteúdo de áudio e vídeo não é permitido
* Os espaços de cores devem ser definidos de forma independente do dispositivo
* Todos os metadados devem seguir determinados padrões

### Exibição de um arquivo PDF/A

Dois arquivos nos arquivos de amostra foram criados no mesmo arquivo do Microsoft Word. Um foi criado como PDF tradicional e o outro como um arquivo PDF/A. Abra estes dois arquivos no Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Embora os documentos tenham a mesma aparência, o arquivo PDF/A é aberto com uma barra azul na parte superior, indicando que você está visualizando esse documento no modo PDF/A. Essa barra azul é a barra de mensagens do documento do Acrobat, que você vê ao abrir determinados tipos de arquivos de PDF.

![Pdf-img](assets/pdfa-message.png)

A barra de mensagens do documento inclui instruções e possivelmente botões para ajudar você a concluir uma tarefa. Ele é codificado por cores e você verá a cor azul ao abrir tipos especiais de PDF (como este arquivo PDF/A), bem como PDF certificados e assinados digitalmente. A barra muda para violeta para PDF forms e amarelo quando você está participando de uma revisão de PDF.

>[!NOTE]
>
> Se você clicar em Habilitar edição, retire este documento da conformidade com o PDF/A.
