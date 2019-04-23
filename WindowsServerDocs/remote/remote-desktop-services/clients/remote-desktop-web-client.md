---
title: Accedere al client Web di Desktop remoto
description: Viene descritto come eseguire l'accesso al client di web Desktop remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849372"
---
# <a name="access-the-remote-desktop-web-client"></a>Accedere al client Web di Desktop remoto

Il client di web Desktop remoto consente di utilizzare un web browser compatibile con per accedere a risorse remote della propria organizzazione (le App e desktop) pubblicate all'utente dall'amministratore. Sarà in grado di interagire con l'App remote e desktop come si farebbe con un PC locale ovunque trovino, senza dover passare a un diverso computer desktop. Dopo che l'amministratore configura le risorse remote, è sufficiente sono il dominio, nome utente e password, l'amministratore inviato è e un web browser supportato, l'URL e sei pronto per iniziare.

>[!NOTE]
>Conoscere le nuove versioni per il client web? Scopri [novità di client web Desktop remoto?](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>È necessario usare il client web

* Per il client web, è necessario un computer che esegue Windows, macOS, ChromeOS o Linux. I dispositivi mobili non sono supportati in questo momento.
* Un browser moderno, ad esempio Microsoft Edge, Internet Explorer 11, Google Chrome, Safari o Mozilla Firefox (v55.0 e versioni successive).
* L'URL all'amministratore è ricevuta.

>[!NOTE]
>La versione di Internet Explorer del client web è privo di audio in questo momento.
>Safari potrebbe visualizzare una schermata grigio se il browser è stato ridimensionato o entra in modalità a schermo intero più volte.

## <a name="start-using-the-remote-desktop-client"></a>Iniziare a usare il client Desktop remoto

Per accedere al client, passare all'URL di cui è stato inviato all'amministratore. Nella pagina di accesso, immettere il nome di dominio e utente nel formato ```DOMAIN\username```, immettere la password e quindi selezionare **Accedi**.

>[!NOTE]
>Effettuando l'accesso al client web, si accetta che i PC sia conforme ai criteri di sicurezza dell'organizzazione.

Dopo l'accesso, il client verrà visualizzato il **tutte le risorse** scheda, che contiene tutti gli elementi pubblicati in è in uno o più gruppi comprimibili, ad esempio il gruppo "Risorse di lavoro". Si noterà varie icone che rappresentano le applicazioni, desktop o cartelle che contengono più applicazioni o desktop che l'amministratore ha reso disponibile per il gruppo di lavoro. È possibile tornare a questa scheda in qualsiasi momento per avviare le risorse aggiuntive.

Per iniziare a usare un'app o il desktop, selezionare l'elemento che si desidera usare, immettere il nome utente e password usati per accedere al client web, se richiesto stesso e quindi selezionare **Submit**. Si potrebbe anche essere visualizzato una finestra di dialogo di consenso per accedere alle risorse locali, ad esempio negli Appunti e stampanti. È possibile scegliere di non reindirizza uno di questi elementi, o selezionare **Consenti** per usare le impostazioni predefinite. Attendere che il client web stabilire la connessione e quindi iniziare a usare la risorsa come di consueto.

Quando hai finito, è possibile terminare la sessione di, selezionando il **Sign Out** pulsante sulla barra degli strumenti nella parte superiore della schermata o la chiusura della finestra del browser.

## <a name="printing-from-the-remote-desktop-web-client"></a>Stampa dal client web Desktop remoto

Seguire questa procedura per stampare dal client web:

1. Avviare il processo di stampa come di consueto per le app che si desidera stampare da.
2. Quando viene richiesto di scegliere una stampante, selezionare **stampante virtuale Desktop remota**.
3. Dopo aver scelto le preferenze, selezionare **stampa**.
4. Il browser genererà un file PDF del processo di stampa.
5. È possibile scegliere di aprire il file PDF e stampare il contenuto per la stampante locale oppure salvarlo sul proprio PC per un uso successivo.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copiare e incollare dal client web Desktop remoto

Il client web attualmente supporta copiando e incollando solo testo. I file non possono essere copiati o incollati da e verso il client web. Inoltre, è possibile usare solo **Ctrl + C** e **Ctrl + V** per copiare e incollare il testo.

## <a name="get-help-with-the-web-client"></a>Ottieni assistenza per il client web

Se si è verificato un problema che non può essere risolto tramite le informazioni contenute in questo articolo, è possibile ottenere assistenza con il client web all'indirizzo e-mail l'indirizzo nella pagina di informazioni del client web.