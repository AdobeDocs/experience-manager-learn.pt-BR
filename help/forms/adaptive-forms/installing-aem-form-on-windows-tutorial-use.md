---
title: Etapas simplificadas para instalar o AEM Forms no Windows
description: Etapas rápidas e fáceis para instalar o AEM Forms no Windows
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 113
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 1%

---

# Etapas simplificadas para instalar o AEM Forms no Windows

>[!NOTE]
>
>Nunca clique duas vezes no jar AEM Quick Start, se pretender usar o AEM Forms.
>
>Além disso, verifique se não há espaços no caminho da pasta AEM Forms Installation.
>
>Por exemplo, não instale o AEM Forms na pasta c:\jack and jill\AEM Forms

>[!NOTE]
>
>Se estiver instalando o AEM Forms 6.5, certifique-se de ter instalado os seguintes redistribuíveis do Microsoft Visual C++ de 32 bits.
>
>* Microsoft Visual C++ 2008 redistribuível
>* Microsoft Visual C++ 2010 redistribuível
>* Microsoft Visual C++ 2012 redistribuível
>* Microsoft Visual C++ 2013 redistribuível (a partir da 6.5)

Embora recomendemos seguir o [documentação oficial](https://helpx.adobe.com/br/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) para instalar o AEM Forms. As seguintes etapas podem ser seguidas para instalar e configurar o AEM Forms no ambiente Windows:

* Verifique se você tem o JDK apropriado instalado
   * AEM 6.2 necessário: Oracle SE 8 JDK 1.8.x (64 bits)
   * AEM 6.3 e AEM 6.4 necessários: Oracle SE 8 JDK 1.8.x (64 bits)
   * AEM 6.5 você precisa do JDK 8 ou JDK 11
   * [Requisitos oficiais do JDK](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=pt-BR) estão listados aqui
* Verifique se JAVA_HOME está definido para apontar para o JDK instalado.
   * Para criar a variável JAVA_HOME no Windows, siga as etapas abaixo:
      * Clique com o botão direito em Meu computador e selecione Propriedades
      * Na guia Avançado, selecione Variáveis de ambiente e crie uma nova variável de sistema chamada JAVA_HOME.
      * Defina o valor da variável para apontar para o JDK instalado no sistema. Por exemplo c:\program files\java\jdk1.8.0_25

* Crie uma pasta chamada AEMForms na unidade C
* Localize o AEMQuickStart.Jar e mova-o para a pasta AEMForms
* Copie o arquivo license.properties para esta pasta AEMForms
* Crie um arquivo de lote chamado &quot;StartAemForms.bat&quot; com o seguinte conteúdo:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Aqui AEM_6.5_Quickstart.jar é o nome do meu jar AEM quickstart.
   * Você pode renomear o jar com qualquer nome, mas certifique-se de que esse nome esteja refletido no arquivo de lote. Salve o arquivo de lote na pasta AEMForms.

* Abra um novo prompt de comando e navegue até _c:\aemforms_.

* Execute o arquivo StartAemForms.bat a partir do prompt de comando.

* Você deve receber uma pequena caixa de diálogo indicando o progresso da inicialização.

* Quando a inicialização estiver concluída, abra o arquivo sling.properties. Está localizado na pasta c:\AEMForms\crx-quickstart\conf.

* Copie as 2 linhas a seguir na parte inferior do arquivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Essas duas propriedades são necessárias para que os serviços de documento funcionem
* Salve o arquivo sling.properties
* [Baixar o pacote de complemento de formulários apropriado](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* Instale o pacote complementar do forms usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp).
* Depois de instalar o pacote complementar, siga as etapas a seguir

   * **Verifique se todos os pacotes estão no estado ativo. (Exceto para o pacote de assinaturas AEMFD).**
   * **Normalmente, levaria 5 minutos ou mais para todos os pacotes chegarem ao estado ativo.**

   * **Quando todos os pacotes estiverem ativos (exceto o pacote de assinaturas AEMFD), reinicie o sistema para concluir a instalação do AEM Forms**

## pacote sun.util.calendar para a lista de permissões

1. Abra o Felix web console em seu [janela do navegador](http://localhost:4502/system/console/configMgr)
1. Pesquisar e abrir Configuração do firewall de desserialização: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. Adicionar `sun.util.calendar` como uma nova entrada em `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
1. Salve as alterações.

Parabéns!! Agora você instalou e configurou o AEM Forms em seu sistema.
Dependendo das suas necessidades, você pode configurar  [Extensões Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) ou [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html) no servidor
