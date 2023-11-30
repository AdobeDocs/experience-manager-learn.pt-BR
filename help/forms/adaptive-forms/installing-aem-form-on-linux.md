---
title: Instalação do AEM Forms no Linux
description: Saiba como instalar bibliotecas de 32 bits para o AEM Forms para funcionar na instalação do Linux.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---

# Instalação da versão de 32 bits das bibliotecas compartilhadas

Quando o AEM FORMS OSGi ou o AEM Forms j2EE é implantado no Linux, é necessário garantir que as versões de 32 bits de um conjunto de bibliotecas compartilhadas sejam instaladas e estejam disponíveis.  As descrições são dos próprios pacotes.

* expat (Biblioteca C de analisador XML orientado por fluxo para análise de XML, escrita por James Clark)
* fontconfig (Biblioteca de configuração e personalização de fontes projetada para localizar fontes no sistema e selecioná-las de acordo com os requisitos especificados pelos aplicativos)
* freetype (mecanismo de renderização de fontes), desenvolvido para fornecer suporte avançado a fontes para uma variedade de plataformas e ambientes. Ele pode abrir e gerenciar arquivos de fonte, bem como carregar, ditar e renderizar glifos individuais com eficiência. Não é um servidor de fontes ou uma biblioteca completa de renderização de texto)
* glibc (Bibliotecas principais para o sistema GNU e sistemas GNU/Linux, bem como muitos outros sistemas que usam Linux como kernel)
* libcurl (biblioteca de transferência de URL do lado do cliente)
* libICE (Biblioteca Exchange entre clientes)
* libicu (Biblioteca que oferece suporte robusto e completo a Unicode e locale - Componentes internacionais para Unicode). São necessárias as edições de 64 e 32 bits desta biblioteca
* libSM (biblioteca de Gerenciamento de sessão X11)
* libuuid (biblioteca de Identificador exclusivo universal compatível com DCE - usada para gerar identificadores exclusivos para objetos que podem ser acessíveis além do sistema local)
* libX11 (biblioteca do lado do cliente X11)
* libXau (Protocolo de autorização X11 - útil para restringir o acesso do cliente à exibição)
* libxcb (Vínculo de linguagem C do protocolo X - XCB)
* libXext (biblioteca para extensões comuns do protocolo X11)
* libXinerama (extensão X11), que oferece suporte para estender um desktop em várias exibições. O nome é um trocadilho com Cinerama, um formato de filme widescreen que usava vários projetores. libXinerama é a biblioteca que faz interface com a extensão RandR)
* libXrandr (A extensão Xinerama é amplamente obsoleta hoje em dia - foi substituída pela extensão RandR)
* libXrender (biblioteca do cliente da Extensão de Renderização X) nss-softokn-freebl (Biblioteca Freebl do Network Security Services)
* zlib (Biblioteca de compactação de dados sem perdas, sem patentes e de uso geral)

A partir do Red Hat Enterprise Linux 6, a edição de 32 bits de uma biblioteca terá a extensão de nome de arquivo .686, enquanto a edição de 64 bits terá .x86_64. Exemplo, expat.i686. Antes do RHEL 6, as edições de 32 bits tinham a extensão .i386. Antes de instalar as edições de 32 bits, verifique se as edições de 64 bits mais recentes estão instaladas. Se a edição de 64 bits de uma biblioteca for anterior à versão de 32 bits que está sendo instalada, ocorrerá o seguinte erro:

0mError: Versões multilib protegidas: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Problemas de versão multilib encontrados.]

## Primeira instalação

No Red Hat Enterprise Linux, use o YellowDog Update Modifier (YUM) para instalar, conforme mostrado abaixo:

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum instalar freetype.i686
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

Além disso, você precisa criar os symlinks libcurl.so, libcrypto.so e libssl.so, apontando para as versões mais recentes de 32 bits das bibliotecas libcurl, libcrypto e libssl, respectivamente. Você pode encontrar os arquivos em /usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Atualizações do sistema existente

Pode haver conflitos entre as arquiteturas x86_64 e i686 durante atualizações, como este: Erro: Erro de verificação de transação: arquivo /lib/ld-2.28.so da instalação de glibc-2.28-72.el8.i686 conflita com o arquivo do pacote glibc32-2.28-42.1.el8.x86_64

Se você encontrar isso, desinstale o pacote incorreto primeiro, como neste caso: yum remove glibc32-2.28-42.1.el8.x86_64

Feito isso, você deseja que as versões x86_64 e i686 sejam exatamente as mesmas, como por exemplo desta saída para o comando: yum info glibc

Última verificação de expiração de metadados: 0:41:33 atrás em Sat 18 Jan 2020 11:37:08:00 EST.
Nome dos Pacotes Instalados : glibc Versão : 2.28 Versão : 72.el8 Arquitetura : i686 Tamanho : 13 M Fonte : glibc-2.28-72.el8.src.rpm Repositório : @System Do repo : BaseOS Resumo : As bibliotecas libc GNU URL : http://www.gnu.org/software/glibc/ Licença : LGPLv2+ e LGPLv2+ com exceções e GPLv2+ e GPLv2+ com exceções e BSD e Inner-Net e ISC e Domínio Público e GFDL : A Descrição O pacote c contém bibliotecas padrão que são usadas por : vários programas no sistema. Para economizar espaço em disco e : memória, bem como para facilitar a atualização, o código de sistema comum é : mantido em um local e compartilhado entre programas. Este pacote específico : contém os conjuntos mais importantes de bibliotecas compartilhadas: a biblioteca C : padrão e a biblioteca matemática padrão. Sem essas duas bibliotecas, um sistema : Linux não funcionará.

Nome : glibc Versão : 2.28 Versão : 72.el8 Arquitetura : x86_64 Tamanho : 15 M Fonte : glibc-2.28-72.el8.src.rpm Repositório : @System Do repositório : BaseOS Resumo : O URL das bibliotecas libc GNU : http://www.gnu.org/software/glibc/ Licença : LGPLv2+ e LGPLv2+ com exceções e GPLv2+ com exceções e BSD e Inner-Net e ISC e Domínio Público e GFDL Descrição : O pacote glibc contém bibliotecas padrão usadas por : vários programas no sistema. Para economizar espaço em disco e : memória, bem como para facilitar a atualização, o código de sistema comum é : mantido em um local e compartilhado entre programas. Este pacote específico : contém os conjuntos mais importantes de bibliotecas compartilhadas: a biblioteca C : padrão e a biblioteca matemática padrão. Sem essas duas bibliotecas, um sistema : Linux não funcionará.

## Alguns comandos yum úteis

yum list instalado yum search [part_of_package_name]
yum whatproviders [package_name]
instalação do yum [package_name]
reinstalação do yum [package_name]
informações do yum [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
atualização do yum [package_name]
