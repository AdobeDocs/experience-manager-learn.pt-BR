---
title: Instalação do AEM Forms no Linux
description: Saiba como instalar bibliotecas de 32 bits para o AEM Forms para funcionar na instalação do Linux.
feature: Formulários adaptáveis
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: desenvolvimento
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# Instalação da versão de 32 bits das bibliotecas compartilhadas

Quando o AEM FORMS OSGi ou o AEM Forms j2EE são implantados no Linux, é necessário garantir que as versões de 32 bits de um conjunto de bibliotecas compartilhadas sejam instaladas e disponibilizadas.  As descrições são dos próprios pacotes.

* expat (Biblioteca de analisador C XML orientado por fluxo para análise XML, escrita por James Clark)
* fontconfig (biblioteca de configuração e personalização de fontes projetada para localizar fontes no sistema e selecioná-las de acordo com os requisitos especificados pelos aplicativos)
* freetype (mecanismo de renderização de fonte, desenvolvido para fornecer suporte avançado a fontes para uma variedade de plataformas e ambientes. Ele pode abrir e gerenciar arquivos de fonte, bem como carregar, sugerir e renderizar glifos individuais de maneira eficiente. Não é um servidor de fonte ou uma biblioteca completa de renderização de texto)
* glibc (bibliotecas principais para sistemas GNU e GNU/Linux, bem como muitos outros sistemas que usam Linux como kernel)
* libcurl (biblioteca de transferência de URL do lado do cliente)
* libICE (Biblioteca de Intercâmbio entre Clientes)
* libicu (Biblioteca que fornece Unicode robusto e completo e suporte local - Componentes internacionais para Unicode). As edições de 64 e 32 bits desta biblioteca são necessárias
* libSM (biblioteca de gerenciamento de sessão X11)
* libuuid (biblioteca de identificadores universais exclusiva compatível com DCE - usada para gerar identificadores exclusivos para objetos que podem ser acessados fora do sistema local)
* libX11 (biblioteca do lado do cliente X11)
* libXau (Protocolo de autorização X11 - útil para restringir o acesso do cliente à exibição)
* libxcb (Protocolo X Ligação de Idioma C - XCB)
* libXext (Biblioteca para extensões comuns do protocolo X11)
* libXinerama (extensão X11), que fornece suporte para a extensão de um desktop em várias exibições. O nome é um trocadilho no Cinerama, um formato de filme widescreen que usou vários projetores. libXinerama é a biblioteca que faz interface com a extensão RandR)
* libXrandr (a extensão Xinerama é amplamente obsoleta hoje em dia - foi substituída pela extensão RandR)
* libXrender (biblioteca do cliente Extensão de Renderização X)
nss-softokn-freebl (Biblioteca livre para serviços de segurança de rede)
* zlib (Biblioteca de compactação de dados sem patente, de uso geral)

A partir do Red Hat Enterprise Linux 6, a edição de 32 bits de uma biblioteca terá a extensão de arquivo .686, enquanto a edição de 64 bits terá .x86_64. Exemplo, expat.i686. Antes do RHEL 6, as edições de 32 bits tinham a extensão .i386. Antes de instalar as edições de 32 bits, verifique se as edições de 64 bits mais recentes estão instaladas. Se a edição de 64 bits de uma biblioteca for anterior à versão de 32 bits que está sendo instalada, você receberá um erro como abaixo:

0mErro: Versões multilib protegidas: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mErro: Problemas de versão multilib encontrados.]

## Instalação pela primeira vez

No Red Hat Enterprise Linux, use o YellowDog Update Modifier (YUM) para instalar, conforme mostrado abaixo:

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

Além disso, é necessário criar links simbólicos libcurl.so, libcrypto.so e libssl.so apontando para as versões mais recentes de 32 bits das bibliotecas libcurl, libcrypto e libssl, respectivamente. Você pode encontrar os arquivos em /usr/lib/
ln-s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln-s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln-s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Atualizações do sistema existente

pode haver conflitos entre as arquiteturas x86_64 e i686 durante as atualizações, como este:
Erro: Erro de verificação de transação:
arquivo /lib/ld-2.28.so da instalação do glibc-2.28-72.el8.i686 entra em conflito com o arquivo do pacote glibc32-2.28-42.1.el8.x86_64

Se você tiver feito isso, desinstale o pacote incorreto primeiro, como neste caso:
yum remove glibc32-2.28-42.1.el8.x86_64

Dito isto e feito, você quer que as versões x86_64 e i686 sejam exatamente as mesmas, como por exemplo desta saída para o comando:
yum info glibc

Última verificação de expiração de metadados: Há 0:41:33 de 18 de janeiro de 2020 11:37:08 AM EST.
Pacotes instalados
Nome : glibc
Versão : 2,28
Versão : 72.el8
Arquitetura : i686
Tamanho : 13 M
Fonte : glibc-2.28-72.el8.src.rpm
Repositório : @System
Do acordo de recompra: BaseOS
Resumo : As bibliotecas da biblioteca GNU
URL : http://www.gnu.org/software/glibc/
Licença : LGPLv2+ e LGPLv2+ com exceções e GPLv2+ e GPLv2+ com exceções e BSD e Inner-Net e ISC e Domínio Público e GFDL
Descrição : O pacote glibc contém bibliotecas padrão que são usadas pelo : vários programas no sistema. Para economizar espaço em disco e : além de facilitar a atualização, o código comum do sistema é: mantido num único local e partilhado entre programas. Este pacote específico : O contém os conjuntos mais importantes de bibliotecas compartilhadas: a norma C : e a biblioteca matemática padrão. Sem essas duas bibliotecas, um : O sistema Linux não funcionará.

Nome : glibc
Versão : 2,28
Versão : 72.el8
Arquitetura : x86_64
Tamanho : 15 M
Fonte : glibc-2.28-72.el8.src.rpm
Repositório : @System
Do acordo de recompra: BaseOS
Resumo : As bibliotecas da biblioteca GNU
URL : http://www.gnu.org/software/glibc/
Licença : LGPLv2+ e LGPLv2+ com exceções e GPLv2+ e GPLv2+ com exceções e BSD e Inner-Net e ISC e Domínio Público e GFDL
Descrição : O pacote glibc contém bibliotecas padrão que são usadas pelo : vários programas no sistema. Para economizar espaço em disco e : além de facilitar a atualização, o código comum do sistema é: mantido num único local e partilhado entre programas. Este pacote específico : O contém os conjuntos mais importantes de bibliotecas compartilhadas: a norma C : e a biblioteca matemática padrão. Sem essas duas bibliotecas, um : O sistema Linux não funcionará.

## Alguns comandos úteis de yum

lista yum instalada
pesquisa de yum [part_of_package_name]
yum, o que fornece [nome_do_pacote]
yum install [nome_do_pacote]
reinstale o yum [nome_do_pacote]
informações do yum [nome_do_pacote]
depleção de yum [nome_do_pacote]
remover [nome_do_pacote]
yum check-update [nome_do_pacote]
atualização yum [nome_do_pacote]
