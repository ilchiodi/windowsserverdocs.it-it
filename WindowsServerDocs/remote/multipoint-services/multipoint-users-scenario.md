---
title: Account utente di multiPoint Services
description: Altre informazioni sugli account utente in servizi MultiPoint, in particolare il tipo da usare per scenari diversi
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 31279f81d5af597b0b1f1729c953fefaf24a214f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855362"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Scenari esemplificativi: Account utente di multiPoint Services
Cosa è necessario eseguire per implementare lo scenario di account utente che si è scelto per l'ambiente di servizi MultiPoint? Nelle tabelle seguenti descrivono le singole attività da eseguire per configurare gli account utente e preparare le stazioni per gli account utente condiviso o individuale in un computer autonomo MultiPoint o nei server in rete in un gruppo di lavoro o un dominio Active Directory. Scegliere lo scenario che si applica all'ambiente. Seguire i collegamenti della tabella per completare ogni attività di configurazione necessarie.  
  
> [!NOTE]  
> Se non si è ancora deciso come configurare gli account utente, vedere [pianificare gli account utente per l'ambiente di servizi MultiPoint](Plan-user-accounts-for-your-MultiPoint-services-environment.md) Per ulteriori informazioni sull'influenza gli utenti da parte di ogni scelta.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Computer con servizi MultiPoint singolo in un ambiente autonomo (non di rete)  
  
|||  
|-|-|  
|**Gli utenti non sono necessario effettuare l'accesso.** Le stazioni possono essere disponibili per tutti coloro che risale a essi. Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1.  Creare account utente locale (per istruzioni, vedere [creare account utente locali](Create-local-user-accounts.md).)<br />2.  [Consentire a un account per avere più sessioni](Allow-one-account-to-have-multiple-sessions.md)<br />3.  [Configurare le stazioni per l'accesso automatico](Configure-stations-for-automatic-logon.md)|  
|**Gli utenti possono condividere lo stesso accesso utente.** Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1.  Creare account utente locale (per istruzioni, vedere [creare account utente locali](Create-local-user-accounts.md).)<br />2.  [Consentire a un account per avere più sessioni](Allow-one-account-to-have-multiple-sessions.md)|  
|**Gli utenti devono disporre la propria esperienza desktop di Windows singoli.**|Creare un account utente locale per ogni utente (per istruzioni, vedere [creare account utente locali](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Più computer che eseguono servizi MultiPoint in una rete, ma senza dominio  
  
|||  
|-|-|  
|**Gli utenti non sono necessario effettuare l'accesso.** Le stazioni possono essere disponibili per tutti coloro che risale a essi. Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1.  Creare un account utente locale singolo in ogni server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />2.  [Consentire a un account per avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server<br />3.  [Configurare le stazioni per l'accesso automatico](Configure-stations-for-automatic-logon.md) in ogni server|  
|**Gli utenti possono condividere lo stesso accesso utente.** Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1.  Creare un account utente locale singolo in ogni server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />2.  [Consentire a un account per avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server.|  
|**Gli utenti devono disporre la propria esperienza desktop di Windows singoli.**<br /><br />-   **Opzione A** -gli utenti verranno sempre utilizzato stazioni locale connesse allo stesso computer MultiPoint Services.<br />-   **Opzione B** -gli utenti utilizzeranno postazioni locali in più di un computer di MultiPoint Services.<br />-   **Opzione C** -gli utenti utilizzeranno i client remoti nella rete LAN.|-   **Opzione A** -creare un account utente locale su ciascun server per gli utenti di tale server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />-   **Opzione B** -creare account utente locali per ogni utente in ogni server. **Nota:** Ciò significa che ogni utente dispongono di un profilo in ogni server. In altre parole, se si salva un file nella cartella documenti, mentre è avvenuto stazione del Server, non visualizzeranno il file quando accedono stazione del Server B. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br />-   **Opzione C** -assegnare ogni utente a un computer di servizi MultiPoint specifico. Creare account utente locale per gli utenti in ogni server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Uno o più computer di servizi MultiPoint in un ambiente di rete di dominio  
  
|||  
|-|-|  
|**Gli utenti non sono necessario effettuare l'accesso.** Le stazioni possono essere disponibili per tutti coloro che risale a essi. Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1.  Creare un account di dominio per accedere a server.<br />2.  [Consentire a un account per avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server.<br />3.  [Configurare le stazioni per l'accesso automatico](Configure-stations-for-automatic-logon.md) in ogni server.|  
|**Gli utenti possono condividere lo stesso accesso utente.** Non è necessario una singola esperienza desktop Windows che include cartelle private per l'archiviazione dei dati o desktop personalizzato.|1.  Creare un account di dominio per un gruppo o per ogni utente.<br />2.  [Consentire a un account per avere più sessioni](Allow-one-account-to-have-multiple-sessions.md) in ogni server.|  
|**Gli utenti devono disporre la propria esperienza desktop di Windows singoli.**<br /><br />-   **Opzione A** -qualsiasi utente con un account di dominio può utilizzare il computer MultiPoint Services.<br />-   **Opzione B** -si desidera limitare gli account di dominio possono accedere al server.|-   **Opzione A** -non è necessaria alcuna configurazione. Per impostazione predefinita, tutti gli utenti di dominio hanno accesso a qualsiasi computer MultiPoint servizi sulla rete.<br />-   **Opzione B** -limitare l'accesso degli account utente di dominio per il computer MultiPoint Services. Per istruzioni, vedere [limitare agli utenti di accedere al server](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Si vuole usare gli account utente locale e gestirli separatamente dal mio account di dominio.** Ad esempio, si desidera gestire i servizi MultiPoint ma non il dominio o non si desidera concedere agli account di dominio per tutti gli utenti ai servizi MultiPoint.|Creare uno o più account utente locale su ciascun server. (Per istruzioni, vedere [Creare account utente locali](Create-local-user-accounts.md).)<br /><br />**Nota:** Ciò significa che ogni account utente dispongono di un profilo in ogni server. In altre parole, se si salva un file nella cartella documenti, mentre è avvenuto stazione del Server, non visualizzeranno il file quando accedono stazione del Server B.|  