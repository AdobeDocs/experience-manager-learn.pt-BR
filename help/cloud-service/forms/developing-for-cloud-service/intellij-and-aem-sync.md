---
title: Configuração do IntelliJ com a ferramenta Repo
description: Preparar o IntelliJ para sincronização com a instância pronta para nuvem do AEM
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
duration: 92
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Instalação do Cygwin


Cygwin é um ambiente de programação e tempo de execução compatível com POSIX que é executado nativamente no Microsoft Windows.
Instale o [Cygwin](https://www.cygwin.com/). Instalei o na pasta C:\cygwin64
>[!NOTE]
> Certifique-se de instalar pacotes zip, unzip, curl e rsync com sua instalação do cygwin

Crie uma pasta chamada adoberepo em c:\cloudmanager.

[Instalar a ferramenta de repositório](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) Instalar a ferramenta de repositório nada mais é do que copiar o arquivo de repositório e colocá-lo na pasta c:\cloudmanger\adoberepo.

Adicione o seguinte à variável de ambiente Caminho C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Configurar ferramentas externas

* Iniciar IntelliJ
* Pressione Ctrl+Alt+S para iniciar a janela de configurações.
* Selecione Ferramentas->Ferramentas externas, clique no sinal + e insira o seguinte, conforme mostrado na captura de tela.
  ![rep](assets/repo.png)
* Certifique-se de criar um grupo chamado repositório digitando &quot;repo&quot; no campo suspenso Grupo e todos os comandos criados pertencem ao grupo **repo**


**Colocar Comando**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Diretório de Trabalho**: \$ProjectFileDir\$
![colocar-comando](assets/put-command.png)

**Obter Comando**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Diretório de Trabalho**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Comando de Status**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Diretório de Trabalho**: \$ProjectFileDir\$
![comando de status](assets/status-command.png)

**Comando de comparação**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Diretório de Trabalho**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

Extraia o arquivo .repo do [repo.zip](assets/repo.zip) e coloque-o na pasta raiz dos projetos AEM. C:\CloudManager\aem-banking-application). Abra o arquivo .repo e verifique se o servidor e as configurações de credenciais correspondem ao seu ambiente.
Abra o arquivo .gitignore, adicione o seguinte na parte inferior do arquivo e salve as alterações
\# repositório
.repo

Selecione qualquer projeto no projeto aem-banking-application, como ui.content, e clique com o botão direito do mouse. Você verá a opção repo e, sob a opção repo, você verá os 4 comandos que adicionamos anteriormente.

## Configurar instância do autor do AEM{#set-up-aem-author-instance}

As etapas a seguir podem ser seguidas para configurar rapidamente a instância pronta para nuvem no sistema local.
* [Baixe o AEM SDK mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [Baixe o complemento mais recente do AEM Forms](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* Criar a seguinte estrutura de pastas
c:\aemformscs\aem-sdk\author

* Extraia o arquivo aem-sdk-quickstart-xxxxxxx.jar do arquivo zip do AEM SDK e coloque-o na pasta c:\aemformscs\aem-sdk\author. Renomeie o arquivo jar para aem-author-p4502.jar

* Abra o prompt de comando e navegue até c:\aemformscs\aem-sdk\author
insira o seguinte comando java -jar aem-author-p4502.jar -gui. Isso iniciará a instalação do AEM.
* Fazer logon usando credenciais de administrador/administrador
* Parar a instância do AEM
* Crie a seguinte estrutura de pastas.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Copie o aem-forms-addon-xxxxxx.far para a pasta de instalação
* Abra o prompt de comando e navegue até c:\aemformscs\aem-sdk\author
insira o seguinte comando java -jar aem-author-p4502.jar -gui. Isso implantará o pacote complementar de formulários na instância do AEM.

## Próximas etapas

[Sincronizar formulários e modelos do AEM com o projeto do AEM](./deploy-your-first-form.md)
