---
title: Etapas simplificadas para instalar o AEM Forms no Windows
seo-title: Etapas simplificadas para instalar o AEM Forms no Windows
description: Etapas rápidas e fáceis para instalar o AEM Forms no Windows
seo-description: Etapas rápidas e fáceis para instalar o AEM Forms no Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---


# Etapas simplificadas para instalar o AEM Forms no Windows

>[!NOTE]
>
>Nunca clique no pod de Start rápido AEM, se você pretende usar o AEM Forms.
>
>Além disso, verifique se não há espaços no caminho da pasta Instalação do AEM Forms.
>
>Por exemplo, não instale o AEM Forms em c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Se você estiver instalando o AEM Forms 6.5, certifique-se de ter instalado os seguintes redistribuíveis do Microsoft Visual C++ de 32 bits.
>
>* Microsoft Visual C++ 2008 redistribuível
>* Microsoft Visual C++ 2010 redistribuível
>* Microsoft Visual C++ 2012 redistribuível
>* Microsoft Visual C++ 2013 redistribuível (a partir de 6.5)


Embora seja recomendável seguir a [documentação oficial](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) para instalar o AEM Forms. As etapas a seguir podem ser seguidas para instalar e configurar o AEM Forms no ambiente Windows:

* Verifique se o JDK apropriado está instalado
   * AEM 6.2 é necessário: Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 e AEM 6.4 é necessário: Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 você precisa de JDK 8 ou JDK 11
* [Os ](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) requisitos oficiais do JDK estão listados aqui
* Verifique se o JAVA_HOME está definido para apontar para o JDK que você instalou.
   * Para criar a variável JAVA_HOME no Windows, siga as etapas abaixo:
      * Clique com o botão direito do mouse em Meu computador e selecione Propriedades
      * Na guia Avançado, selecione Variáveis de Ambiente e crie uma nova variável de sistema chamada JAVA_HOME.
      * Defina o valor da variável para apontar para JDK instalado no sistema. Por exemplo, c:\program files\java\jdk1.8.0_25

* Crie uma pasta chamada AEMForms na sua unidade C
* Localize o AEMQuickStart.Jar e mova-o para a pasta AEMForms
* Copie o arquivo license.properties para esta pasta AEMForms
* Crie um arquivo em lote chamado &quot;StartAemForms.bat&quot; com o seguinte conteúdo:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Aqui AEM_6.3_Quickstart.jar é o nome do meu jar AEM quickstart.
   * Você pode renomear seu jar para qualquer nome, mas certifique-se de que esse nome seja refletido no arquivo de lote.Salve o arquivo de lote na Pasta AEMForms.

* Abra um novo prompt de comando e navegue até c:\aemforms.

* Execute o arquivo StartAemForms.bat do prompt de comando.

* Você deve obter uma pequena caixa de diálogo indicando o progresso da inicialização.

* Quando a inicialização estiver concluída, abra o arquivo sling.properties. Está localizado em c:\AEMForms\crx-quickstart\conf folder.

* Copie as 2 linhas a seguir na parte inferior do arquivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncyCastle.jce.provider.BouncyCastleProvider=org.bouncyCastle.***
* Essas duas propriedades são necessárias para que os serviços de documento funcionem
* Salve o arquivo sling.properties

* [Fazer login no Package Share](http://localhost:4502/crx/packageshare/login.html)

   * Você precisará do Adobe Id para fazer logon no compartilhamento de pacote
   * Procurar o pacote AEM Forms Add on apropriado para a sua versão do AEM Forms e do sistema operacional
   * Ou [você pode baixar os formulários apropriados e adicionar package](https://helpx.adobe.com/br/aem-forms/kb/aem-forms-releases.html)
   * Após instalar o pacote de adição, siga as etapas a seguir

      * **Verifique se todos os pacotes estão no estado ativo. (Exceto para o pacote Assinaturas do AEMFD).**
      * **Normalmente, levaria 5 ou mais minutos para todos os pacotes chegarem ao estado ativo.**
   * **Quando todos os pacotes estiverem ativos (exceto o pacote Assinaturas do AEMFD), reinicie o sistema para concluir a instalação do AEM Forms**


* Adicione o pacote `sun.util.calendar` à lista de permissões:

   1. Abra o console da Web do Felix na [janela do navegador](http://localhost:4502/system/console/configMgr)
   2. Pesquise e abra a Configuração do Firewall de desserialização: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Adicionar `sun.util.calendar` como uma nova entrada em `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Salve as alterações.

Parabéns!!! Agora você instalou e configurou o AEM Forms em seu sistema.
Dependendo das suas necessidades, você pode configurar [Extensões de Reader](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) ou [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) no seu servidor
