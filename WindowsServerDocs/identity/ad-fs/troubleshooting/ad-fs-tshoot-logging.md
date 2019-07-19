---
title: 'AD FS risoluzione dei problemi: controllo di eventi e registrazione'
description: Questo documento descrive come usare i vari log di AD FS per risolvere i problemi
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bfa305103e81f316dc5ad5df22cd238f6fb5ec31
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314341"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>AD FS risoluzione dei problemi-eventi e registrazione
AD FS fornisce due log principali che possono essere utilizzati per la risoluzione dei problemi.  Sono:

- Log amministratore
- Registro di traccia  
 
Ognuno di questi log verrà illustrato di seguito.

## <a name="admin-log"></a>Log amministratore
Il log di amministrazione offre informazioni di alto livello sui problemi che si verificano ed è abilitato per impostazione predefinita.

### <a name="to-view-the-admin-log"></a>Per visualizzare il log amministratore
1.  Apri il Visualizzatore eventi
2.  Espandere **registro applicazioni e servizi**.
3.  Espandere **ad FS**.
4.  Fare clic su **admin**.

![miglioramenti del controllo](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>Registro di traccia
Il log di traccia è il punto in cui vengono registrati i messaggi dettagliati e sarà il log più utile per la risoluzione dei problemi. Poiché è possibile generare numerose informazioni del log di traccia in un breve periodo di tempo, che può influisca sulle prestazioni del sistema, i log di traccia sono disabilitati per impostazione predefinita. 

### <a name="to-enable-and-view-the-trace-log"></a>Per abilitare e visualizzare il log di traccia
1.  Apri il Visualizzatore eventi
2.  Fare clic con il pulsante destro del mouse su registri **applicazioni e servizi** e selezionare Visualizza, quindi fare clic su Mostra **log analitici e di debug**.  Verrà visualizzato un nodo aggiuntivo a sinistra.
![miglioramenti del controllo](media/ad-fs-tshoot-logging/event2.PNG)  
3.  Espandi traccia AD FS
4.  Fare clic con il pulsante destro del mouse su debug e selezionare **Abilita log**.
![miglioramenti del controllo](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Informazioni sul controllo degli eventi per AD FS in Windows Server 2016  
Per impostazione predefinita, AD FS in Windows Server 2016 dispone di un livello di base di controllo abilitato.  Con il controllo di base, gli amministratori vedranno 5 o meno gli eventi per una singola richiesta.  Questa operazione contrassegna una riduzione significativa del numero di eventi che gli amministratori devono esaminare per poter visualizzare una singola richiesta.   Il livello di controllo può essere generato o abbassato usando il cmdlet di PowerShell:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

La tabella seguente illustra i livelli di controllo disponibili.  

|Livello di controllo|Sintassi di PowerShell|Descrizione|  
|----- | ----- | ----- |
|Nessuna|Set-AdfsProperties-AuditLevel None|Il controllo è disabilitato e non viene registrato alcun evento.|  
|Basic (impostazione predefinita)|Set-AdfsProperties-AuditLevel Basic|Non verranno registrati più di 5 eventi per una singola richiesta|  
|Dettagliato|Set-AdfsProperties-AuditLevel verbose|Tutti gli eventi verranno registrati.  Questa operazione registrerà una quantità significativa di informazioni per ogni richiesta.|  
  
Per visualizzare il livello di controllo corrente, è possibile usare il cmdlet di PowerShell:  Get-AdfsProperties.  
  
![miglioramenti del controllo](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
Il livello di controllo può essere generato o abbassato usando il cmdlet di PowerShell:  Set-AdfsProperties-AuditLevel.  
  
![miglioramenti del controllo](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>Tipi di eventi  
Gli eventi AD FS possono essere di tipi diversi, in base ai diversi tipi di richieste elaborate da AD FS. A ogni tipo di evento sono associati dati specifici.  Il tipo di eventi può essere differenziato tra le richieste di accesso (ad esempio, le richieste di token) e le richieste di sistema (chiamate server-server, incluso il recupero delle informazioni di configurazione).    

Nella tabella seguente vengono descritti i tipi di eventi di base.  
  
|Tipo evento|ID evento|Descrizione| 
|----- | ----- | ----- | 
|Convalida nuova credenziale riuscita|1202|Una richiesta in cui le nuove credenziali vengono convalidate correttamente dal Servizio federativo. Sono inclusi WS-Trust, WS-Federation, SAML-P (prima parte per generare SSO) e gli endpoint di autorizzazione OAuth.|  
|Nuovo errore di convalida delle credenziali|1203|Una richiesta in cui la convalida delle credenziali aggiornata non è riuscita nel Servizio federativo. Sono inclusi WS-Trust, WS-Fed, SAML-P (prima tappa per generare SSO) e gli endpoint di autorizzazione OAuth.|  
|Il token dell'applicazione è riuscito|1200|Una richiesta in cui un token di sicurezza viene emesso correttamente dal Servizio federativo. Per WS-Federation, SAML-P viene registrato quando la richiesta viene elaborata con l'artefatto SSO. come il cookie SSO.|  
|Errore del token dell'applicazione|1201|Richiesta in cui il rilascio del token di sicurezza non è riuscito nel Servizio federativo. Per WS-Federation, SAML-P viene registrato quando la richiesta è stata elaborata con l'artefatto SSO. come il cookie SSO.|  
|Richiesta di modifica della password riuscita|1204|Transazione in cui la richiesta di modifica della password è stata elaborata correttamente dal Servizio federativo.|  
|Errore di richiesta di modifica della password|1205|Transazione in cui la richiesta di modifica della password non è stata elaborata dal Servizio federativo.| 
|Disconnessione riuscita|1206|Descrive una richiesta di disconnessione riuscita.|  
|Errore di disconnessione|1207|Descrive una richiesta di disconnessione non riuscita.|  

## <a name="security-auditing"></a>Controllo della sicurezza
Il controllo di sicurezza dell'account del servizio AD FS può talvolta aiutare a tenere traccia dei problemi relativi agli aggiornamenti delle password, alla registrazione di richieste/risposte, alla richiesta di intestazioni contect e ai risultati della registrazione del dispositivo.  Il controllo dell'account del servizio AD FS è disabilitato per impostazione predefinita.

### <a name="to-enable-security-auditing"></a>Per abilitare il controllo della sicurezza
1. Fare clic sul pulsante Start, scegliere **programmi**, **strumenti di amministrazione**, quindi fare clic su **criteri di sicurezza locali**.
2. Passare alla cartella **Impostazioni sicurezza\Criteri locali\Assegnazione diritti utenti** e quindi fare doppio clic su **Generazione di controlli di sicurezza**.
3. Nella scheda **Impostazioni sicurezza locale** verificare che sia elencato l'account del servizio AD FS. Se non è presente, fare clic su Aggiungi utente o gruppo e aggiungerlo all'elenco, quindi fare clic su OK.
4. Aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente per abilitare il controllo di Auditpol. exe/set/SubCategory: "applicazione generata"/Failure: Enable/Success: Enable
5. Chiudere **criteri di sicurezza locali**e quindi aprire lo snap-in gestione ad FS.
 
Per aprire lo snap-in gestione AD FS, fare clic sul pulsante Start, scegliere programmi, strumenti di amministrazione, quindi fare clic su Gestione AD FS.
 
6. Nel riquadro azioni fare clic su Modifica proprietà Servizio federativo
7. Nella finestra di dialogo Proprietà Servizio federativo fare clic sulla scheda eventi.
8. Selezionare le caselle di controllo **Operazioni riuscite** e **Operazioni non riuscite**.
9. Fare clic su OK.

![miglioramenti del controllo](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>Le istruzioni sopra riportate vengono utilizzate solo quando AD FS si trova in un server membro autonomo.  Se AD FS è in esecuzione in un controller di dominio, anziché i criteri di sicurezza locali, utilizzare il **criterio controller di dominio predefinito** situato in **criteri di gruppo gestione/foresta/domini/controller di dominio**.  Fare clic su modifica e passare a **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri locali\assegnazione Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Messaggi Windows Communication Foundation e Windows Identity Foundation
Oltre alla registrazione della traccia, a volte potrebbe essere necessario visualizzare i messaggi Windows Communication Foundation (WCF) e Windows Identity Foundation (WIF) per risolvere un problema. Questa operazione può essere eseguita modificando il file **Microsoft. IdentityServer. ServiceHost. exe. config** nel server ad FS. 

Il file si trova in **<% System Root% > \Windows\ADFS** ed è in formato XML. Di seguito sono elencate le parti rilevanti del file: 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Dopo aver applicato queste modifiche, salvare la configurazione e riavviare il servizio AD FS. Dopo aver abilitato queste tracce impostando le opzioni appropriate, verranno visualizzate nel registro di traccia AD FS nel Visualizzatore eventi di Windows.

## <a name="correlating-events"></a>Correlazione di eventi
Uno degli aspetti più difficili da risolvere è costituito da problemi di accesso che generano un numero elevato di eventi di errore o di debug.

Per semplificare questa operazione, AD FS mette in correlazione tutti gli eventi registrati nel Visualizzatore eventi, sia nei log di amministratore che in quelli di debug, che corrispondono a una determinata richiesta utilizzando un identificatore univoco globale (GUID) univoco denominato ID attività. Questo ID viene generato quando la richiesta di rilascio di token viene presentata inizialmente all'applicazione Web (per le applicazioni che usano il profilo del richiedente passivo) o alle richieste inviate direttamente al provider di attestazioni (per le applicazioni che usano WS-Trust). 

![ActivityId](media/ad-fs-tshoot-logging/activityid1.png)

Questo ID attività rimane invariato per tutta la durata della richiesta e viene registrato come parte di ogni evento registrato nel Visualizzatore eventi per la richiesta. Ciò significa che:
 - il filtraggio o la ricerca nella Visualizzatore eventi usando questo ID attività può essere utile per tenere traccia di tutti gli eventi correlati che corrispondono alla richiesta di token
 - lo stesso ID attività viene registrato tra computer diversi che consentono di risolvere i problemi relativi a una richiesta utente tra più computer, ad esempio il proxy server federativo (FSP)
 - Se la richiesta AD FS ha esito negativo, l'ID attività verrà visualizzato anche nel browser dell'utente, in modo da consentire all'utente di comunicare questo ID al help desk o al supporto IT.

![ActivityId](media/ad-fs-tshoot-logging/activityid2.png)

Per facilitare il processo di risoluzione dei problemi, AD FS registra anche l'evento ID chiamante ogni volta che il processo di rilascio del token ha esito negativo in un server AD FS. Questo evento contiene il tipo di attestazione e il valore di uno dei tipi di attestazione seguenti, presupponendo che queste informazioni siano state passate al Servizio federativo come parte di una richiesta di token:
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

L'evento ID chiamante registra anche l'ID attività per consentire l'uso di tale ID attività per filtrare o cercare i registri eventi per una determinata richiesta.




## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)
