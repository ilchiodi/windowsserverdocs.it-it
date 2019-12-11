---
title: Novità del client desktop di Windows
description: Informazioni sulle modifiche recenti apportate al client Desktop remoto per Windows Desktop
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 12/04/2019
ms.localizationpriority: medium
ms.openlocfilehash: 5526d401ba26cabcaccfc0b88df9b0e69f8149a5
ms.sourcegitcommit: 3d76683718ec6f38613f552f518ebfc6a5db5401
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74829603"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novità del client desktop di Windows

Per informazioni più dettagliate sul client desktop di Windows, vedi [Introduzione al client desktop di Windows](windowsdesktop.md). Di seguito sono riportati gli aggiornamenti del client più recenti.

## <a name="latest-client-versions"></a>Versioni del client più recenti

Il client può essere configurato per [gruppi di utenti](windowsdesktop-admin.md#configure-user-groups) diversi. Nella tabella seguente sono elencate le versioni correnti disponibili per ogni gruppo di utenti:

|Gruppo di utenti |Versione  |
|-----------|---------|
|Public     |1.2.535  |
|Insider    |1.2.535  |

## <a name="updates-for-version-12535"></a>Aggiornamenti per la versione 1.2.535

*Data di pubblicazione: 04/12/2019*

Download: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jH), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jL), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k27O)

- Puoi ora accedere alle informazioni sugli aggiornamenti direttamente con il pulsante Altre opzioni nella barra dei comandi nella parte superiore del client.
- Puoi ora lasciare un feedback dalla barra dei comandi del client.
- L'opzione Feedback viene ora visualizzata solo se è disponibile Hub di Feedback.
- È stato verificato che la notifica di aggiornamento non venga visualizzata quando le notifiche sono state disabilitate tramite criteri.
- È stato corretto un errore che impediva l'avvio di alcuni file RDP.
- È stato corretto l'arresto anomalo del sistema all'avvio del client causato dal danneggiamento di alcune impostazioni permanenti.

## <a name="updates-for-version-12431"></a>Aggiornamenti per la versione 1.2.431

*Data di pubblicazione: 12/11/2019*

Download: [Windows 64-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48kow), [Windows 32-bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48koA), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48zYj)

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

Download: [Windows a 64 bit](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3LkSa)

- Sono state migliorate le lingue di fallback per la versione localizzata. Ad esempio, FR-CA verrà visualizzato correttamente in francese anziché in inglese.
- Quando viene rimossa una sottoscrizione, il client ora rimuove correttamente da Gestione credenziali le credenziali salvate.
- Dopo l'avvio, il processo di aggiornamento del client ora è automatico e, al termine, il client viene riavviato.
- Il client ora può essere usato in Windows 10 in modalità S.
- È stato risolto un problema a causa del quale il processo di aggiornamento aveva esito negativo per gli utenti con uno spazio nel nome utente.
- È stato risolto un problema di arresto anomalo del sistema che si verificava al momento dell'autenticazione durante una connessione.
- È stato risolto un problema di arresto anomalo del sistema che si verificava durante la chiusura del client.
