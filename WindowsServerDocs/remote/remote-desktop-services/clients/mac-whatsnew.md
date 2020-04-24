---
title: Novità del client macOS
description: Informazioni sulle modifiche recenti apportate al client Desktop remoto per Mac
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/08/2020
ms.localizationpriority: medium
ms.openlocfilehash: c378d8c4a87b6aa0cf4f6b4f30f3bd5524dbb7a9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80980853"
---
# <a name="whats-new-in-the-macos-client"></a>Novità del client macOS

Il [client Desktop remoto per macOS](remote-desktop-mac.md) viene aggiornato regolarmente, con l'aggiunta di nuove funzionalità e la correzione dei problemi. Qui sono disponibili gli aggiornamenti più recenti.

In caso di problemi, puoi sempre contattarci passando a **Guida** > **Segnala un problema**.

## <a name="updates-for-version-1039"></a>Aggiornamenti per la versione 10.3.9

*Data di pubblicazione: 6/4/2020*

In questa versione sono state apportate alcune modifiche per migliorare l'interoperabilità con il [servizio Desktop virtuale Windows](https://azure.microsoft.com/services/virtual-desktop/). Sono stati inoltre inclusi gli aggiornamenti seguenti:

- CTRL+OPZIONE+CANC attiva ora la sequenza CTRL+ALT+CANC (in precedenza era necessario premere il tasto Fn).
- È stata corretta la combinazione di colori per la notifica in modalità tastiera per la modalità chiara.
- Sono stati risolti scenari in cui le connessioni avviate tramite la proprietà del file RDP GatewayAccessToken non funzionavano.

>[!NOTE]
>Questa è l'ultima versione compatibile con macOS 10.12.

## <a name="updates-for-version-1038"></a>Aggiornamenti per la versione 10.3.8

*Data di pubblicazione: 12/2/2020*

È il momento del primo rilascio del 2020.

Con questo aggiornamento puoi alternare le modalità Scancode (CTRL+COMANDO+K) e Unicode (CTRL+COMANDO+U) quando attivi l'input da tastiera. La modalità Unicode consente la digitazione di caratteri estesi usando il tasto OPZIONE su una tastiera Mac. Ad esempio, su una tastiera Mac USA premendo OPZIONE+2 viene immesso il simbolo di marchio registrato (&trade;). Nella modalità Unicode puoi immettere anche caratteri accentati. Ad esempio, su una tastiera Mac USA premendo contemporaneamente OPZIONE+E e il tasto "A", viene immesso il carattere "á" nella sessione remota.

Tra gli altri aggiornamenti disponibili in questa versione sono inclusi i seguenti:

- È stata eseguita la pulizia dell'interfaccia utente e dell'esperienza e di aggiornamento dell'area di lavoro.
- È stato risolto un problema di reindirizzamento delle smart card per cui la sessione remota si bloccava nella schermata di accesso con la visualizzazione del messaggio di verifica dello stato.
- È stato ridotto il tempo di creazione di file temporanei usati per copiare e incollare file basati sugli Appunti.
- I file temporanei usati per copiare e incollare file degli Appunti vengono ora eliminati automaticamente all'uscita dall'app e non deve più essere macOS a eliminarli.
- Le azioni di segnalibro PC vengono ora visualizzate nell'angolo superiore destro delle anteprime.
- Sono state apportate correzioni per risolvere i problemi segnalati tramite la telemetria degli arresti anomali del sistema.

## <a name="updates-for-version-1037"></a>Aggiornamenti per la versione 10.3.7

*Data di pubblicazione: 06/01/2020*

Nell'ultimo aggiornamento dell'anno è stato ottimizzato il codice e sono stati corretti i comportamenti seguenti:

- La copia di elementi dalla sessione remota alla condivisione di rete o all'unità USB non crea più file vuoti.
- La specifica di una password vuota in un account utente non comporta più l'invio di una doppia richiesta di certificato.

## <a name="updates-for-version-1036"></a>Aggiornamenti per la versione 10.3.6

*Data di pubblicazione: 06/01/2020*

In questa versione, abbiamo risolto il problema che comportava la creazione di file di lunghezza zero per ogni azione di copia di una cartella dalla sessione remota al computer locale eseguendo l'operazione di copia e incolla del file.

## <a name="updates-for-version-1035"></a>Aggiornamenti per la versione 10.3.5

*Data di pubblicazione: 06/01/2020*

Abbiamo effettuato questo aggiornamento grazie alle segnalazioni ricevute. In questa versione, abbiamo apportato le modifiche seguenti:

- Le cartelle reindirizzate possono ora essere contrassegnate come di sola lettura per impedire la modifica del contenuto nella sessione remota.
- Abbiamo risolto un errore 0x607 che si verificava durante la connessione tramite RPC su scenari Gateway Desktop remoto HTTPS.
- Abbiamo corretto i casi in cui agli utenti venivano inviate due richieste di credenziali.
- Abbiamo corretto i casi in cui gli utenti ricevevano due volte la richiesta di avviso del certificato.
- Abbiamo aggiunto l'euristica per migliorare lo scorrimento basato su trackpad.
- Il client non visualizza più il gruppo "Saved Desktops" (Desktop salvati) se non sono presenti gruppi creati dall'utente.
- Abbiamo aggiornato l'interfaccia utente per i riquadri nella visualizzazione PC.
- Abbiamo corretto l'indirizzamento degli arresti anomali inviati a Microsoft tramite la telemetria dell'applicazione.

> [!NOTE]
> In questa versione, accettiamo feedback per il client Mac solo tramite [UserVoice](https://remotedesktop.uservoice.com/forums/287834-remote-desktop-for-mac).

## <a name="updates-for-version-1034"></a>Aggiornamenti per la versione 10.3.4

*Data di pubblicazione: 18/11/2019*

Dopo aver attentamente esaminato il feedback fornito dai clienti, abbiamo realizzato una serie di correzioni di bug e aggiornamenti di funzionalità.

- Quando ci si connette tramite un servizio Gateway Desktop remoto con l'autenticazione a più fattori, la connessione gateway verrà mantenuta aperta per evitare richieste multiple di autenticazione a più fattori.
- L'interfaccia utente client è ora completamente accessibile dalla tastiera con supporto per VoiceOver.
- I file copiati negli Appunti della sessione remota vengono ora trasferiti solo quando vengono incollati nel computer locale.
- Gli URL copiati negli Appunti della sessione remota vengono ora incollati correttamente nel computer locale.
- Il fattore di conversione della comunicazione remota per il supporto dei display Retina è ora disponibile per gli scenari a più monitor.
- È stato risolto un problema di compatibilità dei server Desktop remoto basati su FreeRDP, che causava problemi di connettività negli scenari di reindirizzamento.
- È stato affrontato il problema della compatibilità di reindirizzamento delle smart card nelle versioni future di Windows 10.
- È stato risolto un problema specifico di macOS 10.15 per cui lo spazio disponibile per le cartelle reindirizzate veniva segnalato in modo errato.
- Le connessioni PC pubblicate vengono rappresentate con una nuova icona nella scheda Aree di lavoro.
- Il termine "feed" è stato sostituito con "aree di lavoro" e il termine "desktop" con "PC".
- Sono stati corretti bug e incoerenze nella gestione degli account utente nell'interfaccia utente delle preferenze.
- Sono state introdotte numerose correzioni di bug per consentire un funzionamento più semplice e affidabile.

## <a name="updates-for-version-1033"></a>Aggiornamenti per la versione 10.3.3

*Data di pubblicazione: 18/11/2019*

Per la versione 10.3.3 è stato creato un aggiornamento delle funzionalità e sono stati corretti bug.

Sono state prima aggiunte le impostazioni predefinite utente per disabilitare la smart card, gli Appunti, il microfono, la fotocamera e il reindirizzamento delle cartelle:

- ClientSettings.DisableSmartcardRedirection
- ClientSettings.DisableClipboardRedirection
- ClientSettings.DisableMicrophoneRedirection
- ClientSettings.DisableCameraRedirection
- ClientSettings.DisableFolderRedirection

Sono state quindi introdotte le correzioni di bug:

- È stato risolto un problema che causava il mancato rilevamento dei ridimensionamenti della finestra di sessione a livello di codice.
- È stato risolto un problema per cui il contenuto della finestra di sessione risultava di dimensioni ridotte durante la connessione in modalità finestra (con visualizzazione dinamica abilitata).
- È stato corretto lo sfarfallio iniziale che si verificava durante la connessione a una sessione in modalità finestra con visualizzazione dinamica abilitata.
- Sono stati corretti i problemi di grafica che si verificavano durante la connessione a Windows 7 dopo l'attivazione e/o la disattivazione di Adatta alla finestra con visualizzazione dinamica abilitata.
- È stato corretto un bug che causava l'invio di un nome di dispositivo errato alla sessione remota (interrompendo il funzionamento delle licenze in alcune app di terze parti).
- È stato risolto un problema per cui le finestre delle app remote occupavano un intero monitor quando venivano ingrandite.
- È stato risolto un problema per cui l'interfaccia utente delle autorizzazioni di accesso veniva visualizzata al di sotto delle finestre locali.
- È stata eseguita la pulizia di parte del codice di arresto per garantire che la chiusura del client avvenga in modo più affidabile.

## <a name="updates-for-version-1032"></a>Aggiornamenti per la versione 10.3.2

*Data di pubblicazione: 18/11/2019*

In questa versione è stato corretto un bug che rendeva la visualizzazione a bassa risoluzione durante la connessione a una sessione.

## <a name="updates-for-version-1031"></a>Aggiornamenti per la versione 10.3.1

*Data di pubblicazione: 18/11/2019*

Sono state create alcune correzioni per risolvere le regressioni che erano riuscite a insinuarsi nella versione 10.3.0.

- Sono stati risolti i problemi di connettività dei server Gateway Desktop remoto che usavano chiavi asimmetriche a 4096 bit.
- È stato corretto un bug che causava l'interruzione della risposta da parte del client durante il download delle risorse di feed.
- È stato corretto un bug che causava l'arresto anomalo del client durante l'apertura.
- È stato corretto un bug che causava l'arresto anomalo del client durante l'importazione di connessioni da Desktop remoto versione 8.

## <a name="updates-for-version-1030"></a>Aggiornamenti per la versione 10.3.0

*Data di pubblicazione: 27/08/2019*

Sono trascorse alcune settimane dall'ultimo aggiornamento, ma il team ha lavorato con grande impegno in questo periodo. La versione 10.3.0 offre alcune nuove funzionalità e numerose correzioni "under the hood".

- È ora possibile reindirizzare la fotocamera al momento della connessione a Windows 10 1809, Windows Server 2019 e versioni successive.
- In Mojave e Catalina è stata aggiunta una nuova finestra di dialogo che richiede l'autorizzazione all'uso del microfono e della fotocamera per il reindirizzamento del dispositivo.
- Il flusso della sottoscrizione di feed è stato riscritto in modo da risultare più semplice e veloce.
- Il reindirizzamento degli Appunti include ora il formato RTF (Rich Text Format).
- Nell'area per l'immissione della password è disponibile una casella di controllo che consente di visualizzarla.
- Sono stati risolti gli scenari in cui la finestra della sessione passava da un monitor all'altro.
- In Connection Center (Centro connessioni) vengono visualizzate le icone di app remote con risoluzione elevata, se disponibili.
- La combinazione di tasti CMD+A è ora mappata a CTRL+A quando vengono usati i tasti di scelta rapida per gli Appunti Mac.
- La combinazione di tasti CMD+R consente ora di aggiornare tutti i feed sottoscritti.
- Sono state aggiunte nuove opzioni di clic secondarie per espandere o comprimere tutti i gruppi o i feed in Connection Center (Centro connessioni).
- È stata aggiunta una nuova opzione di clic secondaria per modificare le dimensioni dell'icona nella scheda dei feed in Connection Center (Centro connessioni).
- È stata introdotta una nuova icona di app più semplice e pulita.

## <a name="updates-for-version-10213"></a>Aggiornamenti alla versione 10.2.13

*Data di pubblicazione: 08/05/2019*

- Correzione di un blocco che si verificava durante la connessione tramite Gateway Desktop remoto.
- Aggiunta di un'informativa sulla privacy nella finestra di dialogo "Aggiungi feed".

## <a name="updates-for-version-10212"></a>Aggiornamenti alla versione 10.2.12

*Data di pubblicazione: 16/04/2019*

- Risoluzione delle disconnessioni casuali (con codice di errore 0x904) che si verificavano durante la connessione tramite Gateway Desktop remoto.
- Correzione di un bug a causa del quale l'elenco di risoluzioni nelle preferenze dell'applicazione era vuoto dopo l'installazione.
- Correzione di un bug che causava l'arresto anomalo del client in caso di aggiunta di alcune risoluzioni all'elenco di risoluzioni.
- Risoluzione di un ciclo infinito di richiesta di autenticazione ADAL durante la connessione alle distribuzioni di Desktop virtuale Windows.

## <a name="updates-for-version-10210"></a>Aggiornamenti alla versione 10.2.10

*Data di pubblicazione: 30/03/2019*

- In questa versione sono stati risolti i problemi di instabilità dovuti al recente aggiornamento a macOS 10.14.4. Sono stati corretti anche i problemi di visualizzazione in caso di decodifica di dati del codec AVC codificati da un server con hardware NVIDIA.

## <a name="updates-for-version-1029"></a>Aggiornamenti alla versione 10.2.9

*Data di pubblicazione: 06/03/2019*

- In questa versione è stato corretto un problema di connettività di Gateway Desktop remoto che si verificava durante il reindirizzamento del server.
- È stato risolto anche un problema di regressione di Gateway Desktop remoto causato dall'aggiornamento 10.2.8.

## <a name="updates-for-version-1028"></a>Aggiornamenti alla versione 10.2.8

*Data di pubblicazione: 01/03/2019*

- Risoluzione dei problemi di connettività riscontrati durante l'uso di Gateway Desktop remoto.
- Correzione di avvisi errati relativi ai certificati visualizzati durante la connessione.
- Risoluzione di alcuni casi in cui la barra dei menu e il dock venivano nascosti inutilmente durante l'avvio di app remote.
- Rielaborazione del codice di reindirizzamento degli Appunti per risolvere arresti anomali e blocchi riscontrati da alcuni utenti.
- Correzione di un bug che causava lo scorrimento inutile del centro connessioni durante l'avvio di una connessione.

## <a name="updates-for-version-1027"></a>Aggiornamenti alla versione 10.2.7

*Data di pubblicazione: 06/02/2019*

- In questa versione sono stati risolti problemi grafici (causati da un bug di codifica server) che si verificavano quando veniva usata la modalità AVC444.

## <a name="updates-for-version-1026"></a>Aggiornamenti alla versione 10.2.6

*Data di pubblicazione: 28/01/2019*

- Aggiunta del supporto per il codec AVC (420 e 444), disponibile in fase di connessione alle versioni correnti di Windows 10.
- Nella modalità Adatta alla finestra, l'aggiornamento di una finestra ora avviene immediatamente dopo il ridimensionamento, per garantire che il rendering del contenuto venga eseguito con il livello di interpolazione corretto.
- Correzione di un bug del layout che causava la sovrapposizione delle intestazioni dei feed per alcuni utenti.
- Pulizia dell'interfaccia utente relativa alle preferenze dell'applicazione.
- Perfezionamento dell'interfaccia utente relativa ad aggiunta/modifica desktop.
- Implementazione di numerosi perfezionamenti estetici al riquadro del centro connessioni e alle visualizzazioni elenco per desktop e feed.

>[!NOTE]
>In macOS 10.14.0 e 10.14.1 è presente un bug a causa del quale la cartella ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (annidata nella cartella ~/Library) utilizza una grande quantità di spazio su disco. Per risolvere questo problema, è necessario eliminare il contenuto della cartella ed eseguire l'aggiornamento a macOS 10.14.2. Un effetto collaterale dell'eliminazione del contenuto della cartella è l'eliminazione delle immagini snapshot assegnate ai segnalibri. Queste immagini verranno rigenerate durante la riconnessione al computer remoto.

## <a name="updates-for-version-1024"></a>Aggiornamenti alla versione 10.2.4

*Data di pubblicazione: 18/12/2018*

- Aggiunta del supporto per la modalità scura per macOS Mojave 10.14.
- Viene ora visualizzata un'opzione per l'importazione da Desktop remoto Microsoft 8 nel centro connessioni, se è vuoto.
- Risoluzione del problema di compatibilità del reindirizzamento delle cartelle con alcune applicazioni aziendali di terze parti.
- Risoluzione dei problemi per cui gli utenti ricevevano un errore 0x30000069 di Gateway Desktop remoto a causa di problemi di fallback del protocollo di sicurezza.
- Correzione di problemi di rendering progressivo riscontrati da alcuni utenti nella modalità Adatta alla finestra.
- Correzione di un bug che impediva di copiare e incollare la versione più recente di un file.
- Miglioramento dello scorrimento tramite mouse per spostamenti piccoli.

## <a name="updates-for-version-1023"></a>Aggiornamenti alla versione 10.2.3

*Data di pubblicazione: 06/11/2018*

- Aggiunta del supporto per l'impostazione del file RDP "remoteapplicationcmdline" per gli scenari di app remote.
- Il titolo della finestra della sessione ora include il nome del file RDP (e il nome del server) quando l'avvio avviene da un file RDP.
- Correzione di problemi segnalati relativi alle prestazioni di Gateway Desktop remoto.
- Correzione di arresti anomali di Gateway Desktop remoto segnalati.
- Correzione di problemi che provocavano il blocco della connessione quando questa veniva stabilita tramite Gateway Desktop remoto.
- Miglioramento della gestione delle app remote a schermo intero nascondendo in modo intelligente la barra dei menu e il dock.
- Correzione di scenari in cui le app remote rimanevano nascoste dopo l'avvio.
- Risoluzione dei problemi di aggiornamento del rendering lento con l'opzione "Adatta alla finestra" con l'accelerazione hardware disabilitata.
- Gestione degli errori di creazione dei database causati da autorizzazioni non corrette all'avvio del client.
- Correzione di un problema a causa del quale il client si arrestava continuamente in modo anomalo e non veniva avviato per alcuni utenti.
- Correzione di uno scenario in cui le connessioni venivano importate in modo non corretto come a schermo intero da Desktop remoto 8.

## <a name="updates-for-version-1022"></a>Aggiornamenti alla versione 10.2.2

*Data di pubblicazione: 09/10/2018*

- Implementazione di un centro connessioni completamente nuovo che supporta il trascinamento della selezione, la disposizione manuale dei desktop, le colonne ridimensionabili in modalità di visualizzazione elenco, l'ordinamento basato su colonne e una gestione più semplice dei gruppi.
- Il centro connessioni ora ricorda l'ultimo pivot attivo (Desktop o Feed) quando si chiude l'app.
- Modifica dell'interfaccia utente e dei flussi di richiesta di credenziali.
- Aggiunta del feedback di Gateway Desktop remoto nell'interfaccia utente relativa allo stato della connessione.
- Miglioramento dell'importazione delle impostazioni del client versione 8.
- Aggiunta della possibilità di importare i file RDP che puntano agli endpoint RemoteApp nel centro connessioni.
- Ottimizzazione dei display Retina per gli scenari di Desktop remoto con singolo monitor.
- Supporto per la specifica del livello di interpolazione della grafica (che influisce sulla sfocatura) quando non si usano le ottimizzazioni Retina.
- Supporto di 256 colori per consentire la connettività a Windows 2000.
- Correzione del ritaglio dei bordi inferiore e destro dello schermo durante la connessione a Windows 7, Windows Server 2008 R2 e versioni precedenti.
- La copia di un file locale in Outlook (in esecuzione in una sessione remota) comporta ora l'aggiunta del file come allegato.
- Correzione di un problema che rallentava i trasferimenti di file basati su spazio di lavoro in caso di file provenienti da una condivisione di rete.
- Risoluzione di un bug che bloccava Excel (in esecuzione in una sessione remota) durante il salvataggio in un file in una cartella reindirizzata.
- Correzione di un problema a causa del quale veniva segnalata l'assenza di spazio disponibile per le cartelle reindirizzate.
- Correzione di un bug a causa del quale le anteprime utilizzavano una quantità eccessiva di spazio di archiviazione su disco in macOS 10.14.
- Aggiunta del supporto per l'applicazione dei criteri di reindirizzamento dei dispositivi di Gateway Desktop remoto.
- Correzione di un problema che impediva la chiusura delle finestre di sessione durante la disconnessione da una connessione stabilita tramite Gateway Desktop remoto.
- Se l'Autenticazione a livello di rete non viene applicata dal server, viene ora eseguito il reindirizzamento alla schermata di accesso, se la password è scaduta.
- Correzione dei problemi di prestazioni rilevati in caso di trasferimento di una grande quantità di dati nella rete.
- Correzioni relative al reindirizzamento delle smart card.
- Supporto per tutti i valori possibili delle impostazioni dei file RDP "EnableCredSspSupport" e "Authentication Level" se la chiave utente predefinita ClientSettings.EnforceCredSSPSupport (nel dominio com.microsoft.rdc.macos) è impostata su 0.
- Supporto per l'impostazione del file RDP "Prompt for Credentials on Client" quando l'Autenticazione a livello di rete non viene negoziata.
- Supporto per l'accesso basato su smart card mediante il reindirizzamento della smart card al prompt di Winlogon quando l'Autenticazione a livello di rete non viene negoziata.
- Correzione di un problema che impediva il download delle risorse feed con spazi nell'URL.

## <a name="updates-for-version-1021"></a>Aggiornamenti alla versione 10.2.1

*Data di pubblicazione: 06/08/2018*

- Abilitazione della connettività ai PC aggiunti ad Azure Active Directory (AAD). Per connettersi a un PC aggiunto ad Azure AD, il nome utente deve essere in uno dei formati seguenti: "AzureAD\user" o "AzureAD\user@domain".
- Risoluzione di alcuni bug relativi all'uso di smart card in una sessione remota.

## <a name="updates-for-version-1020"></a>Aggiornamenti alla versione 10.2.0

*Data di pubblicazione: 24/07/2018*

- Integrazione di aggiornamenti per la conformità al GDPR.
- MicrosoftAccount\username@domain è ora accettato come nome utente valido.
- Riscrittura della condivisione degli Appunti per offrire maggiore velocità e supporto di altri formati.
- Quando si copiano e incollano testo, immagini o file tra sessioni ora non si passa per gli Appunti del computer locale.
- È ora possibile connettersi tramite un server Gateway Desktop remoto con un certificato non attendibile (se si accettano i messaggi di avviso).
- L'accelerazione hardware Metal viene ora usata (se supportata) per accelerare il rendering e ottimizzare l'utilizzo della batteria.
- Quando si usa l'accelerazione hardware Metal vengono eseguiti alcuni comandi magic per rendere più nitida la grafica della sessione.
- Eliminazione di alcuni scenari in cui le finestre si bloccavano dopo la chiusura.
- Correzione di bug che impedivano l'avvio di programmi RemoteApp in alcuni scenari.
- Correzione di un errore di sincronizzazione del canale di Gateway Desktop remoto che provocava errori 0x204.
- La forma del cursore del mouse ora si aggiorna correttamente in uscita da una sessione o una finestra di RemoteApp.
- Correzione di un bug di reindirizzamento delle cartelle che causava la perdita di dati quando le cartelle venivano copiate e incollate.
- Correzione di un problema di reindirizzamento delle cartelle causava la segnalazione di dimensioni delle cartelle non corrette.
- Correzione di una regressione che impediva l'accesso a un computer aggiunto ad Azure AD con un account locale.
- Correzione di bug che causavano il ritaglio dei contenuti della finestra della sessione.
- Aggiunta del supporto per i certificati di endpoint di Desktop remoto contenenti chiavi asimmetriche a curva ellittica.
- Correzione di un bug che impediva il download delle risorse gestite in alcuni scenari.
- Risoluzione di un problema di ritaglio con il centro connessioni aggiunto in alto.
- Correzione delle caselle di controllo nella scheda delle proprietà di visualizzazione nella finestra per l'aggiunta di un desktop per consentire una migliore interazione.
- Il blocco delle proporzioni è ora disabilitato quando viene applicata la modifica della visualizzazione dinamica.
- Risoluzione di problemi di compatibilità con l'infrastruttura di F5.
- Aggiornamento della gestione delle password vuote per assicurare la visualizzazione dei messaggi corretti in fase di connessione.
- Correzione di problemi di compatibilità dello scorrimento del mouse con MapInfra Pro.
- Correzione di alcuni problemi di allineamento nel centro connessioni durante l'esecuzione in Mojave.

## <a name="updates-for-version-1018"></a>Aggiornamenti alla versione 10.1.8

*Data di pubblicazione: 04/05/2018*

- Aggiunta del supporto per la modifica della risoluzione remota tramite ridimensionamento della finestra della sessione.
- Correzione degli scenari in cui il download di feed delle risorse remote richiedeva un tempo eccessivamente lungo.
- Risoluzione dell'errore 0x207 che poteva verificarsi durante la connessione ai server a cui non era stato applicato l'aggiornamento di correzione Oracle della crittografia CredSSP (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Aggiornamenti alla versione 10.1.7

*Data di pubblicazione: 05/04/2018*

- Implementazione di correzioni della sicurezza per integrare gli aggiornamenti di correzione Oracle della crittografia CredSSP descritti in CVE-2018-0886.
- Miglioramento del rendering del cursore del mouse e dell'icona di RemoteApp per risolvere i problemi di visualizzazione segnalati.
- Risoluzione dei problemi a causa dei quali le finestre di RemoteApp venivano visualizzate dietro il centro connessioni.
- Correzione di un problema che si verificava quando venivano modificate le risorse locali dopo l'importazione da Desktop remoto 8.
- È ora possibile avviare una connessione premendo INVIO in un riquadro del desktop.
- Quando viene usata la visualizzazione a schermo intero, CMD+M viene ora correttamente mappato a WIN+M.
- Le finestre relative a centro connessioni, preferenze e informazioni rispondono ora a CMD+M.
- Puoi ora avviare l'individuazione dei feed premendo INVIO nella pagina **Adding Remote Resources*- (Aggiunta risorse remote).
- Correzione di un problema a causa del quale un nuovo feed delle risorse remote veniva visualizzato vuoto nel centro connessioni fino a quando non veniva aggiornato.

## <a name="updates-for-version-1016"></a>Aggiornamenti alla versione 10.1.6

*Data di pubblicazione: 26/03/2018*

- Correzione di un problema a causa del quale le finestre di RemoteApp si riordinavano autonomamente.
- Risoluzione di un bug che provocava il blocco di alcune finestre di RemoteApp dietro alla finestra padre.
- Risoluzione di un problema di offset del puntatore del mouse che interessava alcuni programmi RemoteApp.
- Correzione di un problema a causa del quale l'avvio di una nuova connessione comportava l'assegnazione dello stato attivo a una sessione esistente invece dell'apertura di una nuova finestra di sessione.
- Correzione di un problema relativo a un messaggio di errore: ora se non è possibile trovare il gateway viene visualizzato il messaggio corretto.
- Il collegamento Esci (⌘+Q) viene ora visualizzato in modo coerente nell'interfaccia utente.
- Miglioramento della qualità delle immagini in caso di estensione in modalità "Adatta alla finestra".
- Correzione di una regressione che causava la visualizzazione di più istanze della home directory nella sessione remota.
- Aggiornamento dell'icona predefinita per i riquadri del desktop.
