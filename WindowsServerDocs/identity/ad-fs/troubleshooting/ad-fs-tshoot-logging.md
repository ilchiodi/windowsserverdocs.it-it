---
title: 'AD FS, risoluzione dei problemi: gli eventi di controllo e registrazione'
description: Questo documento descrive come usare i vari registri di ADFS per risolvere i problemi
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 20e2d0747b98e7c7728230d0768506261f5b0d50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825122"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Risoluzione dei problemi di AD FS - eventi e la registrazione
ADFS offre due log primario che può essere usato nella risoluzione dei problemi.  Sono:

- il Log di amministrazione
- il Log di traccia  
 
Ognuno di questi log verrà spiegato di seguito.

## <a name="admin-log"></a>Log amministrazione
Il log di amministrazione fornisce informazioni di alto livello sui problemi che si verificano ed sono abilitati per impostazione predefinita.

### <a name="to-view-the-admin-log"></a>Per visualizzare il log di amministrazione
1.  Apri il Visualizzatore eventi
2.  Espandere **registri applicazioni e servizi**.
3.  Espandere **ADFS**.
4.  Fare clic su **Admin**.

![miglioramenti di controllo](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>Log di traccia
Il log di traccia è in cui i messaggi dettagliati vengono registrati e saranno il log più utile per risolvere il problema. Poiché molte delle informazioni di log di traccia possono essere generate in un breve periodo di tempo, che può influire sulle prestazioni del sistema, i log di traccia sono disabilitati per impostazione predefinita. 

### <a name="to-enable-and-view-the-trace-log"></a>Per abilitare e visualizzare il log di traccia
1.  Apri il Visualizzatore eventi
2.  Fare clic su **registri applicazioni e servizi** e selezionare Visualizza e fare clic su **Visualizza registri analitici e Debug**.  Verranno visualizzati ulteriori nodi a sinistra.
![miglioramenti di controllo](media/ad-fs-tshoot-logging/event2.PNG)  
3.  Espandere la traccia di AD FS
4.  Fare clic su Debug e selezionare **Attiva registro**.
![miglioramenti di controllo](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Informazioni di controllo di eventi per ADFS in Windows Server 2016  
Per impostazione predefinita, ADFS in Windows Server 2016 ha un livello base di controllo abilitato.  Con il controllo di base, gli amministratori noteranno minore o uguale a 5 eventi per una singola richiesta.  In questo modo contrassegnato una diminuzione significativa nel numero di eventi gli amministratori dispongono di esaminare, in modo da visualizzare una singola richiesta.   Il livello di controllo può essere aumentato o abbassata usando il cmdlet di PowerShell:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

La tabella seguente illustra i livelli di controllo disponibili.  

|Livello di controllo|Sintassi di PowerShell|Descrizione|  
|----- | ----- | ----- |
|Nessuno|Set-AdfsProperties - AuditLevel None|Il controllo è disabilitato e non verrà registrato alcun evento.|  
|Basic (impostazione predefinita)|Set-AdfsProperties - AuditLevel Basic|Verranno registrati non più di 5 eventi per una singola richiesta|  
|Verbose|Set-AdfsProperties - AuditLevel dettagliato|Verranno registrati tutti gli eventi.  Registra una quantità significativa di informazioni per ogni richiesta.|  
  
Per visualizzare il livello di controllo corrente, è possibile usare il cmdlet di PowerShell:  Get-AdfsProperties.  
  
![miglioramenti di controllo](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
Il livello di controllo può essere aumentato o abbassata usando il cmdlet di PowerShell:  Set-AdfsProperties - AuditLevel.  
  
![miglioramenti di controllo](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>Tipi di eventi  
Gli eventi di AD FS possono essere di tipi diversi, in base ai tipi diversi di richieste elaborate da AD FS. Ogni tipo di evento ha dati specifici associati.  Il tipo di eventi può essere differenziato tra le richieste di accesso (ad esempio le richieste di token) e le richieste di sistema (chiamate di un server inclusi il recupero di informazioni di configurazione).    

La tabella seguente descrive i tipi di base degli eventi.  
  
|Tipo evento|ID evento|Descrizione| 
|----- | ----- | ----- | 
|Credenziale aggiornata convalida riuscita|1202|Una richiesta in cui nuove credenziali vengono convalidate correttamente dal servizio federativo. Ciò include WS-Trust, WS-Federation, SAML-P (prima parte per generare l'accesso SSO) e gli endpoint di autorizzazione OAuth.|  
|Errore di convalida delle credenziali aggiornato|1203|Una richiesta in cui non è riuscita la convalida delle credenziali aggiornato nel servizio federativo. Ciò include WS-Trust, WS-Fed, SAML-P (prima parte per generare l'accesso SSO) e gli endpoint di autorizzazione OAuth.|  
|Esito positivo di Token di applicazione|1200|Una richiesta in cui un token di sicurezza viene rilasciato correttamente dal servizio federativo. Per WS-Federation, SAML-P viene registrato quando la richiesta viene elaborata con l'elemento SSO. (ad esempio, il cookie SSO).|  
|Errore di Token dell'applicazione|1201|Una richiesta in cui emissione di token di sicurezza non riuscita nel servizio federativo. Per WS-Federation, SAML-P viene registrato quando l'elaborazione della richiesta con l'elemento SSO. (ad esempio, il cookie SSO).|  
|Richiesta di modifica password riuscita|1204|Una transazione di richiesta di modifica della password in cui è stata elaborata correttamente dal servizio federativo.|  
|Errore di richiesta di modifica password|1205|Una transazione in cui la modifica della password richiesta non riuscita essere elaborato dal servizio federativo.| 
|Disconnettersi da operazione riuscita|1206|Descrive una richiesta di disconnessione ha esito positivo.|  
|L'accesso in uscita non riuscito|1207|Descrive una richiesta di disconnessione non riuscita.|  

## <a name="security-auditing"></a>Controllo della sicurezza
Controllo della sicurezza dell'account del servizio AD FS in alcuni casi consentono di rilevare i problemi con gli aggiornamenti della password, la registrazione di richiesta/risposta, essendo intestazioni e i risultati di registrazione dispositivi.  Il controllo dell'account del servizio AD FS è disabilitato per impostazione predefinita.

### <a name="to-enable-security-auditing"></a>Per abilitare il controllo di sicurezza
1.       Fare clic su Start, scegliere **i programmi**, scegliere **strumenti di amministrazione**, quindi fare clic su **criteri di sicurezza locali**.
2.       Passare alla cartella **Impostazioni sicurezza\Criteri locali\Assegnazione diritti utenti** e quindi fare doppio clic su **Generazione di controlli di sicurezza**.
3.       Nel **impostazioni di sicurezza locali** scheda, verificare che sia elencato l'account del servizio AD FS. Se non è presente, fare clic su Aggiungi utente o gruppo e aggiungerlo all'elenco e quindi fare clic su OK.
4.       Aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente per abilitare controllo auditpol.exe /set /subcategory: "Generato dall'applicazione" /Failure /Success 5.       Chiusura **criteri di sicurezza locali**e quindi aprire lo snap-in Gestione ADFS.
 
Per aprire lo snap-in Gestione AD FS, fare clic su Start, scegliere Programmi, strumenti di amministrazione e quindi fare clic su Gestione AD FS.
 
6.       Nel riquadro azioni, fare clic su Edit Federation Service proprietà 7.       Nella finestra di dialogo Proprietà servizio federativo, fare clic sulla scheda eventi. 8.       Selezionare il **Success audits** e **Failure audits** caselle di controllo.
9.       Fare clic su OK.

![miglioramenti di controllo](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>Le istruzioni riportate sopra vengono usate solo se ADFS si trova in un server membro autonomo.  Se ADFS è in esecuzione in un controller di dominio, invece di criteri di sicurezza locali, usare il **criterio Controller di dominio predefinito** che si trova **controller/foresta/domini/dominio di gestione di criteri di gruppo**.  Fare clic su Modifica e passare a **Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti di gestione**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Messaggi di Windows Communication Foundation e Windows Identity Foundation
Oltre alla registrazione di traccia, in alcuni casi potrebbe essere necessario visualizzare i messaggi di Windows Communication Foundation (WCF) e Windows Identity Foundation (WIF) per poter risolvere un problema. Questa operazione può essere eseguita modificando i **Microsoft.IdentityServer.ServiceHost.Exe.Config** file nel server AD FS. 

Questo file si trova **\Windows\ADFS < % system radice % >** ed è in formato XML. Le sezioni rilevanti del file sono illustrate di seguito: 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Dopo aver applicato queste modifiche, salvare la configurazione e riavviare il servizio AD FS. Dopo aver abilitato queste tracce impostando le opzioni appropriate, verranno visualizzati nel log di traccia di ADFS nel Visualizzatore eventi di Windows.

## <a name="correlating-events"></a>Correlazione di eventi
Uno degli aspetti più difficili da risolvere i problemi è problemi di accesso che generano molti errori o eventi di debug.

A tale scopo, AD FS mette in correlazione tutti gli eventi che vengono registrati nel Visualizzatore eventi, l'amministratore sia i log di debug, che corrispondono a una particolare richiesta con un univoco identificatore univoco globale (GUID) di ID attività. Questo ID viene generato quando la richiesta di rilascio dei token viene inizialmente presentata l'applicazione web (per le applicazioni usando il profilo di richiedenti passivi) o le richieste inviate direttamente al provider di attestazioni (per applicazioni che usano WS-Trust). 

![ID attività](media/ad-fs-tshoot-logging/activityid1.png)

Questo ID attività rimane invariato per tutta la durata della richiesta e viene registrato come parte di tutti gli eventi registrati nell'evento Viewer per tale richiesta. Ciò significa che:
 - che il filtro o eseguendo una ricerca nel Visualizzatore eventi utilizza questa attività che consente di tenere traccia di tutti gli eventi correlati che corrispondono alla richiesta di token ID
 - lo stesso ID attività viene registrato in diversi computer che consente alla risoluzione dei problemi di una richiesta dell'utente in più computer, ad esempio il proxy Server federativo (FSP)
 - l'ID dell'attività verrà anche visualizzato nel browser dell'utente se la richiesta di AD FS non riesce in qualsiasi modo, consentendo in tal modo l'utente comunicare questo ID al supporto tecnico o il supporto IT.

![ID attività](media/ad-fs-tshoot-logging/activityid2.png)

Per agevolare il processo di risoluzione dei problemi, AD FS registra anche l'evento ID chiamante ogni volta che il processo di rilascio di token ha esito negativo in un server AD FS. Questo evento contiene il tipo di attestazione e il valore di uno dei seguenti tipi di attestazione, presupponendo che queste informazioni è state passate al servizio federativo come parte di una richiesta di token:
- http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- http://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

L'evento ID chiamante registra inoltre l'ID dell'attività che consentono di utilizzare tale ID attività per filtrare o cercare i registri eventi per una particolare richiesta.




## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)
