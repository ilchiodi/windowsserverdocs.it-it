---
title: Novità di Desktop remoto su Mac?
description: Informazioni sulle modifiche recenti per il client Desktop remoto per Mac
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
ms.openlocfilehash: d0cf81f24374e81ca28c2d2cfd83a394e096706c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844792"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>Novità per il client Desktop remoto in macOS?

Viene aggiornato regolarmente le [client Desktop remoto per macOS](remote-desktop-mac.md), aggiungendo nuove funzionalità e risoluzione dei problemi. Scopri gli aggiornamenti più recenti riportato di seguito.

Se si verificano problemi, è possibile sempre contattateci **aiutare > segnala un problema**.

## <a name="updates-for-version-1029"></a>Aggiornamenti per la versione 10.2.9
*Data di pubblicazione: 3/6/2019*

- In questa versione che è stato risolto un problema di connettività di gateway desktop remoto che può verificarsi quando si richiede il reindirizzamento di server inserire.
- Abbiamo anche risolto una regressione di gateway desktop remoto causata dal 10.2.8 update.

## <a name="updates-for-version-1028"></a>Aggiornamenti per la versione 10.2.8
*Data di pubblicazione: 3/1/2019*

- Risolvere i problemi di connettività che viene rilevato quando si usa un Gateway Desktop remoto.
- Correzione di avvisi relativi al certificato corretto che sono stati visualizzati durante la connessione.
- Risolti alcuni casi in cui la barra dei menu e il dock inutilmente nasconderà durante l'avvio remoto di App.
- Rielaborare il codice di reindirizzamento negli Appunti da arresti anomali di indirizzo che sono stati casi alcuni utenti.
- Risolto un bug che ha causato il centro di connessione scorrere inutilmente quando si avvia una connessione.

## <a name="updates-for-version-1027"></a>Aggiornamenti per la versione 10.2.7
*Data di pubblicazione: 2/6/2019*

- In questa versione sono risolti mispaints grafica che venivano visualizzate quando si usa la modalità AVC444 (causato da un bug di codifica server).

## <a name="updates-for-version-1026"></a>Aggiornamenti per la versione 10.2.6
*Data di pubblicazione: 1/28/2019*

- Aggiunta del supporto per codec AVC (420 e 444), disponibile quando ci si connette alle versioni correnti di Windows 10.
- In adatta alla modalità di finestra, finestra ora si verifica un aggiornamento immediatamente dopo un ridimensionamento per verificare che il contenuto viene eseguito il rendering a livello di interpolazione corretto.
- Risolto un bug di layout che hanno causato le intestazioni feed sovrapporsi ad alcuni utenti.
- Pulizia l'interfaccia utente alle preferenze dell'applicazione.
- Polished l'interfaccia utente del Desktop di aggiunta/modifica.
- Apportati numerosi semplice e ben definita alle viste di riquadro e l'elenco di centro di connessione per desktop e i feed di modifiche.

>[!NOTE]
>È presente un bug nella cartella di macOS 10.14.0 e 10.14.1 che possono causare ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (annidato profonde all'interno della cartella ~/Library) per utilizzare una grande quantità di spazio su disco. Per risolvere questo problema, eliminare il contenuto della cartella e l'aggiornamento a macOS 10.14.2. Si noti che un effetto collaterale di eliminare il contenuto della cartella che verranno eliminate le immagini di snapshot assegnate ai segnalibri. Queste immagini verranno rigenerate durante la riconnessione al computer remoto.

## <a name="updates-for-version-1024"></a>Aggiornamenti per la versione 10.2.4
*Data di pubblicazione: 12/18/2018*

- Aggiunto il supporto della modalità scura per macOS Mojave 10.14.
- Se è vuota, viene visualizzata un'opzione per importare da Microsoft Remote Desktop 8 ora nel Centro connessioni.
- Compatibilità di reindirizzamento cartelle indirizzata con alcune applicazioni aziendali di terze parti.
- Risolvere i problemi in cui gli utenti sono stati ricevere un errore di Gateway Desktop remoto 0x30000069 a causa di sicurezza i problemi di fallback di protocollo.
- Problemi di rendering progressivo fissi che alcuni utenti sono stati verificati con adatta alla modalità di finestra.
- Risolto un bug che impediva la copia del file e incollare da copiando la versione più recente di un file.
- Migliorata basata su mouse lo scorrimento per i delta di scorrimento di piccole dimensioni.

## <a name="updates-for-version-1023"></a>Aggiornamenti per la versione 10.2.3
*Data di pubblicazione: 11/06/2018*

- Aggiunta del supporto per l'impostazione del file RDP "remoteapplicationcmdline" per gli scenari di app remota.
- Il titolo della finestra della sessione include ora il nome del file RDP (e nome del server) quando avviata da un file RDP.
- Correzione di problemi delle prestazioni del gateway segnalata desktop remoto.
- Arresto anomalo fisso segnalate gateway di desktop remoto.
- Risoluzione dei problemi in cui la connessione si blocca durante la connessione tramite un gateway desktop remoto.
- Migliore gestione delle App remote a schermo intero in modo intelligente nascondere la barra dei menu e ancoraggio.
- Correzione di scenari in cui App remote sono rimaste nascoste dopo essere stato avviato.
- Risolti gli aggiornamenti per il rendering lento quando si usa "Adatta alla finestra" con l'accelerazione hardware disabilitato.
- Gestione degli errori di creazione database causati da autorizzazioni non corrette quando il client di avvio. 
- Risolto un problema in cui il client è stato in modo coerente in modo anomalo all'avvio e non a partire da alcuni utenti.
- Correzione di uno scenario in cui le connessioni sono state importate correttamente come schermo da 8 di Desktop remoto.

## <a name="updates-for-version-1022"></a>Aggiornamenti per la versione 10.2.2
*Data di pubblicazione: 10/09/2018*

- Un nuovissimo centro di connessione che supporta trascinamento della selezione, disposizione manuale dei computer desktop, le colonne ridimensionabili in modalità di visualizzazione elenco, basata su colonne di ordinamento e semplificare la gestione di gruppo.
- Centro connessioni ora ricorda l'ultima pivot active (desktop o feed) quando la chiusura dell'app.
- Le credenziali che richiede l'interfaccia utente e i flussi, sono state modificate.
- Commenti e suggerimenti di Gateway Desktop remoto fa ora parte dello stato dell'interfaccia utente che esegue la connessione.
- Importazione delle impostazioni del client versione 8 è stata migliorata.
- I file RDP che punta all'endpoint di RemoteApp possono ora essere importati in Centro connessioni.
- Ottimizzazioni di display retina per gli scenari di Desktop remoto singolo monitor.
- Supporto per specificare il livello di interpolazione di grafica (che influisce sulla sfocatura) se non si utilizza ottimizzazioni Retina.
- supporto di 256 colori per abilitare la connettività a Windows 2000.
- Correzione di ritaglio dei bordi inferiore e destro dello schermo quando ci si connette a Windows 7, Windows Server 2008 R2 e versioni precedenti.
- Copia un file locale in Outlook (in esecuzione in una sessione remota) ora aggiunto il file come allegato.
- Risolto un problema che è stata rallentare i trasferimenti di file basato su spazio di lavoro se i file ha origine da una condivisione di rete.
- Risolto un bug che impediva a Excel (in esecuzione in una sessione remota) bloccarsi durante il salvataggio in un file in una cartella reindirizzata.
- Risolto un problema che non causava l'alcun spazio sufficiente per eseguire il report per le cartelle reindirizzate.
- Risolto un bug che ha causato le anteprime di utilizzare una quantità eccessiva archiviazione su disco in macOS 10.14.
- Aggiunta del supporto per l'applicazione di criteri di reindirizzamento di dispositivi di Gateway Desktop remoto.
- Risolto un problema che impediva finestre della sessione di chiusura durante la disconnessione da una connessione tramite Gateway Desktop remoto.
- Se l'autenticazione a livello di rete (NLA) non viene applicata dal server, verrà a questo punto verranno indirizzati alla schermata di accesso se la password è scaduta.
- Risolto i problemi di prestazioni rilevati quando è stato in corso una grande quantità di dati trasferiti nella rete.
- Correzioni di reindirizzamento della smart card.
- Supporto per tutti i valori possibili del "Livello di autenticazione" e "EnableCredSspSupport" impostazioni del file RDP se la chiave predefinita utente ClientSettings.EnforceCredSSPSupport (nel dominio com.microsoft.rdc.macos) è impostata su 0.
- Supporto per l'impostazione del file RDP "Dei messaggi di richiesta per le credenziali sul Client" se NLA non viene negoziata.
- Supporto per l'accesso basato su smart card mediante il reindirizzamento della smart card al prompt di Winlogon quando non viene negoziata NLA.
- Risolto un problema che impediva il download di risorse che hanno spazi nell'URL del feed.

## <a name="updates-for-version-1021"></a>Aggiornamenti per la versione 10.2.1
*Data di pubblicazione: 08/06/2018*

- PC aggiunti connettività abilitata per Azure Active Directory (AAD). Per connettersi a un PC aggiunto a Azure ad, il nome utente deve essere in uno dei formati seguenti: "AzureAD\user" o "AzureAD\user@domain".
- Risolti alcuni bug che interessano l'utilizzo di smart card in una sessione remota.

## <a name="updates-for-version-1020"></a>Aggiornamenti per la versione 10.2.0
*Data di pubblicazione: 07/24/2018*

- Aggiornamenti integrati per conformità al GDPR.
- MicrosoftAccount\username@domain ora è accettato come un nome utente valido.
- Condivisione degli Appunti è stato riscritto per essere più veloce e supportare altri formati.
- Copia e Incolla di testo, immagini o i file tra le sessioni a questo punto viene ignorato negli Appunti del computer locale.
- È ora possibile connettersi tramite un server Gateway Desktop remoto con un certificato non attendibile (se si accettano i messaggi di avviso).
- L'accelerazione hardware bare metal viene ora usato (se supportato) per velocizzare il rendering e ottimizzare l'utilizzo della batteria.
- Quando si usa l'accelerazione hardware bare Metal che è provare a usare alcuni magic per apportare la grafica della sessione vengono visualizzati più nitida.
- È stato eliminare alcune istanze in cui windows si blocca dopo la chiusura.
- Bug risolti che sono stati impedendo l'avvio di programmi di RemoteApp in alcuni scenari.
- Correzione di un errore di sincronizzazione del canale di Gateway Desktop remoto che è stato conseguente 0x204 errori.
- La forma del cursore del mouse ora aggiorna correttamente quando lo spostamento all'esterno di una sessione o una finestra di RemoteApp.
- Risolto un bug di reindirizzamento cartelle che causa perdita di dati quando copiare e incollare le cartelle.
- Risolto un problema di reindirizzamento cartelle che ha causato corretta segnalazione delle dimensioni cartella.
- Correzione di una regressione che è stata impedire la registrazione in un computer aggiunto al AAD con un account locale.
- Bug risolti che sono stati causando la sessione di contenuto della finestra da ritagliare.
- Aggiunta del supporto per i certificati di endpoint di desktop remoto che contengono le chiavi asimmetriche a curva ellittica.
- Risolto un bug che è stata impedisce il download delle risorse gestite in alcuni scenari.
- Risolto un problema di ridimensionamento con il centro di connessione aggiunto.
- Risolto le caselle di controllo nella finestra delle proprietà di visualizzazione per migliorare la collaborazione.
- Bloccare proporzioni è disabilitata quando è effettiva la modifica della visualizzazione dinamica.
- Risolvere i problemi di compatibilità con l'infrastruttura di F5.
- Aggiornare la gestione di password vuote per assicurarsi che i messaggi corretti vengono visualizzati in fase di connessione.
- Lo scorrimento di problemi di compatibilità con MapInfra Pro mouse fisso.
- Risolti alcuni problemi di allineamento al centro di connessione durante l'esecuzione in Mojave.

## <a name="updates-for-version-1018"></a>Aggiornamenti per la versione 10.1.8
*Data di pubblicazione: 05/04/2018*

- Aggiunta del supporto per la modifica della risoluzione remota ridimensionando la finestra della sessione.
- Fissi scenari in cui risorsa remota feed download richiederebbe troppo tempo.
- Risolvere l'errore 0x207 che poteva verificarsi durante la connessione al server non corretto con l'aggiornamento di monitoraggio e aggiornamento oracle crittografia CredSSP (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Aggiornamenti per la versione 10.1.7
*Data di pubblicazione: 04/05/2018*

- State apportate correzioni di sicurezza per incorporare CredSSP crittografia oracle correzione degli aggiornamenti, come descritto in CVE-2018-0886.
- RemoteApp icona e un mouse cursore rendering migliorato per soddisfare mispaints segnalate.
- Risolvere i problemi in cui windows RemoteApp è apparsa dietro il centro di connessione.
- Risolto un problema che si sono verificati quando si modificano le risorse locali dopo l'importazione da 8 di Desktop remoto.
- È ora possibile avviare una connessione premendo INVIO su un riquadro del desktop.
- Quando si è in modalità schermo intero, CMD + M ora esegue correttamente il mapping a WIN + M.
- Il Centro connessioni, le preferenze e su windows ora rispondere a CMD + M.
- È ora possibile avviare l'individuazione dei feed premendo INVIO **aggiunta di risorse Remote** pagina.
- Risolto un problema in cui un nuovo feed risorse remote ha bussato vuoto nel Centro connessioni fino a dopo che è stato aggiornato.
 
## <a name="updates-for-version-1016"></a>Aggiornamenti per la versione 10.1.6
*Data di pubblicazione: 03/26/2018*

- Risolto un problema in cui windows RemoteApp potrebbe riordinare autonomamente.
- Risolto un bug che provocava alcune finestre di RemoteApp a si blocca dietro la finestra padre.
- Risolto un problema di offset puntatore del mouse che interessava alcuni programmi di RemoteApp.
- Risolto un problema in cui avviare una nuova connessione assegnato lo stato attivo a una sessione esistente, invece di aprire una finestra nuova sessione.
- È stato risolto un errore con un messaggio di errore: vediamo il messaggio corretto se il gateway non è stato trovato.
- Il collegamento Quit (⌘ + Q) è ora visualizzato in modo coerente nell'interfaccia utente.
- Migliorata la qualità dell'immagine quando l'estensione in modalità "Adatta alla finestra".
- Correzione di una regressione che ha causato più istanze della home directory compaia nella sessione remota.
- Aggiornare l'icona predefinita per i riquadri del desktop.
