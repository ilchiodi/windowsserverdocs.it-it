---
title: Account utente di MultiPoint Services
description: Informazioni sugli account utente in MultiPoint Services, in particolare il tipo da usare per diversi scenari
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 0933eb856182239ef6940079f7d6c9700a868e60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858884"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Scenari di esempio: account utente di MultiPoint Services
Cosa è necessario eseguire per implementare lo scenario di account utente che si è scelto per l'ambiente di servizi MultiPoint? Nelle tabelle seguenti descrivono le singole attività da eseguire per configurare gli account utente e preparare le stazioni per gli account utente condiviso o individuale in un computer autonomo MultiPoint o nei server in rete in un gruppo di lavoro o un dominio Active Directory. Scegliere lo scenario che si applica all'ambiente. Seguire i collegamenti della tabella per completare ogni attività di configurazione necessarie.  
  
> [!NOTE]  
> Se non si è ancora deciso come configurare gli account utente, vedere [pianificare gli account utente per l'ambiente di servizi MultiPoint](Plan-user-accounts-for-your-MultiPoint-services-environment.md) Per ulteriori informazioni sull'influenza gli utenti da parte di ogni scelta.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Computer con servizi MultiPoint singolo in un ambiente autonomo (non di rete)  
  
|||  
|-|-|  
|**Non è necessario che gli utenti eseguano l'accesso.** Le stazioni possono essere disponibili per tutti coloro che risale a essi. Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1. creare un singolo account utente locale (per istruzioni, vedere [creare account utente locali](Create-local-user-accounts.md)).<br />2. [consentire a un account di avere più sessioni](Allow-one-account-to-have-multiple-sessions.md)<br />3. [configurare le stazioni per l'accesso automatico](Configure-stations-for-automatic-logon.md)|  
|**Tutti gli utenti possono condividere lo stesso accesso utente.** Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1. creare un singolo account utente locale (per istruzioni, vedere [creare account utente locali](Create-local-user-accounts.md)).<br />2. [consentire a un account di avere più sessioni](Allow-one-account-to-have-multiple-sessions.md)|  
|**Gli utenti devono avere una propria esperienza desktop di Windows.**|Creare un account utente locale per ogni utente (per istruzioni, vedere [creare account utente locali](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Più computer che eseguono servizi MultiPoint in una rete, ma senza dominio  
  
|||  
|-|-|  
|**Non è necessario che gli utenti eseguano l'accesso.** Le stazioni possono essere disponibili per tutti coloro che risale a essi. Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1. creare un singolo account utente locale in ogni server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />2. [consentire a un account di avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server<br />3. [configurare le stazioni per l'accesso automatico](Configure-stations-for-automatic-logon.md) in ogni server|  
|**Tutti gli utenti possono condividere lo stesso accesso utente.** Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1. creare un singolo account utente locale in ogni server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />2. [consentire a un account di avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server.|  
|**Gli utenti devono avere una propria esperienza desktop di Windows.**<p>-   **opzione A** : gli utenti utilizzeranno sempre le stazioni locali connesse allo stesso computer Servizi multipoint.<br />-   **opzione B** : gli utenti utilizzeranno le stazioni locali in più di un computer multipoint Services.<br />-   **opzione C** -gli utenti utilizzeranno client remoti sulla LAN.|-   **opzione A** : creare un singolo account utente locale in ogni server per gli utenti di tale server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />-   **opzione B** : creare account utente locali per ogni utente in ogni server. **Nota:** ciò significa che ogni utente dispongono di un profilo in ogni server. In altre parole, se si salva un file nei documenti mentre si è connessi alla stazione del server A, il file non sarà visibile durante l'accesso alla stazione del server B. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />-   **opzione C** -assegnare ogni utente a uno specifico computer Servizi multipoint. Creare account utente locale per gli utenti in ogni server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Uno o più computer di servizi MultiPoint in un ambiente di rete di dominio  
  
|||  
|-|-|  
|**Non è necessario che gli utenti eseguano l'accesso.** Le stazioni possono essere disponibili per tutti coloro che risale a essi. Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1. creare un account di dominio per accedere ai server.<br />2. [consentire a un account di avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server.<br />3. [configurare le stazioni per l'accesso automatico](Configure-stations-for-automatic-logon.md) in ogni server.|  
|**Tutti gli utenti possono condividere lo stesso accesso utente.** Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1. creare un account di dominio per un gruppo o per ogni utente.<br />2. [consentire a un account di avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server.|  
|**Gli utenti devono avere una propria esperienza desktop di Windows.**<p>-   **opzione A** -qualsiasi utente con un account di dominio può usare il computer Servizi multipoint.<br />-   **opzione B** -voglio limitare gli account di dominio che possono accedere al server.|-   **opzione A** -non è necessaria alcuna installazione. Per impostazione predefinita, tutti gli utenti di dominio hanno accesso a qualsiasi computer MultiPoint servizi sulla rete.<br />-   **opzione B** : limitare l'accesso degli account utente di dominio al computer Servizi multipoint. Per istruzioni, vedere [limitare agli utenti di accedere al server](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Si vuole usare gli account utente locali e gestirli separatamente dagli account di dominio.** Ad esempio, si desidera gestire i servizi MultiPoint ma non il dominio o non si desidera concedere agli account di dominio per tutti gli utenti ai servizi MultiPoint.|Creare uno o più account utente locale su ciascun server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<p>**Nota:** ciò significa che ciascun account utente dispongono di un profilo in ogni server. In altre parole, se si salva un file nei documenti mentre si è connessi alla stazione del server A, il file non sarà visibile durante l'accesso alla stazione del server B.|  