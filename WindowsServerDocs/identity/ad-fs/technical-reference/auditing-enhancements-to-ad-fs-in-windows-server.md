---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Controllo dei miglioramenti di AD FS in Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2f6e4abb4255281be85b7fa928566f681bcf2de2
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188356"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Controllo dei miglioramenti di AD FS in Windows Server 2016


Attualmente, in AD FS per Windows Server 2012 R2 sono generati per una singola richiesta e le informazioni relative a un registro in numerosi eventi di controllo o attività di rilascio dei token è assente (in alcune versioni di AD FS) o suddiviso in più eventi di controllo. Per impostazione predefinita, ADFS gli eventi di controllo sono disattivati per loro natura dettagliato.  
    Con il rilascio di ADFS in Windows Server 2016, il controllo è diventato più semplice e meno dettagliato.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Livelli di controllo in AD FS per Windows Server 2016  
Per impostazione predefinita, ADFS in Windows Server 2016 ha il controllo di base abilitata.  Con il controllo di base, gli amministratori noteranno minore o uguale a 5 eventi per una singola richiesta.  In questo modo contrassegnato una diminuzione significativa nel numero di eventi gli amministratori dispongono di esaminare, in modo da visualizzare una singola richiesta.   Il livello di controllo può essere aumentato o abbassata usando il cmdlet di PowerShell:  Set-AdfsProperties - AuditLevel.  La tabella seguente illustra i livelli di controllo disponibili.  
  
||||  
|-|-|-|  
|Livello di controllo|Sintassi di PowerShell|Descrizione|  
|Nessuno|Set-AdfsProperties - AuditLevel None|Il controllo è disabilitato e non verrà registrato alcun evento.|  
|Basic (impostazione predefinita)|Set-AdfsProperties - AuditLevel Basic|Verranno registrati non più di 5 eventi per una singola richiesta|  
|Verbose|Set-AdfsProperties - AuditLevel dettagliato|Verranno registrati tutti gli eventi.  Registra una quantità significativa di informazioni per ogni richiesta.|  
  
Per visualizzare il livello di controllo corrente, è possibile usare il cmdlet di PowerShell:  Get-AdfsProperties.  
  
![miglioramenti di controllo](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
Il livello di controllo può essere aumentato o abbassata usando il cmdlet di PowerShell:  Set-AdfsProperties - AuditLevel.  
  
![miglioramenti di controllo](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipi di eventi di controllo  
Gli eventi di controllo di AD FS può essere di tipi diversi, in base ai tipi diversi di richieste elaborate da AD FS. Ogni tipo di evento Audit ha dati specifici associati.  Il tipo di eventi di controllo può essere differenziato tra le richieste di accesso (ad esempio le richieste di token) e le richieste di sistema (chiamate di un server inclusi il recupero di informazioni di configurazione).    
  La tabella seguente descrive i tipi di base degli eventi di controllo.  
  
||||  
|-|-|-|  
|Tipo di evento audit|ID evento|Descrizione|  
|Credenziale aggiornata convalida riuscita|1202|Una richiesta in cui nuove credenziali vengono convalidate correttamente dal servizio federativo. Ciò include WS-Trust, WS-Federation, SAML-P (prima parte per generare l'accesso SSO) e gli endpoint di autorizzazione OAuth.|  
|Errore di convalida delle credenziali aggiornato|1203|Una richiesta in cui non è riuscita la convalida delle credenziali aggiornato nel servizio federativo. Ciò include WS-Trust, WS-Fed, SAML-P (prima parte per generare l'accesso SSO) e gli endpoint di autorizzazione OAuth.|  
|Esito positivo di Token di applicazione|1200|Una richiesta in cui un token di sicurezza viene rilasciato correttamente dal servizio federativo. Per WS-Federation, SAML-P viene registrato quando la richiesta viene elaborata con l'elemento SSO. (ad esempio, il cookie SSO).|  
|Errore di Token dell'applicazione|1201|Una richiesta in cui emissione di token di sicurezza non riuscita nel servizio federativo. Per WS-Federation, SAML-P viene registrato quando l'elaborazione della richiesta con l'elemento SSO. (ad esempio, il cookie SSO).|  
|Richiesta di modifica password riuscita|1204|Una transazione di richiesta di modifica della password in cui è stata elaborata correttamente dal servizio federativo.|  
|Errore di richiesta di modifica password|1205|Una transazione in cui la modifica della password richiesta non riuscita essere elaborato dal servizio federativo.| 
|Disconnettersi da operazione riuscita|1206|Descrive una richiesta di disconnessione ha esito positivo.|  
|L'accesso in uscita non riuscito|1207|Descrive una richiesta di disconnessione non riuscita.|  

  


