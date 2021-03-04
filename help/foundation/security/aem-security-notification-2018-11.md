---
title: Notificação de segurança do AEM (novembro de 2018)
seo-title: Notificação de segurança do AEM (novembro de 2018)
description: Dispatcher de notificação de segurança do AEM Experience Manager
seo-description: Dispatcher de notificação de segurança do AEM Experience Manager
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 7%

---


# Notificação de segurança do AEM (novembro de 2018)

## Resumo

Este artigo aborda algumas vulnerabilidades recentes e antigas que foram relatadas recentemente no AEM. Observe que a maioria das vulnerabilidades identificadas eram problemas conhecidos do produto AEM e sua atenuação foram identificadas anteriormente, uma nova versão do dispatcher está disponível para as novas vulnerabilidades. A Adobe também solicita que os clientes concluam a [Lista de verificação de segurança do AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) e sigam as diretrizes relevantes.

## Ação obrigatória

* As implantações do AEM devem começar a usar a versão mais recente do Dispatcher.
* As regras de segurança do dispatcher devem ser aplicadas de acordo com a configuração recomendada.
* A [Lista de verificação de segurança do AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) deve ser concluída para implantações do AEM.

## Vulnerabilidades e resoluções

| Problema | Resolução | Links |
|-------|------------|-------|
| Ignorando regras do Dispatcher do AEM | Instale a versão mais recente do Dispatcher(4.3.1) e siga a configuração recomendada do Dispatcher. | Consulte [Notas de versão do AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurando o Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidade de desvio de filtro de URL que pode ser usada para contornar as regras do dispatcher - CVE-2016-0957 | Isso foi corrigido em uma versão mais antiga do Dispatcher, mas agora é recomendável instalar a versão mais recente do Dispatcher (4.3.1) e seguir a configuração recomendada do Dispatcher. | Consulte [Notas de versão do AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurando o Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidade XSS relacionada aos arquivos SWF armazenados | Isso foi resolvido com as correções de segurança lançadas anteriormente. | Consulte [Boletim de segurança do AEM APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Explosões relacionadas a senha | Siga a recomendação na lista de verificação de segurança para senhas mais fortes. | Consulte [Lista de verificação de segurança do AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposição do uso de disco para usuários anônimos | Esse problema foi resolvido para o AEM 6.1 e posterior, para o AEM 6.0, as permissões prontas para uso podem ser modificadas para serem mais restritivas. | Consulte as [notas de versão](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=pt-BR#previous-updates)para AEM 6.1 e mais recente. |
| Exposição do Proxy Social Aberto para usuários anônimos | Isso foi resolvido nas versões a partir do 6.0 SP2. | Consulte as [notas de versão](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) para AEM 6.1 e posterior. |
| Acesso ao CRX Explorer em instâncias de produção | O gerenciamento do acesso ao CRX Explorer já está coberto na Lista de verificação de segurança, o CRX Explorer deve ser removido do autor e da publicação da produção e a verificação de integridade da segurança a relata caso não tenha sido removido. | Consulte [Lista de verificação de segurança do AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets é exposto | Isso foi resolvido desde o AEM 6.2. | Consulte [Notas de versão do AEM 6.2](https://helpx.adobe.com/br/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guia do usuário do AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notas de lançamento do AEM Dispatcher Release ](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Boletins de segurança do AEM](https://helpx.adobe.com/security.html#experience-manager)

