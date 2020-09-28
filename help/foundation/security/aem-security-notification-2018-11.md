---
title: Notificação de segurança da AEM (novembro de 2018)
seo-title: Notificação de segurança da AEM (novembro de 2018)
description: AEM Dispatcher de Notificação de Segurança do Experience Manager
seo-description: AEM Dispatcher de Notificação de Segurança do Experience Manager
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# Notificação de segurança da AEM (novembro de 2018)

## Resumo

Este artigo aborda algumas vulnerabilidades recentes e antigas que foram recentemente reportadas em AEM. Observe que a maioria das vulnerabilidades identificadas eram problemas conhecidos do produto AEM e da mitigação foram identificados anteriormente, uma nova versão do dispatcher está disponível para as novas vulnerabilidades. A Adobe também recomenda que os clientes preencham a lista de verificação [de segurança](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) AEM e sigam as diretrizes relevantes.

## Ação obrigatória

* AEM implantações devem ser start usando a versão mais recente do Dispatcher.
* As regras de segurança do dispatcher devem ser aplicadas de acordo com a configuração recomendada.
* A Lista de Verificação [de Segurança](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) AEM deve ser concluída para implantações AEM.

## Vulnerabilidades e resoluções

| Problema | Resolução | Links |
|-------|------------|-------|
| Ignorando AEM regras do Dispatcher | Instale a versão mais recente do Dispatcher(4.3.1) e siga as configurações recomendadas do dispatcher. | Consulte [AEM Notas](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) de versão do Dispatcher e [Configuração do Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidade de desvio de filtro de URL que pode ser usada para contornar as regras do dispatcher - CVE-2016-0957 | Isso foi corrigido em uma versão anterior do Dispatcher, mas agora é recomendável instalar a versão mais recente do Dispatcher (4.3.1) e seguir a configuração recomendada do Dispatcher. | Consulte [AEM Notas](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) de versão do Dispatcher e [Configuração do Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidade XSS relacionada a arquivos SWF armazenados | Isso foi solucionado com correções de segurança lançadas anteriormente. | Consulte [AEM Boletim de segurança APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Explosões relacionadas a senha | Siga as recomendações na lista de verificação Segurança para obter senhas mais fortes. | Consulte Lista de verificação de segurança [AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposição do uso do disco para usuários anônimos | Esse problema foi resolvido para o AEM 6.1 e posterior, para o AEM 6.0, as permissões prontas para uso podem ser modificadas para serem mais restritivas. | Consulte as notas [de versão](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)para AEM 6.1 e mais recentes. |
| Exposição do Proxy Social Aberto para usuários anônimos | Isso foi resolvido em versões a partir do 6.0 SP2. | Consulte as notas [de](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) versão para AEM 6.1 e mais recentes. |
| Acesso ao CRX Explorer em instâncias de produção | O gerenciamento do acesso ao CRX Explorer já está coberto na Lista de verificação de segurança, o CRX Explorer deve ser removido do autor e da publicação da produção e a verificação de integridade da segurança a relata se não for removida. | Consulte [AEM Lista de verificação](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)de segurança. |
| BGServlets está exposto | Esta questão foi resolvida desde AEM 6.2. | See [AEM 6.2 Release Notes](https://helpx.adobe.com/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guia do usuário do Dispatcher AEM](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notas de lançamento do AEM Dispatcher Release ](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Boletins de segurança AEM](https://helpx.adobe.com/security.html#experience-manager)

