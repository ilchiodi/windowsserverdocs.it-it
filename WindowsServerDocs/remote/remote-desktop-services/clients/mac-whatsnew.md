---
title: Novità per Desktop remoto su Mac?
description: Informazioni sulle modifiche recenti al client Desktop remoto per Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 03/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 11da8bc333da7f44b2732266837d2df216142f85
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279122"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>Novità del client Desktop remoto su macOS?

Aggiorniamo regolarmente il [client Desktop remoto per macOS](remote-desktop-mac.md), aggiungendo nuove funzionalità e risoluzione dei problemi. Scopri gli aggiornamenti più recenti seguenti.

Se si verificano problemi, puoi sempre contattarci tramite **Guida gt _ Report un problema**.

## <a name="updates-for-version-10210"></a>Aggiornamenti per la versione 10.2.10
*Data di pubblicazione: 30/3/2019*

- In questa versione abbiamo risolto instabilità causata dall'aggiornamento di macOS 10.14.4 recenti. Abbiamo anche fisso mispaints Cancella quando decodificano i dati di codec AVC codificati da un server con hardware NVIDIA.

## <a name="updates-for-version-1029"></a>Aggiornamenti per la versione 10.2.9
*Data di pubblicazione: 6/3/2019*

- In questa versione che abbiamo risolto un problema di connettività di gateway desktop remoto che possa verificarsi quando esegue il reindirizzamento server posiziona.
- Abbiamo risolto anche una regressione gateway desktop remoto causata da 10.2.8 l'aggiornamento.

## <a name="updates-for-version-1028"></a>Aggiornamenti per la versione 10.2.8
*Data di pubblicazione: 3/1/2019*

- Risolvere i problemi di connettività che visibili quando si utilizza un Gateway Desktop remoto.
- Avvisi del certificato corretto fissa che sono stati visualizzati durante la connessione.
- Risolto alcuni casi in cui la barra dei menu e dock inutilmente nasconderà durante l'avvio di App remota.
- Rielaborare il codice di reindirizzamento degli Appunti di arresti anomali indirizzo che sono stati casi alcuni utenti.
- Fissa un bug che ha causato il centro di connessione inutilmente Scorri fino all'apertura di una connessione.

## <a name="updates-for-version-1027"></a>Aggiornamenti per la versione 10.2.7
*Data di pubblicazione: 2/6/2019*

- In questa versione abbiamo risolto mispaints grafica (causato da un bug di codifica server) che è stato visualizzato quando si usa la modalità AVC444.

## <a name="updates-for-version-1026"></a>Aggiornamenti per la versione 10.2.6
*Data di pubblicazione: 28/1/2019*

- Aggiunta del supporto per il codec AVC (420 e 444), disponibile quando ti Connetti a versioni correnti di Windows 10.
- In adatta alla modalità finestra, un aggiornamento della finestra si verifica ora immediatamente dopo un ridimensionamento per garantire che il contenuto viene eseguito il rendering a livello di interpolazione corretto.
- Fissa un bug di layout che ha causato feed intestazioni la sovrapposizione per alcuni utenti.
- Pulizia l'interfaccia utente delle preferenze di applicazione.
- Impreziosisce l'interfaccia utente di Desktop aggiunta/modifica.
- Resi grandi quantità di adattarsi e completare le rettifiche per le visualizzazioni di riquadro ed elenco connessione Centro per i desktop e feed.

>[!NOTE]
>Esiste un bug nella cartella macOS 10.14.0 e 10.14.1 che possono causare ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (annidato profondo all'interno della cartella ~/Library) per l'utilizzo di una grande quantità di spazio su disco. Per risolvere questo problema, eliminare il contenuto della cartella e l'aggiornamento a macOS 10.14.2. Tieni presente che un effetto collaterale di eliminare il contenuto della cartella è che le immagini di snapshot assegnate ai segnalibri verranno eliminate. Queste immagini verranno rigenerazione durante la riconnessione al PC remoto.

## <a name="updates-for-version-1024"></a>Aggiornamenti per la versione 10.2.4
*Data di pubblicazione: 12/18/2018*

- Supporto modalità scura per macOS Mojave 10.14 aggiunto.
- Un'opzione per importare da 8 Desktop remoto Microsoft ora visualizzate al centro di connessione se è vuoto.
- Risolto compatibilità di reindirizzamento cartelle con alcune applicazioni aziendali di terze parti.
- Risolvere i problemi in cui gli utenti sono stati ricevere un errore di Gateway Desktop remoto 0x30000069 a causa di sicurezza problemi di fallback protocollo.
- Alcuni utenti erano verificati con problemi di rendering progressivo fissa adattano alla modalità finestra.
- Fissa un bug che ha impedito copia i file e incollare informazioni da copiare la versione più recente di un file.
- Miglioramento basati su mouse lo scorrimento per delta di scorrimento.

## <a name="updates-for-version-1023"></a>Aggiornamenti per la versione 10.2.3
*Data di pubblicazione: 06/11/2018*

- Aggiunta del supporto per l'impostazione di file RDP "remoteapplicationcmdline" per gli scenari di app remota.
- Il titolo della finestra di sessione include ora il nome del file RDP (e il nome del server) all'avvio da un file RDP.
- Risolto problemi di prestazioni di gateway desktop remoto segnalato.
- Fissa gateway desktop remoto segnalati arresti anomali.
- Problemi risolti in cui la connessione sarebbe blocco quando ci si connette tramite un gateway desktop remoto.
- Migliore gestione delle App remota a schermo intero, in modo intelligente nascondendo la barra dei menu e dock.
- Fissi scenari in cui le app remote è rimasto nascoste dopo l'avvio.
- Risolto aggiornamenti del rendering lento quando si usa "Adatta a finestra" con accelerazione hardware disabilitato.
- Gestione degli errori di creazione di database causati da autorizzazioni non corrette quando si avvia il client. 
- Risolto un problema in cui il client è l'arresto anomalo in modo coerente all'avvio e l'avvio non per alcuni utenti.
- Fissa uno scenario in cui le connessioni sono state importate in modo non corretto da 8 di Desktop remoto a schermo intero.

## <a name="updates-for-version-1022"></a>Aggiornamenti per la versione 10.2.2
*Data di pubblicazione: 09/10/2018*

- Un nuovo centro di connessione che supporta trascinamento della selezione, la disposizione manuale dei computer desktop, le colonne ridimensionabili in modalità di visualizzazione elenco, in base alle colonne ordinamento e semplificare la gestione di gruppo.
- Il centro di connessione ricorda ora l'ultimo elemento pivot attivo (desktop o feed) quando la chiusura dell'app.
- La credenziale chiedere conferma dell'interfaccia utente e i flussi sono state modificate.
- Feedback Gateway Desktop remoto è ora parte dello stato di connessione dell'interfaccia utente.
- Importa le impostazioni dal client versione 8 è stato migliorato.
- File RDP che punta all'endpoint RemoteApp possono ora essere importati nel centro di connessione.
- Ottimizzazioni di visualizzazione retina per gli scenari di Desktop remoto singolo monitor.
- Supporto per specificare il livello di interpolazione grafica (con effetti di sfocatura) quando non si utilizza ottimizzazioni Retina.
- supporto di 256 colori per abilitare la connettività a Windows 2000.
- Fisso ritaglio dei bordi destro e inferiore dello schermo quando ti Connetti a Windows 7, Windows Server 2008 R2 e versioni precedenti.
- Copiare un file locale in Outlook (in esecuzione in una sessione remota) ora aggiunge il file come allegato.
- Risolto un problema che è stata rallentare trasferimenti di file in base tavolo se i file ha origine da una condivisione di rete.
- Risolto un bug che provoca il blocco durante il salvataggio in un file su una cartella reindirizzata in Excel (in esecuzione in una sessione remota).
- Risolto un problema che non provoca alcun spazio libero venga segnalato per le cartelle reindirizzate.
- Fissa un bug che ha causato le anteprime per l'utilizzo eccessivo archiviazione su disco in macOS 10.14.
- Aggiunta del supporto per l'applicazione di criteri di reindirizzamento dispositivo Gateway Desktop remoto.
- È stato risolto un problema che ha impedito windows sessione chiusura durante la disconnessione da una connessione tramite Gateway Desktop remoto.
- Se l'autenticazione a livello di rete (NLA) non viene applicato dal server, verranno ora indirizzati alla schermata di accesso se la password è scaduta.
- Risolto problemi di prestazioni che visibili quando è stato trasferiti grandi quantità di dati in rete.
- Correzioni di reindirizzamento delle smart card.
- Supporto per tutti i possibili valori del "livello di autenticazione di" e "EnableCredSspSupport" le impostazioni RDP file se la chiave predefinita utente ClientSettings.EnforceCredSSPSupport (nel dominio com.microsoft.rdc.macos) è impostata su 0.
- Supporto per l'impostazione di file RDP "Richiedi per le credenziali nel Client" quando non viene negoziata NLA.
- Supporto per l'accesso di smart card basate su tramite il reindirizzamento delle smart card al prompt di Winlogon quando non viene negoziata NLA.
- Risolto un problema che impediva download feed risorse con spazi nell'URL.

## <a name="updates-for-version-1021"></a>Aggiornamenti per la versione 10.2.1
*Data di pubblicazione: 08/06/2018*

- Abilitata la connettività di Azure Active Directory (AAD) aggiunti a un PC. Per connettersi a un AAD aggiunti a un PC, il nome utente deve essere in uno dei formati seguenti: "AzureAD\user" o "AzureAD\user@domain".
- Risolto alcuni bug influenzare l'utilizzo della smart card in una sessione remota.

## <a name="updates-for-version-1020"></a>Aggiornamenti per la versione 10.2.0
*Data di pubblicazione: 24/07/2018*

- Incorporati gli aggiornamenti per la conformità RGPD.
- MicrosoftAccount\username@domaina questo punto vengono accettati come un nome utente valido.
- Condivisione degli Appunti è stato riscritto per essere più veloce e supportare ulteriori formati.
- Copiare e incollare testo, immagini o file tra diverse sessioni dell'ora consente di ignorare gli Appunti del computer locale.
- È ora possibile connettersi tramite un server Gateway Desktop remoto con un certificato non attendibile (se accettare i messaggi di avviso).
- L'accelerazione hardware Metal viene ora usato per velocizzare il rendering e ottimizzare l'utilizzo della batteria (se supportato).
- Quando si usa l'accelerazione hardware Metal che si tenta di utilizzare alcuni magia per rendere la grafica di sessione risultano più nitida.
- Hai rid di alcuni casi in cui windows sarebbe blocco dopo aver chiuso.
- Bug fisso che sono stati impedire l'avvio di programmi RemoteApp in alcuni scenari.
- Fissa un errore di sincronizzazione di canale Gateway Desktop remoto che è stato generando 0x204 errori.
- La forma del cursore del mouse ora aggiorna correttamente quando si passa da una sessione o della finestra RemoteApp.
- Fissa un bug di reindirizzamento cartelle che provoca la perdita di dati quando copiare e incollare cartelle.
- È stato risolto un problema di reindirizzamento cartella che ha causato la segnalazione errata di dimensioni di cartella.
- Fissa una regressione che è stata impedendo l'accesso a un computer aggiunto al AAD con un account locale.
- Bug fisso che sono stati causando la sessione di contenuto della finestra essere ritagliato.
- Aggiunta del supporto per i certificati di endpoint di desktop remoto che contengono le chiavi asimmetriche ellittico curva.
- Fissa un bug che è stata impedire il download di risorse gestite in alcuni scenari.
- Risolto un problema di ritaglio con il centro di connessione bloccati.
- Fissa le caselle di controllo nella finestra delle proprietà Display da collaborano.
- Il blocco proporzioni è ora disabilitato quando cambia visualizzazione dinamica è attiva.
- Risolto problemi di compatibilità con F5 infrastruttura.
- Aggiornare la gestione delle password vuota per garantire che i corretti messaggi vengono visualizzati in fase di connettersi.
- Mouse fisso problemi di compatibilità con MapInfra Pro di scorrimento.
- Fissa alcuni problemi di allineamento al centro di connessione durante l'esecuzione su Mojave.

## <a name="updates-for-version-1018"></a>Aggiornamenti per la versione 10.1.8
*Data di pubblicazione: 05/04/2018*

- Aggiunta del supporto per la modifica della risoluzione remota ridimensionando la finestra di sessione!
- Fissi scenari in cui le risorse remote feed download richiederebbe un tempo eccessivamente lungo.
- Risolto l'errore 0x207 che potrebbe verificarsi durante la connessione ai server con l'aggiornamento di correzione oracle crittografia CredSSP (CVE-2018-0886) non installati.

## <a name="updates-for-version-1017"></a>Aggiornamenti per la versione 10.1.7
*Data di pubblicazione: 05/04/2018*

- Resi correzioni per la sicurezza per incorporare gli aggiornamenti di correzione CredSSP crittografia oracle come descritto in CVE-2018-0886.
- Miglioramento RemoteApp icona e un mouse cursore rendering per soddisfare le mispaints segnalati.
- Risolto problemi in cui windows RemoteApp era visualizzata dietro il centro di connessione.
- Risolto un problema che si è verificato durante la modifica di risorse locali al termine dell'importazione da 8 di Desktop remoto.
- È ora possibile avviare una connessione premendo INVIO su un riquadro desktop.
- Quando sei in modalità schermo intero, CMD + M ora correttamente corrisponde al WIN + M.
- Il centro di connessione, le preferenze e informazioni su windows ora rispondere alle CMD + M.
- Ora puoi iniziare l'individuazione di feed premendo INVIO nella pagina **Aggiunta di risorse Remote** .
- Risolto un problema in cui un nuovo feed di risorse remote illustrava vuoto al centro di connessione fino a dopo che un aggiornamento.
 
## <a name="updates-for-version-1016"></a>Aggiornamenti per la versione 10.1.6
*Data di pubblicazione: 26/03/2018*

- Risolto un problema in cui windows RemoteApp sarebbe riordinare se stessi.
- Risolvere un bug che ha causato qualche windows RemoteApp di impediscono dietro la finestra padre.
- Risolto un problema di offset del puntatore del mouse che alcuni programmi RemoteApp interessati.
- Risolto un problema in cui avviare una nuova connessione assegnato lo stato attivo a una sessione esistente, invece di aprire una finestra nuova sessione.
- Abbiamo risolto un errore con un messaggio di errore: verrà visualizzato il messaggio corretto ora se Impossibile trovare il gateway.
- Il collegamento Esci (⌘ + Q) in modo coerente viene visualizzato nell'interfaccia utente.
- Migliorare la qualità delle immagini quando il stretching in modalità "Adatta alla finestra".
- Fissa una regressione che ha causato più istanze della cartella principale a comparire nella sessione remota.
- Aggiornamento dell'icona predefinita per i riquadri desktop.
