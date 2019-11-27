---
title: Introduzione al client Web
description: Descrive come eseguire l'accesso al client Web di Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 08/27/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 6c37c28fded08ba68e46a10a534ee0269c714938
ms.sourcegitcommit: 89aea00fe0e00fc8b1a6e20af36ad04df8c9fe5b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/19/2019
ms.locfileid: "74189465"
---
# <a name="get-started-with-the-web-client"></a>Introduzione al client Web

Il client Web di Desktop remoto consente di usare un Web browser compatibile per accedere alle risorse remote dell'organizzazione (app e desktop) pubblicate dall'amministratore per l'utente. Questo consente all'utente di interagire ovunque con app e desktop remoti come si farebbe con un PC locale, senza dover passare a un PC desktop diverso. Dopo che l'amministratore configura le risorse remote, per iniziare saranno sufficienti il dominio, il nome utente, la password, l'URL inviato dall'amministratore e un Web browser supportato.

>[!NOTE]
>Se si vogliono conoscere le nuove versioni per il client Web, consultare [Novità del client Web di Desktop remoto](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>È necessario usare il client Web

* Per il client Web, è necessario un PC che esegue Windows, macOS, ChromeOS o Linux. I dispositivi mobili attualmente non sono supportati.
* Un browser moderno, ad esempio Microsoft Edge, Internet Explorer 11, Google Chrome, Safari o Mozilla Firefox (v55.0 e versioni successive).
* L'URL inviato dall'amministratore.

>[!NOTE]
>La versione di Internet Explorer del client Web è priva di audio in questo momento.
>Safari potrebbe visualizzare una schermata grigia se il browser è stato ridimensionato o entra in modalità a schermo intero più volte.

## <a name="start-using-the-remote-desktop-client"></a>Iniziare a usare il client di Desktop remoto

Per accedere al client, passare all'URL inviato dall'amministratore. Nella pagina di accesso, immettere il dominio e il nome utente nel formato ```DOMAIN\username```, immettere la password e quindi selezionare **Accedi**.

>[!NOTE]
>Effettuando l'accesso al client Web, si accetta che il PC sia conforme ai criteri di sicurezza dell'organizzazione.

Dopo l'accesso, il client indirizzerà l'utente alla scheda **Tutte le risorse**, contenente tutti gli elementi pubblicati in uno o più gruppi comprimibili, ad esempio il gruppo "Risorse di lavoro". Si noteranno varie icone a rappresentare applicazioni, desktop o cartelle contenenti più applicazioni o desktop che l'amministratore ha reso disponibile per il gruppo di lavoro. È possibile tornare a questa scheda in qualsiasi momento per avviare le risorse aggiuntive.

Per iniziare a usare un'app o desktop, selezionare l'elemento che si vuole usare, immettere il nome utente e password usati per accedere al client Web, se richiesto, quindi selezionare **Invio**. Potrebbe anche essere visualizzata una finestra di dialogo di consenso per accedere alle risorse locali, come appunti e stampante. È possibile scegliere di non reindirizzare uno di questi elementi o selezionare **Consenti** per usare le impostazioni predefinite. Attendere che il client Web stabilisca la connessione, quindi iniziare a usare la risorsa come di consueto.

Al termine delle operazioni, è possibile chiudere la sessione selezionando il pulsante **Disconnetti** nella barra degli strumenti nella parte superiore della schermata o chiudendo la finestra del browser.

## <a name="printing-from-the-remote-desktop-web-client"></a>Stampa dal client Web di Desktop remoto

Seguire questa procedura per stampare dal client Web:

1. Avviare il processo di stampa come di consueto per l'applicazione dalla quale si vuole stampare.
2. Quando viene richiesto di scegliere una stampante, selezionare **Stampante virtuale di Desktop remoto**.
3. Dopo aver scelto le preferenze, selezionare **Stampa**.
4. Il browser genererà un file PDF del processo di stampa.
5. È possibile aprire il file PDF e stampare il contenuto con la stampante locale oppure salvarlo sul proprio PC per usarlo in un secondo momento.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copiare e incollare dal client Web di Desktop remoto

Il client Web attualmente supporta la funzione di copia e incolla solo per testo. I file non possono essere copiati o incollati da e verso il client Web. Inoltre, è possibile usare solo **Ctrl+C** e **Ctrl+V** per copiare e incollare il testo.

## <a name="use-an-input-method-editor-ime-in-the-remote-session"></a>Usare un Input Method Editor (IME) nella sessione remota

Per usare un Input Method Editor per immettere caratteri complessi nella sessione remota, selezionare l'icona a forma di ingranaggio nella barra di spostamento per aprire il pannello laterale **Impostazioni** e impostare l'interruttore **Abilita Input Method Editor** su **Attiva**. È necessario che Input Method Editor sia installato e abilitato nella sessione remota. 

## <a name="get-help-with-the-web-client"></a>Ottenere assistenza per il client Web

Se si è verificato un problema che non può essere risolto con le informazioni contenute in questo articolo, puoi ottenere assistenza per il client Web inviando un messaggio di posta elettronica all'indirizzo indicato nella pagina di Informazioni sul client Web.
