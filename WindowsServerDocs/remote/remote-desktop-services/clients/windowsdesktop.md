---
title: Introduzione al client desktop di Windows
description: Informazioni di base sul client desktop di Windows.
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
ms.date: 11/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: e71aa2b1cccda85e0bf6f8a80ad68013346b30d4
ms.sourcegitcommit: 3d76683718ec6f38613f552f518ebfc6a5db5401
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74829614"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introduzione al client desktop di Windows

>Si applica a: Windows 10, Windows 10 IoT Enterprise e Windows 7

Puoi usare il client Desktop remoto per il desktop di Windows per accedere a desktop e app di Windows in remoto da un altro dispositivo Windows.

> [!NOTE]
> - Questa documentazione non è relativa al client Connessione Desktop remoto (MSTSC) fornito con Windows, bensì al nuovo client Desktop remoto (MSRDC).
> - Questo client supporta attualmente solo l'accesso ad app e desktop remoti da [Desktop virtuale Windows](https://aka.ms/wvd).
> - Se vuoi conoscere le nuove versioni per il client desktop di Windows, vedi [Novità del client desktop di Windows](windowsdesktop-whatsnew.md).

## <a name="install-the-client"></a>Installare il client

Scegli il client corrispondente alla versione di Windows. Il nuovo client Desktop remoto (MSRDC) supporta dispositivi client Windows 10, Windows 10 IoT Enterprise e Windows 7. 

- [Windows a 64 bit](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows a 32 bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Puoi installare il client per l'utente corrente, nel qual caso non sono richiesti diritti di amministratore, oppure l'amministratore può installare e configurare il client in modo che tutti gli utenti del dispositivo possano accedervi.

Dopo aver installato il client, puoi avviarlo dal menu Start cercando **Desktop remoto**.

## <a name="update-the-client"></a>Aggiornare il client

Riceverai una notifica ogni volta che sarà disponibile una nuova versione del client, tranne nel caso in cui l'amministratore abbia disabilitato le notifiche. La notifica verrà visualizzata nel Centro connessioni o nel Centro operativo Windows. Per aggiornare il client, è sufficiente selezionare la notifica.

Puoi anche cercare manualmente nuovi aggiornamenti per il client:

1. Dal Centro connessioni, tocca il menu extra ( **...** ) sulla barra dei comandi nella parte superiore del client.
2. Scegli **Informazioni** dal menu a discesa.
3. Tocca **Controlla aggiornamenti**.
4. Se è disponibile un aggiornamento, tocca **Installa aggiornamento** per aggiornare il client.

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

## <a name="managed-desktops"></a>Desktop gestiti

Le aree di lavoro possono contenere più risorse gestite, inclusi i desktop. Quando accedi a un desktop gestito, hai accesso a tutte le app installate dall'amministratore.

### <a name="desktop-settings"></a>Impostazioni del desktop

Puoi configurare alcune impostazioni per le risorse desktop per assicurarti che l'esperienza soddisfi le tue esigenze. Per accedere all'elenco delle impostazioni disponibili:

1. In Centro connessioni fai clic con il pulsante destro del mouse su una risorsa desktop.
2. Scegli **Impostazioni** dal menu a discesa.
3. Sul lato destro del client verrà visualizzato il pannello Impostazioni, in cui sarà visibile il nome del desktop.

Il client userà le impostazioni configurate dall'amministratore, a meno che tu non disattivi l'opzione **Usa impostazioni predefinite**. In tal caso potrai configurare le opzioni seguenti:

- **Usa tutti i monitor** ti consente di passare per la sessione desktop dall'uso di tutti i monitor locali disponibili all'uso di un solo monitor e viceversa.
- **Avvia in modalità schermo intero** determina se la sessione verrà avviata a schermo intero o in modalità finestra. Questa impostazione viene abilitata automaticamente quando vengono usati tutti i monitor.
- **Update the resolution on resize** (Aggiorna la risoluzione in caso di ridimensionamento) modifica il comportamento quando ridimensioni la sessione in modalità finestra. Se questa impostazione è abilitata, la risoluzione del desktop remoto verrà aggiornata in modo da ottenere la corrispondenza con le dimensioni della finestra locale. Se l'impostazione è disabilitata, per l'intera durata della sessione verrà mantenuta la risoluzione specificata in **Risoluzione**. Questa impostazione viene abilitata automaticamente quando vengono usati tutti i monitor.
- **Risoluzione** ti consente di specificare la risoluzione del desktop remoto. Tale risoluzione verrà mantenuta per l'intera durata della sessione. Questa impostazione viene disabilitata automaticamente se la risoluzione è impostata sull'aggiornamento in caso di ridimensionamento.
- **Change the size of the text and apps** (Cambia le dimensioni di testo e app) specifica le dimensioni del contenuto della sessione. Questa impostazione si applica solo in caso di connessione a Windows 8.1 e versioni successive oppure a Windows Server 2012 R2 e versioni successive. Questa impostazione viene disabilitata automaticamente se la risoluzione è impostata sull'aggiornamento in caso di ridimensionamento.
- **Fit session to window** (Adatta la sessione alla finestra) determina la modalità di visualizzazione della sessione quando la risoluzione del desktop remoto non corrisponde alle dimensioni della finestra locale. Se questa impostazione è abilitata, il contenuto della sessione verrà ridimensionato in modo da entrare nella finestra mantenendo comunque le proporzioni della sessione. Se è disabilitata, verranno visualizzate barre di scorrimento o aree nere quando la risoluzione e le dimensioni della finestra non corrispondono.

## <a name="provide-feedback"></a>Fornire feedback

Puoi inviare eventuali suggerimenti su una funzionalità o segnalare un problema Usa l'[Hub di Feedback](feedback-hub://?tabid=2&contextid=883) per inviarci le tue impressioni. Puoi accedere all'Hub di Feedback anche tramite il client:

1. Dal Centro connessioni, tocca il menu extra ( **...** ) sulla barra dei comandi nella parte superiore del client.
2. Scegli **Feedback** dal menu a discesa per aprire l'Hub di Feedback.
