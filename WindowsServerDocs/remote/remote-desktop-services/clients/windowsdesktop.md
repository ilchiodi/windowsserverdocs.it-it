---
title: Introduzione al client desktop di Windows
description: Informazioni di base sul client desktop di Windows.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: c864ba0e51054a553bfd53f845bd4d1c9ff3c8ba
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988239"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introduzione al client desktop di Windows

>Si applica a: Windows 10 e Windows 7

Puoi usare il client Desktop remoto per il desktop di Windows per accedere a desktop e app di Windows in remoto da un altro dispositivo Windows.

> [!NOTE]
> - Questa documentazione non è relativa al client Connessione Desktop remoto (MSTSC) fornito con Windows, bensì al nuovo client Desktop remoto (MSRDC).
> - Questo client supporta attualmente solo l'accesso ad app e desktop remoti da [Desktop virtuale Windows](https://aka.ms/wvd).
> - Se vuoi conoscere le nuove versioni per il client desktop di Windows, vedi [Novità del client desktop di Windows](windowsdesktop-whatsnew.md).

## <a name="install-the-client"></a>Installare il client

Attualmente è possibile scaricare il client per Windows a 64 bit. Questo elenco verrà aggiornato quando il client sarà disponibile per altre versioni di Windows.

- [Client Windows a 64-bit](https://go.microsoft.com/fwlink/?linkid=2068602)

Puoi installare il client per l'utente corrente, nel qual caso non sono richiesti diritti di amministratore, oppure l'amministratore può installare e configurare il client in modo che tutti gli utenti del dispositivo possano accedervi.

Dopo l'installazione, il client può essere avviato dal menu Start cercando **Desktop remoto**.

## <a name="feeds"></a>Feed

Per ottenere l'elenco di risorse gestite a cui puoi accedere, ad esempio app e desktop, effettua la sottoscrizione al feed fornito dall'amministratore. Quando effettui la sottoscrizione, le risorse diventano disponibili nel computer locale. Il client desktop di Windows attualmente supporta le risorse pubblicate da Desktop virtuale Windows.

### <a name="subscribe-to-a-feed"></a>Effettuare la sottoscrizione a un feed

1. Dalla pagina principale del client, a cui viene fatto riferimento anche come Centro connessioni, tocca **Sottoscrivi**.
2. Accedi con l'account utente quando richiesto.
3. Le risorse verranno visualizzate nel Centro connessioni raggruppate per area di lavoro.

Puoi avviare le risorse con uno dei metodi seguenti:

- Passa al Centro connessioni e fai doppio clic su una risorsa per avviarla.
- Puoi anche passare al menu Start e cercare una cartella con il nome dell'area di lavoro oppure immettere il nome della risorsa nella barra di ricerca.

### <a name="workspace-details"></a>Dettagli di un'area di lavoro

Dopo aver effettuato la sottoscrizione, puoi visualizzare altre informazioni relative a un'area di lavoro nel pannello Dettagli:

- Il nome dell'area di lavoro
- L'URL e il nome utente usati per la sottoscrizione
- Il numero di app e desktop
- La data/ora dell'ultimo aggiornamento
- Lo stato dell'ultimo aggiornamento

Accesso al pannello Dettagli:

1. Dal Centro connessioni, tocca il menu extra ( **...** ) accanto all'area di lavoro.
2. Scegli **Dettagli** dal menu a discesa.
3. Il pannello Dettagli verrà visualizzato sul lato destro del client.

Dopo aver effettuato la sottoscrizione, l'area di lavoro verrà aggiornata automaticamente a intervalli regolari. È possibile che vengano aggiunte, modificate o rimosse risorse in base alle modifiche apportate dall'amministratore.

Puoi anche cercare manualmente gli aggiornamenti delle risorse quando necessario selezionando **Aggiorna adesso** dal pannello Dettagli.

### <a name="unsubscribe-from-a-feed"></a>Annullare la sottoscrizione a un feed

In questa sezione viene illustrato come annullare la sottoscrizione a un feed. Puoi annullare la sottoscrizione per effettuarla di nuovo con un account diverso o rimuovere risorse dal sistema.

1. Dal Centro connessioni, tocca il menu extra ( **...** ) accanto all'area di lavoro.
2. Scegli **Annulla sottoscrizione** dal menu a discesa.
3. Esamina la finestra di dialogo e scegli **Continua**.

## <a name="update-the-client"></a>Aggiornare il client

Riceverai una notifica quando è disponibile una nuova versione del client, a meno che questa funzionalità non sia stata disabilitata da un amministratore. La notifica può essere visualizzata direttamente nel Centro connessioni o nel Centro operativo Windows. Seleziona la notifica per avviare il processo di aggiornamento.

Puoi anche cercare manualmente nuovi aggiornamenti per il client:

1. Dal Centro connessioni, tocca il menu extra ( **...** ) sulla barra dei comandi nella parte superiore del client.
2. Scegli **Informazioni** dal menu a discesa.
3. Tocca **Controlla aggiornamenti**.
4. Se è disponibile un aggiornamento, tocca **Installa aggiornamento** per aggiornare il client.

## <a name="providing-feedback"></a>Commenti e suggerimenti

Puoi inviare eventuali suggerimenti su una funzionalità o segnalare un problema usando l'[Hub di Feedback](feedback-hub://?tabid=2&contextid=883), accessibile anche dal client:

1. Dal Centro connessioni, tocca il menu extra ( **...** ) sulla barra dei comandi nella parte superiore del client.
2. Scegli **Feedback** dal menu a discesa per aprire l'Hub di Feedback.
