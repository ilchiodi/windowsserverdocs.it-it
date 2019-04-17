---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Controllo dei miglioramenti di AD FS in Windows Server 2016
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Controllo dei miglioramenti di AD FS in Windows Server 2016

>Si applica a: Windows Server 2016

Attualmente, in AD FS per Windows Server 2012 R2 sono generati per una singola richiesta e le informazioni relative a un registro in numerosi eventi di controllo o attività di rilascio dei token è assente (in alcune versioni di AD FS) o suddiviso in più eventi di controllo. Per impostazione predefinita, ADFS gli eventi di controllo sono disattivati per loro natura dettagliato.  
    Con il rilascio di AD FS in Windows Server 2016, il controllo è diventato più semplice e meno dettagliato.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Livelli di controllo in AD FS per Windows Server 2016  
Per impostazione predefinita, ADFS in Windows Server 2016 ha il controllo di base abilitata.  Con il controllo di base, gli amministratori vedranno gli eventi di 5 o meno per una singola richiesta.  In questo modo una riduzione significativa del numero di eventi, gli amministratori hanno esaminati, per poter visualizzare una singola richiesta.   Il livello di controllo può essere generato o ridotto utilizzando il cmdlet PowerShell: Set-AdfsProperties - AuditLevel.  Nella tabella seguente vengono illustrati i livelli di controllo disponibili.  
  
||||  
|-|-|-|  
|Livello di controllo|Sintassi di PowerShell|Descrizione|  
|Nessuno|Set-AdfsProperties - AuditLevel nessuno|Il controllo è disabilitato e gli eventi non verranno registrati.|  
|Basic (impostazione predefinita)|Set-AdfsProperties - AuditLevel Basic|Per una singola richiesta verranno registrati eventi non più di 5|  
|Verbose|Set-AdfsProperties - Verbose AuditLevel|Tutti gli eventi verranno registrati.  Registra una notevole quantità di informazioni per ogni richiesta.|  
  
Per visualizzare il livello di controllo corrente, è possibile utilizzare il cmdlet PowerShell: Get-AdfsProperties.  
  
![miglioramenti di controllo](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
Il livello di controllo può essere generato o ridotto utilizzando il cmdlet PowerShell: Set-AdfsProperties - AuditLevel.  
  
![miglioramenti di controllo](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipi di eventi di controllo  
Eventi di controllo di AD FS può essere di tipo diverso, in base a diversi tipi di richieste elaborate da ADFS. Ogni tipo di evento di controllo include dati specifici associati.  Il tipo di eventi di controllo può essere distinti tra le richieste di accesso (ad esempio, le richieste di token) e le richieste di sistema (chiamate al server a server tra cui il recupero delle informazioni di configurazione).    
  Nella tabella seguente descrive i tipi di eventi di controllo di base.  
  
||||  
|-|-|-|  
|Tipo di eventi di controllo|ID evento|Descrizione|  
|Credenziali nuova convalida riuscita|1202|Una richiesta in cui le credenziali vengono convalidate correttamente dal servizio federativo. Questo include WS-Trust, WS-Federation, SAML-P (primo segmento per generare SSO) e gli endpoint di autorizzare OAuth.|  
|Errore di convalida le credenziali|1203|Una richiesta in cui la convalida delle credenziali nuova non riuscita del servizio federativo. Questo include WS-Trust, WS-Fed, SAML-P (primo segmento per generare SSO) e gli endpoint di autorizzare OAuth.|  
|Operazione riuscita Token dell'applicazione|1200|Una richiesta in un token di sicurezza viene emesso correttamente dal servizio federativo. Per WS-Federation, SAML-P questo evento viene registrato quando la richiesta viene elaborata con l'artefatto SSO. (ad esempio, il cookie SSO).|  
|Errore nel Token dell'applicazione|1201|Una richiesta di rilascio di token di sicurezza non riuscito nel servizio federativo. Per WS-Federation, SAML-P questo evento viene registrato quando la richiesta è stata elaborata con l'artefatto SSO. (ad esempio, il cookie SSO).|  
|Operazione riuscita richiesta di modifica password|1204|Una transazione di richiesta di modifica della password in cui è stata elaborata correttamente dal servizio federativo.|  
|Errore di richiesta di modifica password|1205|Una transazione in cui richiesta di modifica della password non è riuscito a essere elaborate dal servizio federativo.| 
|Disconnetti operazione riuscita|1206|Descrive una richiesta di disconnessione riuscita.|  
|Eseguire l'accesso in uscita non riuscito|1207|Descrive una richiesta di disconnessione non riuscita.|  

  


