---
title: Accedere al client Web di Desktop remoto
description: Descrive come accedere al client web Desktop remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065858"
---
# Accedere al client Web di Desktop remoto

Il client web di Desktop remoto consente di usare un browser compatibile per accedere a risorse remote dell'organizzazione (App e desktop) è pubblicate dal tuo amministratore. Sarai in grado di interagire con l'App remota e desktop come faresti con un PC locale indipendentemente dalla loro posizione, senza dover passare a un altro PC desktop. Una volta che l'amministratore configura le risorse remote, tutto ciò che devi sono il dominio, nome utente e password, l'URL per il tuo amministratore inviato puoi e un web browser supportato, e sei pronta per l'utilizzo.

>[!NOTE]
>Conoscere i nuovi rilasci per il client web? Consulta [Novità per i client web di Desktop remoto?](web-client-whatsnew.md)

## Cosa è necessario utilizzare il client web

* Per il client web, è necessario un computer che esegue Windows, macOS, Linux o ChromeOS. I dispositivi mobili non sono supportati in questo momento.
* Un browser più moderno come Microsoft Edge, Internet Explorer 11, Google Chrome, Safari o Mozilla Firefox (v55.0 e versioni successive).
* L'URL per il tuo amministratore ti ha inviato.

>[!NOTE]
>La versione di Internet Explorer del client web non prevede l'audio in questo momento.
>Safari potrebbe visualizzare una schermata grigia se il browser viene ridimensionato o entra in modalità a schermo intero più volte.

## Iniziare a usare il client Desktop remoto

Per accedere a client, Vai all'URL il tuo amministratore ti ha inviato. Nella pagina di accesso, Immetti il tuo nome di dominio e utente nel formato ```DOMAIN\username```, Immetti la password e quindi seleziona **Accedi**.

>[!NOTE]
>Effettuando l'accesso al client web, accetti che il PC sia conforme ai criteri di sicurezza dell'organizzazione.

Dopo l'accesso, il client verrà visualizzato alla scheda **Tutte le risorse** , che contiene tutti gli elementi hai pubblicati in uno o più gruppi comprimibili, ad esempio il gruppo di "Risorse di lavoro". Vedrai varie icone che rappresentano l'App, computer desktop o cartelle contenenti altre App o desktop che l'amministratore ha reso disponibile per il gruppo di lavoro. È possibile tornare a questa scheda in qualsiasi momento per avviare risorse aggiuntive.

Per iniziare a usare un'app o desktop, seleziona l'elemento che vuoi usare, Immetti il nome utente e password che è usata per accedere al client web se richiesto e quindi seleziona **Invia**stesso. Si potrebbe essere visualizzata anche una finestra di dialogo di consenso dell'utente per accedere alle risorse locali, come negli Appunti e stampante. Puoi scegliere di non reindirizzare uno di questi elementi oppure seleziona **Consenti** di usare le impostazioni predefinite. Attendere che il client web stabilire la connessione e quindi iniziare a usare la risorsa come di consueto.

Quando hai finito, è possibile terminare la sessione selezionando il pulsante **Sign Out** nella barra degli strumenti nella parte superiore dello schermo o la chiusura della finestra del browser.

## Stampa dal client web di Desktop remoto

Segui questi passaggi per la stampa dal client web di:

1. Avvia il processo di stampa come faresti normalmente per le app che desideri stampare dal.
2. Quando ti viene chiesto di scegliere una stampante, seleziona **Stampante virtuale di Desktop remoto**.
3. Dopo aver scelto le preferenze, selezionare **Stampa**.
4. Il browser genera un file PDF del processo di stampa.
5. Puoi scegliere di aprire il file PDF e stampare il contenuto per la stampante locale o salvarlo al PC per un utilizzo successivo.

## Copiare e incollare dal client web di Desktop remoto

Il client web attualmente supporta copiare e incollare solo testo. I file non possono essere copiati o incollati da e verso il client web. Inoltre, è possibile utilizzare solo **Ctrl + C** e **Ctrl + V** per copiare e incollare testo.

## Ottenere assistenza con il client web

Se si è verificato un problema che non può essere risolto tramite le informazioni contenute in questo articolo, è possibile ottenere assistenza con il client web l'indirizzo nella pagina di informazioni del client web di un messaggio e-mail.