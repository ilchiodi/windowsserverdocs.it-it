---
title: Novità del client desktop di Windows
description: Informazioni sulle modifiche recenti apportate al client Desktop remoto per Windows Desktop
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/14/2020
ms.localizationpriority: medium
ms.openlocfilehash: 016a88999b93d686faff73134a660014fd602765
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2020
ms.locfileid: "81279697"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novità del client desktop di Windows

Per informazioni più dettagliate sul client desktop di Windows, vedi [Introduzione al client desktop di Windows](windowsdesktop.md). Di seguito sono riportati gli aggiornamenti del client più recenti.

## <a name="latest-client-versions"></a>Versioni del client più recenti

Il client può essere configurato per [gruppi di utenti](windowsdesktop-admin.md#configure-user-groups) diversi. Nella tabella seguente sono elencate le versioni correnti disponibili per ogni gruppo di utenti:

|Gruppo utenti |Version  |
|-----------|---------|
|Pubblico     |1.2.790  |
|Insider    |1.2.940  |

## <a name="updates-for-version-12940"></a>Aggiornamenti per la versione 1.2.940

*Data di pubblicazione: 14/04/2020*

Scarica: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4txZU), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4txZV), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4tM6I)

- Sono state aggiunte nuove impostazioni di visualizzazione per le connessioni desktop disponibili quando si fa clic con il pulsante destro del mouse sull'icona di un desktop in Connection Center (Centro connessioni).
  - Sono ora disponibili tre opzioni di configurazione per la visualizzazione: **All displays** (Tutte le visualizzazioni), **Single display** (Visualizzazione singola) e **Select displays** (Alcune visualizzazioni).
  - Le impostazioni disponibili vengono ora visualizzate solo quando si seleziona una configurazione di visualizzazione.
  - In modalità Select displays (Alcune visualizzazioni) la nuova opzione **Maximize to current displays** (Ingrandisci a visualizzazioni correnti) consente di modificare in modo dinamico le visualizzazioni usate per la sessione senza riconnettersi. Quando l'opzione è abilitata, l'ingrandimento determina la visualizzazione della sessione a schermo intero in tutte le visualizzazioni interessate dalla finestra di sessione.
  - È stata aggiunta la nuova opzione **Single display when windowed** (Visualizzazione singola in modalità finestra) per le modalità All displays (Tutte le visualizzazioni) e Select displays (Alcune visualizzazioni). Questa opzione converte automaticamente la sessione in una visualizzazione singola quando si esce dalla modalità schermo intero e ripristina automaticamente più visualizzazioni quando si ingrandisce la finestra.
- È stato aggiunto il nuovo gruppo **Impostazioni di visualizzazione** al menu di sistema che viene visualizzato quando si fa clic con il pulsante destro del mouse sulla barra del titolo di una sessione desktop in modalità finestra. In questo modo potrai modificare alcune impostazioni in modo dinamico durante una sessione. Potrai ad esempio modificare le nuove impostazioni **Single display mode when windowed** (Visualizzazione singola in modalità finestra) e **Maximize to current displays** (Ingrandisci a visualizzazioni correnti).
- Quando si esce dalla visualizzazione a schermo intero, la finestra della sessione tornerà nella posizione in cui si trovava la prima volta che è stata attivata la visualizzazione a schermo intero.
- Al termine della reimpostazione dei dati utente nella pagina Informazioni su, il client non viene chiuso ma viene eseguito il reindirizzamento a Connection Center (Centro connessioni).
- Sono stati risolti alcuni problemi di accessibilità con lo spostamento tramite tabulazione e le utilità per la lettura dello schermo.
- È stato corretto un problema di sfarfallio e compattazione che si verificava durante il trascinamento di una finestra di sessione desktop tra visualizzazioni con fattori di scala diversi.
- È stato corretto un errore che si verificava durante il reindirizzamento delle fotocamere.
- Sono stati corretti diversi arresti anomali per migliorare l'affidabilità.

## <a name="updates-for-version-12790"></a>Aggiornamenti per la versione 1.2.790

*Data di pubblicazione: 24/03/2020*

Scarica: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4siSh), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4siSi), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4sllb)

- L'azione Aggiorna ("Update") per le aree di lavoro è stata rinominata Aggiorna ("Refresh") per coerenza con altri client Desktop remoto.
- Puoi ora aggiornare un'area di lavoro direttamente dal relativo menu di scelta rapida.
- L'aggiornamento manuale di un'area di lavoro garantisce ora l'aggiornamento di tutto il contenuto locale.
- Puoi ora reimpostare i dati utente del client nella pagina Informazioni su, senza dover disinstallare l'app.
- Puoi reimpostare i dati utente del client anche usando msrdcw.exe /reset con un parametro /f facoltativo per ignorare il prompt.
- La ricerca dell'aggiornamento di un client viene ora eseguita automaticamente durante l'accesso alla pagina Informazioni su.
- È stato aggiornato il colore dei pulsanti per motivi di coerenza.

## <a name="updates-for-version-12675"></a>Aggiornamenti per la versione 1.2.675

*Data di pubblicazione: 25/02/2020*

Scarica: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qeak), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qm7h), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qm7g)

- Le connessioni a Desktop virtuale Windows ora vengono bloccate se nel file RDP manca la firma oppure se una delle proprietà signscope è stata modificata.
- Se un'area di lavoro è vuota o è stata rimossa, il Centro connessioni non risulta più vuoto.
- Sono stati aggiunti l'ID attività e il codice di errore per i messaggi di disconnessione per facilitare la risoluzione dei problemi. Puoi copiare il messaggio della finestra di dialogo con **CTRL + C**.
- È stato risolto un problema che causava la mancata rilevazione degli schermi da parte delle impostazioni della connessione desktop.
- Gli aggiornamenti client non riavviano più il PC automaticamente.
- Le icone senza finestra non dovrebbero più essere visualizzate sulla barra delle applicazioni.

## <a name="updates-for-version-12605"></a>Aggiornamenti per la versione 1.2.605

*Data di pubblicazione: 29/01/2020*

Scarica: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oHrD), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oJZs), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oXhD)

- Ora puoi selezionare le visualizzazioni da usare per le connessioni desktop. Per modificare l'impostazione, fai clic con il pulsante destro del mouse sull'icona della connessione desktop e scegli **Impostazioni**.
- È stato risolto il problema che impediva la visualizzazione dei fattori di conversione corretti nelle impostazioni di connessione.
- È stato corretto l'errore che impediva all'Assistente vocale di leggere la finestra di dialogo visualizzata durante l'avvio della connessione.
- È stato risolto il problema per cui veniva visualizzato il nome utente errato quando i nomi di Azure Active Directory e Active Directory non coincidevano.
- È stato risolto il problema che causava l'interruzione del client all'avvio di una connessione quando tale client non era connesso a una rete.
- È stato risolto il problema che causava l'interruzione del client quando veniva collegato un auricolare.

## <a name="updates-for-version-12535"></a>Aggiornamenti per la versione 1.2.535

*Data di pubblicazione: 04/12/2019*

Scarica: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jH), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jL), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k27O)

- Puoi ora accedere alle informazioni sugli aggiornamenti direttamente con il pulsante Altre opzioni nella barra dei comandi nella parte superiore del client.
- Puoi ora lasciare un feedback dalla barra dei comandi del client.
- L'opzione Feedback viene ora visualizzata solo se è disponibile Hub di Feedback.
- È stato verificato che la notifica di aggiornamento non venga visualizzata quando le notifiche sono state disabilitate tramite criteri.
- È stato corretto un errore che impediva l'avvio di alcuni file RDP.
- È stato corretto l'arresto anomalo del sistema all'avvio del client causato dal danneggiamento di alcune impostazioni permanenti.

## <a name="updates-for-version-12431"></a>Aggiornamenti per la versione 1.2.431

*Data di pubblicazione: 12/11/2019*

Scarica: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48kow), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48koA), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48zYj)

- Sono ora disponibili le versioni a 32 bit e ARM64 del client.
- Il client ora salva le modifiche apportate alla barra di connessione, ad esempio la posizione, le dimensioni e lo stato bloccato, e applica tali modifiche alle varie sessioni.
- Le finestre di dialogo relative alle informazioni del gateway e allo stato della connessione sono state aggiornate.
- È stato risolto un problema che causava la richiesta contemporanea di due credenziali durante il tentativo di connessione dopo la scadenza del token di Azure Active Directory.
- Agli utenti di Windows 7 vengono ora richieste correttamente le credenziali se hanno salvato le credenziali quando il server non lo consente.
- Il messaggio di richiesta di Azure Active Directory viene ora visualizzato in primo piano rispetto alla finestra per la riconnessione.
- Gli elementi aggiunti alla barra delle applicazioni vengono ora aggiornati durante un aggiornamento del feed.
- Nel Centro connessioni è stata migliorata la funzionalità di scorrimento tramite tocco.
- È stata rimossa la riga vuota dal menu a discesa relativo alla risoluzione.
- Sono state rimosse le voci non necessarie in Gestione credenziali di Windows.
- Le sessioni desktop vengono ora dimensionate correttamente quando si esce dalla modalità a schermo intero.
- La finestra di dialogo di disconnessione di RemoteApp viene ora visualizzata in primo piano quando si riprende la sessione dopo l'attivazione della modalità sospensione.
- Sono stati risolti alcuni problemi di accessibilità come lo spostamento tramite tastiera.

## <a name="updates-for-version-12247"></a>Aggiornamenti per la versione 1.2.247

*Data di pubblicazione: 17/09/2019*

Scarica: [Windows a 64 bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3LkSa)

- Sono state migliorate le lingue di fallback per la versione localizzata. Ad esempio, FR-CA verrà visualizzato correttamente in francese anziché in inglese.
- Quando viene rimossa una sottoscrizione, il client ora rimuove correttamente da Gestione credenziali le credenziali salvate.
- Dopo l'avvio, il processo di aggiornamento del client ora è automatico e, al termine, il client viene riavviato.
- Il client ora può essere usato in Windows 10 in modalità S.
- È stato risolto un problema a causa del quale il processo di aggiornamento aveva esito negativo per gli utenti con uno spazio nel nome utente.
- È stato risolto un problema di arresto anomalo del sistema che si verificava al momento dell'autenticazione durante una connessione.
- È stato risolto un problema di arresto anomalo del sistema che si verificava durante la chiusura del client.
