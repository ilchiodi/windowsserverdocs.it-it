---
title: Novità del client Web
description: Informazioni sulle modifiche recenti apportate al client Web di Desktop remoto
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
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4eaa9a187b4a8f7879cabf15724bfcfe3f625939
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2019
ms.locfileid: "70150952"
---
# <a name="whats-new-in-the-web-client"></a>Novità del client Web

Aggiorniamo regolarmente il [client Web di Desktop remoto](remote-desktop-web-client.md) aggiungendo nuove funzionalità e risolvendo i problemi. Scopri gli aggiornamenti più recenti riportati di seguito.

> [!NOTE]
> Abbiamo modificato il sistema di controllo delle versioni per il client Web. A partire dalla versione 1.0.18.0, tutte le versioni del client Web conterranno numeri (nel formato "W.X.Y.Z"). I numeri di versione per il client Web di Desktop remoto terminano sempre con il valore 0, ad esempio W.X.Y.0. Ogni versione del client Web di Desktop virtuale Windows modificherà l'ultima cifra fino alla successiva versione del client Web di Desktop remoto, ad esempio 1.0.18.1.

## <a name="updates-for-version-10180"></a>Aggiornamenti alla versione 1.0.18.0
*Data di pubblicazione: 14/05/2019*

- È stata aggiunta la configurazione del metodo di avvio delle risorse nella scheda Impostazioni, consentendo agli utenti di aprire le risorse nel browser o scaricare un file con estensione rdp da gestire con un altro client. Questa impostazione può essere configurata dall'amministratore. I dettagli riguardanti le configurazioni di amministratore per questa funzionalità sono disponibili nella [documentazione di installazione del client Web](remote-desktop-web-client-admin.md).
- Sono stati risolti i problemi relativi al rendering del colore, abilitando la visualizzazione di colori più brillanti nella sessione remota.
- È stata eseguita la revisione dei messaggi di errore relativi a errori del feed delle risorse remote. 
- È stato aggiunto il supporto per più collegamenti di Office, ad esempio Incolla speciale (CTRL+ALT+V).
- È stata aggiunta una combinazione di tasti che consente agli utenti di richiamare il tasto WINDOWS nella sessione remota (ALT+F3)
- È stato aggiornato il messaggio di errore per gli utenti che tentano di autenticarsi usando una password scaduta.
- È stata aggiornata l'interfaccia utente del feed nella pagina Tutte le risorse.
- È stato risolto il problema delle finestre di dialogo sovrapposte durante la riconnessione della sessione.
- È stata ridimensionata l'icona delle risorse remote nella barra delle applicazioni relativa alle risorse.

## <a name="updates-for-version-1011"></a>Aggiornamenti alla versione 1.0.11
*Data di pubblicazione: 22/02/2019*

- È stata abilitata la connessione al gestore Desktop remoto senza un gateway Desktop remoto in Windows Server 2019.
- I feed sono stati ordinati alfabeticamente (ad esempio, prima Desktop e dopo RemoteApp).
- Sono stati corretti vari bug di accessibilità migliorando la compatibilità dell'utilità per la lettura dello schermo.
- Sono stati aggiornati gli strumenti di compilazione.
- Sono stati aggiornati vari bug.

## <a name="updates-for-version-107"></a>Aggiornamenti alla versione 1.0.7
*Data di pubblicazione: 24/01/2019*

- È ora supportato l'uso offline delle reti interne.
- È stato migliorato il rendering nei browser non Microsoft Edge.
- È stato implementato il limite per la ripetizione dei tentativi di recupero del feed per impedire attacchi Denial of Service.
- Sono stati corretti i bug relativi all'accessibilità, in modo da consentire agli utenti con problemi di vista di usare il client Web.
- Sono stati migliorati i messaggi di errore visualizzati dall'utente per gli errori di feed.
- Sono state aggiunte le combinazioni di tasti CTRL+ALT+FINE (Windows) e FN+CONTROL+OPZIONE+CANCELLA (Mac) per richiamare CTRL+ALT+CANC nel computer remoto.
- È stata migliorata la telemetria degli eventi di arresto anomalo.
- Sono stati migliorati gli strumenti di compilazione e la pipeline di compilazione.
- Sono stati aggiornati vari bug.

## <a name="updates-for-version-101"></a>Aggiornamenti alla versione 1.0.1
*Data di pubblicazione: 29/10/2018*

- È stata aggiunta un'opzione per **acquisire le informazioni di supporto** nella pagina Informazioni su per diagnosticare i problemi.
- È ora supportata la modalità InPrivate.
- È stato migliorato il supporto per le tastiere non in lingua inglese.
- È stato risolto un problema relativo alle descrizioni comando con caratteri non in lingua inglese visualizzati in modo errato.
- È stato corretto un problema relativo al rendering degli elementi grafici che interessava gli utenti di Chrome.
- È stato aggiornato il reindirizzamento del fuso orario con pieno supporto dell'ora legale.
- È stato migliorato il messaggio di errore relativo alla memoria esaurita.
- Sono stati aggiornati vari bug.

## <a name="updates-for-version-100"></a>Aggiornamenti alla versione 1.0.0
*Data di pubblicazione: 16/07/2018*

- È ora disponibile a livello generale il client Web di Desktop remoto.
- Gli amministratori possono globalmente disattivare la telemetria per il client Web.
- Sono stati aggiornati vari bug.

## <a name="updates-for-version-090"></a>Aggiornamenti alla versione 0.9.0
*Data di pubblicazione: 05/07/2018*

- È stata implementata una nuova esperienza di accesso al client Web.
- Non è più richiesto l'uso di credenziali per l'avvio di una connessione desktop o app (Single Sign-On).
- Il client Web è stato spostato in un nuovo URL: <https://server_FQDN/RDWeb/webclient/index.html>
- È stato aggiunto il reindirizzamento del fuso orario.
- Sono stati aggiornati vari bug.

## <a name="updates-for-version-081"></a>Aggiornamenti alla versione 0.8.1
*Data di pubblicazione: 17/05/2018*

- Sono stati introdotti aggiornamenti per risolvere la correzione oracolo di crittografia CredSSP descritta in CVE-2018-0886.
- Sono stati corretti gli errori di connessione per alcune lingue quando è abilitata la stampa.
- È stato migliorato il messaggio di errore quando un gateway non fa parte della distribuzione.
- Sono state aggiunte le opzioni **Guida** e **Feedback**.

## <a name="updates-for-version-080"></a>Aggiornamenti alla versione 0.8.0
*Data di pubblicazione: 28/03/2018*

- È stata rilasciata la versione di anteprima pubblica iniziale del client Web.
- È stata aggiunta l'opzione per copiare e incollare testo dagli Appunti con **CTRL+C** e **CTRL+V**.
- È stata aggiunta l'opzione per stampare un file con estensione pdf.
- È stata implementata la localizzazione in 18 lingue.
 
