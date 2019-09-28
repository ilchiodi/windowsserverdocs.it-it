---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Controllo dei miglioramenti di AD FS in Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4eb93513d12b2bba2620ff16be24f62ace5dee85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407255"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Controllo dei miglioramenti di AD FS in Windows Server 2016


Attualmente, in AD FS per Windows Server 2012 R2 sono presenti numerosi eventi di controllo generati per una singola richiesta e le informazioni rilevanti su un'attività di rilascio di token o di accesso sono assenti (in alcune versioni di AD FS) o distribuite tra più eventi di controllo. Per impostazione predefinita, ADFS gli eventi di controllo sono disattivati per loro natura dettagliato.  
    Con il rilascio di AD FS in Windows Server 2016, il controllo è diventato più semplificato e meno dettagliato.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Livelli di controllo in AD FS per Windows Server 2016  
Per impostazione predefinita, AD FS in Windows Server 2016 è abilitato il controllo di base.  Con il controllo di base, gli amministratori vedranno 5 o meno gli eventi per una singola richiesta.  Questa operazione contrassegna una riduzione significativa del numero di eventi che gli amministratori devono esaminare per poter visualizzare una singola richiesta.   Il livello di controllo può essere generato o abbassato usando il cmdlet di PowerShell:  Set-AdfsProperties-AuditLevel.  La tabella seguente illustra i livelli di controllo disponibili.  
  
||||  
|-|-|-|  
|Livello di controllo|Sintassi di PowerShell|Descrizione|  
|Nessuno|Set-AdfsProperties-AuditLevel None|Il controllo è disabilitato e non viene registrato alcun evento.|  
|Basic (impostazione predefinita)|Set-AdfsProperties-AuditLevel Basic|Non verranno registrati più di 5 eventi per una singola richiesta|  
|Verbose|Set-AdfsProperties-AuditLevel verbose|Tutti gli eventi verranno registrati.  Questa operazione registrerà una quantità significativa di informazioni per ogni richiesta.|  
  
Per visualizzare il livello di controllo corrente, è possibile usare il cmdlet di PowerShell:  Get-AdfsProperties.  
  
![miglioramenti del controllo](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
Il livello di controllo può essere generato o abbassato usando il cmdlet di PowerShell:  Set-AdfsProperties-AuditLevel.  
  
![miglioramenti del controllo](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipi di eventi di controllo  
AD FS eventi di controllo possono essere di tipi diversi, in base ai diversi tipi di richieste elaborate da AD FS. A ogni tipo di evento di controllo sono associati dati specifici.  Il tipo di eventi di controllo può essere differenziato tra le richieste di accesso (ad esempio, le richieste di token) e le richieste di sistema (chiamate server-server, incluso il recupero delle informazioni di configurazione).    
  Nella tabella seguente vengono descritti i tipi di base degli eventi di controllo.  
  
||||  
|-|-|-|  
|Tipo di evento di controllo|ID evento|Descrizione|  
|Convalida nuova credenziale riuscita|1202|Una richiesta in cui le nuove credenziali vengono convalidate correttamente dal Servizio federativo. Sono inclusi WS-Trust, WS-Federation, SAML-P (prima parte per generare SSO) e gli endpoint di autorizzazione OAuth.|  
|Nuovo errore di convalida delle credenziali|1203|Una richiesta in cui la convalida delle credenziali aggiornata non è riuscita nel Servizio federativo. Sono inclusi WS-Trust, WS-Fed, SAML-P (prima tappa per generare SSO) e gli endpoint di autorizzazione OAuth.|  
|Il token dell'applicazione è riuscito|1200|Una richiesta in cui un token di sicurezza viene emesso correttamente dal Servizio federativo. Per WS-Federation, SAML-P viene registrato quando la richiesta viene elaborata con l'artefatto SSO. come il cookie SSO.|  
|Errore del token dell'applicazione|1201|Richiesta in cui il rilascio del token di sicurezza non è riuscito nel Servizio federativo. Per WS-Federation, SAML-P viene registrato quando la richiesta è stata elaborata con l'artefatto SSO. come il cookie SSO.|  
|Richiesta di modifica della password riuscita|1204|Transazione in cui la richiesta di modifica della password è stata elaborata correttamente dal Servizio federativo.|  
|Errore di richiesta di modifica della password|1205|Transazione in cui la richiesta di modifica della password non è stata elaborata dal Servizio federativo.| 
|Disconnessione riuscita|1206|Descrive una richiesta di disconnessione riuscita.|  
|Errore di disconnessione|1207|Descrive una richiesta di disconnessione non riuscita.|  

  


