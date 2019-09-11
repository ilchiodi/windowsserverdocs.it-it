---
title: Introduzione alla registrazione accesso utenti
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1706756b52777f5dd3bf1db59fb2ed087ca8648
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866261"
---
# <a name="get-started-with-user-access-logging"></a>Introduzione alla registrazione accesso utenti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Registrazione accesso utenti (registrazione accesso utenti) è una funzionalità di Windows Server che aggrega i dati di utilizzo del client in base al ruolo e ai prodotti in un server locale. Consente agli amministratori di Windows Server di quantificare le richieste provenienti dai computer client per ruoli e servizi in un server locale.  
  
REGISTRAZIONE accesso utenti viene installato e abilitato per impostazione predefinita e raccoglie i dati in tempo quasi reale. La funzionalità Registrazione accesso utente non richiede alcuna configurazione da parte degli amministratori, ma è possibile abilitarla o disabilitarla. Per altre informazioni, vedere [Manage User Access Logging](Manage-User-Access-Logging.md). Il servizio registrazione accesso utenti aggrega i dati di utilizzo del client in base ai ruoli e ai prodotti nei file di database locali.  Gli amministratori possono quindi usare Strumentazione gestione Windows (WMI) o i cmdlet di Windows PowerShell per recuperare quantità e istanze in base al ruolo del server (o prodotto software), all'utente, al dispositivo, al server locale e alla data.  
  
> [!NOTE]  
> Registrazione accesso utenti supporta [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000).  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
REGISTRAZIONE accesso utenti aggrega gli eventi univoci relativi alle richieste dei dispositivi e degli utenti client registrati in un database locale. Tali record vengono quindi resi disponibili (con una query eseguita da un amministratore del server) per consentire il recupero delle quantità e delle istanze in base al ruolo del server, all'utente, al dispositivo, al server locale e alla data.  Inoltre, registrazione accesso utenti è stato esteso per consentire agli sviluppatori di software non Microsoft di instrumentare gli eventi di registrazione accesso utenti per l'aggregazione da parte di Windows Server.  
  
REGISTRAZIONE accesso utenti può eseguire le attività seguenti:  
  
-   Quantificare le richieste di server fisici locali o virtuali da parte degli utenti client.  
  
-   Quantificare le richieste di prodotti software installati in un server fisico locale o in un server virtuale da parte degli utenti client.  
  
-   Recuperare i dati in un server locale che esegue Hyper-V per identificare i periodi di alta e bassa richiesta in un computer virtuale Hyper-V.  
  
-   Recuperare i dati del servizio Registrazione accesso utenti provenienti da più server remoti.  
  
Inoltre, gli sviluppatori di software possono instrumentare gli eventi registrazione accesso utenti che possono essere quindi aggregati e recuperati utilizzando le interfacce WMI e Windows PowerShell.  
  
Il servizio Registrazione accesso utenti supporta i servizi e i ruoli del server seguenti:  
  
-   Servizi certificati Active Directory  
  
-   Active Directory Rights Management Services (AD RMS)  
  
-   BranchCache  
  
-   Domain Name System (DNS)  
  
    > [!NOTE]  
    > Registrazione accesso utenti raccoglie i dati DNS ogni 24 ore ed è disponibile un cmdlet di Registrazione accesso utenti separato per questo scenario.  
  
-   Dynamic Host Configuration Protocol (DHCP)  
  
-   Server fax  
  
-   Servizi file  
  
-   Server FTP (File Transfer Protocol)  
  
-   Hyper-V  
  
    > [!NOTE]  
    > Registrazione accesso utenti raccoglie i dati Hyper-V ogni 24 ore ed è disponibile un cmdlet di Registrazione accesso utenti separato per questo scenario.  
  
-   Server Web (IIS)  
  
    > [!WARNING]  
    > Per usare Registrazione accesso utenti con IIS, è necessario usare iisual.exe. Per altre informazioni, vedere l'articolo relativo all' [analisi dei dati di utilizzo del client con la registrazione accesso utenti IIS](http://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging).  
  
-   Servizi Coda MSMQ (Microsoft Message Queue)  
  
-   Servizi di accesso e criteri di rete  
  
-   Servizi di stampa e digitalizzazione  
  
-   Servizio Routing e Accesso remoto (RRAS)  
  
-   Servizi di distribuzione Windows (WDS)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> L'uso di Registrazione accesso utenti non è consigliato nei server connessi direttamente a Internet, come i server Web che occupano uno spazio degli indirizzi accessibile via Internet, o in scenari in cui la funzione principale del server è offrire prestazioni estremamente elevate, ad esempio in ambienti con carichi di lavoro di elaborazione a prestazioni elevate. REGISTRAZIONE accesso utenti è destinato principalmente a scenari Intranet piccoli, medi e aziendali in cui è previsto un volume elevato, ma non è così elevato quanto le distribuzioni che gestiscono il volume di traffico con connessione Internet a intervalli regolari.  
  
## <a name="BKMK_NEW"></a>Funzionalità importanti  
La tabella seguente descrive le funzioni principali di Registrazione accesso utenti e il relativo valore potenziale.  
  
|Funzionalità|Value|  
|-----------------|---------|  
|Raccolta e aggregazione dei dati degli eventi relativi alle richieste dei client in tempo quasi reale.|Consente di salvare fino a tre anni di dati. **Importante:** Gli amministratori devono imporre la conformità dei dati raccolti e i periodi di conservazione dei dati con l'informativa sulla privacy dell'organizzazione e le normative locali.|  
|Esecuzione di query sui dati di Registrazione accesso utenti usando le interfacce di WMI o Windows PowerShell per recuperare i dati relativi alle richieste dei client in un server locale o remoto.|Registrazione accesso utente offre una visualizzazione centralizzata dei dati di utilizzo correnti. Gli amministratori dei server e IT possono recuperare i dati e coordinarsi con gli amministratori dell'organizzazione per ottimizzare l'uso delle licenze software incluse nei contratti multilicenza.|  
|Abilitata per impostazione predefinita.|Non è necessario che gli amministratori del server configurino questa funzionalità in modo che tutte le caratteristiche di base siano disponibili e funzionanti.|  
  
## <a name="data-logged-with-ual"></a>Dati registrati con Registrazione accesso utenti  
I dati relativi agli utenti riportati di seguito vengono registrati con Registrazione accesso utenti.  
  
|Data|Descrizione|  
|--------|---------------|  
|**Nome utente**|Nome utente indicato sul client che accompagna le voci del servizio Registrazione accesso utenti provenienti dai ruoli e dai prodotti installati, se applicabile.|  
|**ActivityCount**|Il numero di volte in cui un utente specifico ha eseguito l'accesso a un ruolo o un servizio.|  
|**FirstSeen**|Data e ora del primo accesso di un utente a un ruolo o un servizio.|  
|**LastSeen**|Data e ora dell'ultimo accesso di un utente a un ruolo o un servizio.|  
|**ProductName**|Nome del prodotto software padre, ad esempio Windows, che fornisce dati del servizio Registrazione accesso utenti.|  
|**RoleGUID**|GUID assegnato o registrato dal servizio Registrazione accesso utenti, che rappresenta il ruolo del server o il prodotto installato.|  
|**RoleName**|Nome del ruolo, componente o prodotto secondario che fornisce dati di Registrazione accesso utenti. Tale nome è anche associato a un ProductName e un RoleGUID.|  
|**TenantIdentifier**|GUID univoco per un client tenant di un ruolo o un prodotto installato che accompagna i dati di Registrazione accesso utenti, se applicabile.|  
  
In Registrazione accesso utenti vengono registrati i dati relativi ai dispositivi riportati di seguito.  
  
|Data|Descrizione|  
|--------|---------------|  
|**IPAddress**|Indirizzo IP di un dispositivo client usato per accedere a un ruolo o un servizio.|  
|**ActivityCount**|Il numero di volte in cui un dispositivo specifico ha eseguito l'accesso al ruolo o al servizio.|  
|**FirstSeen**|Data e ora in cui un indirizzo IP è stato usato per la prima volta per accedere a un ruolo o un servizio.|  
|**LastSeen**|Data e ora in cui un indirizzo IP è stato usato per l'ultima volta per accedere a un ruolo o un servizio.|  
|**ProductName**|Nome del prodotto software padre, ad esempio Windows, che fornisce dati del servizio Registrazione accesso utenti.|  
|**RoleGUID**|GUID assegnato o registrato dal servizio Registrazione accesso utenti, che rappresenta il ruolo del server o il prodotto installato.|  
|**RoleName**|Nome del ruolo, componente o prodotto secondario che fornisce dati di Registrazione accesso utenti. Tale nome è anche associato a un ProductName e un RoleGUID.|  
|**TenantIdentifier**|GUID univoco per un client tenant di un ruolo o un prodotto installato che accompagna i dati di Registrazione accesso utenti, se applicabile.|  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
REGISTRAZIONE accesso utenti può essere utilizzato in qualsiasi computer in cui sono in esecuzione versioni di Windows Server successive a Windows Server 2012.  
  
## <a name="see-also"></a>Vedere anche  
[Registrazione accesso utenti](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) su MSDN.  
[Gestire Registrazione accesso utenti](Manage-User-Access-Logging.md)  
  

