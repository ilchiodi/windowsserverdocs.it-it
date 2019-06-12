---
title: Novità per i client di web Desktop remoto?
description: Informazioni sulle modifiche recenti per il client di web Desktop remoto
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
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 5be9b05da1e78cc54e12254f43d0f44f7ff65c5d
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804877"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>Novità per il client di web Desktop remoto?

Viene aggiornato regolarmente le [client web Desktop remoto](remote-desktop-web-client.md), aggiungendo nuove funzionalità e risoluzione dei problemi. Scopri gli aggiornamenti più recenti riportato di seguito.

> [!NOTE]
> È stato modificato il sistema di controllo delle versioni per il client web. A partire dalla versione 1.0.18.0, tutte le versioni di rilascio di client web conterrà i numeri (nel formato "W.x.y. z"). I numeri di versione per il client di web Desktop remoto termina sempre con il valore 0 (ad esempio, W.X.Y.0). Ogni versione del client web Windows Desktop virtuale verrà modificato l'ultima cifra finché la prossima versione di client di Desktop remoto web (ad esempio, 1.0.18.1).

## <a name="updates-for-version-10180"></a>Aggiornamenti per la versione 1.0.18.0
*Data di pubblicazione: 5/14/2019*

- Configurazione metodo di avvio delle risorse aggiunto nella scheda Impostazioni, consentendo agli utenti di aprire le risorse nel browser o scaricare un file con estensione rdp da gestire con un altro client. Questa impostazione può essere configurata dall'amministratore. I dettagli riguardanti le configurazioni di amministratore per questa funzionalità è disponibile nel [documentazione dell'installazione di client web](remote-desktop-web-client-admin.md).
- Colore fisso problemi, l'abilitazione di rendering più vivace i colori nella sessione remota.
- Messaggi di errore rivisto correlati a errori relativi alle risorse remote del feed. 
- Aggiunta del supporto per più collegamenti di office, ad esempio Incolla speciale (Ctrl + Alt + V).
- Aggiunta di tasti di scelta rapida per gli utenti richiamare la chiave di Windows nella sessione remota (ALT+F3)
- Messaggio di errore aggiornate per gli utenti che tentano di eseguire l'autenticazione con una password scaduta.
- Feed aggiornato dell'interfaccia utente nella pagina tutte le risorse.
- Risolti dialoghi sovrapposti che si sono verificati durante la sessione di ristabilire la connessione.
- Correzione di ridimensionamento sull'icona di risorsa remota nella barra delle applicazioni di risorse.

## <a name="updates-for-version-1011"></a>Aggiornamenti per la versione 1.0.11
*Data di pubblicazione: 2/22/2019*

- Abilitare connessione desktop remoto senza un Gateway Desktop remoto in Windows Server 2019.
- Feed in ordine alfabetico (ad esempio RemoteApps prima, desktop secondo).
- Correzione di miglioramento della compatibilità di lettura dello schermo più bug di accessibilità.
- Aggiornato gli strumenti di compilazione.
- Varie correzioni di bug.

## <a name="updates-for-version-107"></a>Aggiornamenti per la versione 1.0.7
*Data di pubblicazione: 1/24/2019*

- Utilizzo non in linea nelle reti interne è ora supportato.
- Migliorato il rendering nel browser non Microsoft Edge.
- Il limite implementato per la ripetizione dei tentativi di recupero dei feed tenta di impedire attacchi DoS.
- Bug di accessibilità fisso, consentendo agli utenti con particolari di usare il client web.
- Migliorati i messaggi di errore visualizzati all'utente per gli errori di feed.
- Aggiunta Ctrl + Alt + Fine (Windows) e fn + controllo + opzione + tasti di scelta rapida CANC (Mac) per richiamare Ctrl + Alt + Canc nel computer remoto.
- Dati di telemetria migliorate per gli eventi di arresto anomalo del sistema.
- Migliorate le pipeline di compilazione e gli strumenti di compilazione.
- Varie correzioni di bug.

## <a name="updates-for-version-101"></a>Aggiornamenti per la versione 1.0.1
*Data di pubblicazione: 10/29/2018*

- Aggiunta un'opzione per **le informazioni di supporto di acquisizione** nella pagina di informazioni per diagnosticare i problemi.
- È ora supportata la modalità inPrivate.
- Supporto migliorato per tastiere non inglesi.
- Risolto un problema in cui le descrizioni comando con caratteri non inglesi è stato illustrato in modo non corretto.
- Problema di rendering di grafica fissa che ha interessato agli utenti di Chrome.
- Aggiornato il reindirizzamento di fuso orario con pieno supporto dell'ora legale.
- Migliorato il messaggio di errore per l'errore di memoria insufficiente.
- Varie correzioni di bug.

## <a name="updates-for-version-100"></a>Aggiornamenti per la versione 1.0.0
*Data di pubblicazione: 07/16/2018*

- Client web Desktop remoto è ora disponibile a livello generale.
- Gli amministratori possono globalmente disattivare la telemetria per il client web.
- Varie correzioni di bug.

## <a name="updates-for-version-090"></a>Aggiornamenti per la versione 0.9.0
*Data di pubblicazione: 07/05/2018*

- Esperienza all'interno del client web di accesso nuove.
- Non sono più richieste le credenziali quando si avvia una connessione desktop o nell'app (Single sign-on).
- Spostare il client web a un nuovo URL: <https://server_FQDN/RDWeb/webclient/index.html>
- Reindirizzamento aggiunto fuso orario.
- Varie correzioni di bug.

## <a name="updates-for-version-081"></a>Aggiornamenti per la versione 0.8.1
*Data di pubblicazione: 05/17/2018*

- Aggiornamenti per risolvere descritto in CVE-2018-0886 monitoraggio e aggiornamento di crittografia oracle CredSSP.
- Correzione di errori di connessione per alcune lingue quando è attivata la stampa.
- Messaggio di errore migliorato quando un gateway non fa parte della distribuzione.
- **Guida in linea** e **commenti e suggerimenti** sono state aggiunte le opzioni.

## <a name="updates-for-version-080"></a>Aggiornamenti per la versione 0.8.0
*Data di pubblicazione: 03/28/2018*

- Versione di anteprima pubblica iniziale del client web.
- Copiare e incollare testo dagli Appunti con **CTRL + C** e **CTRL + V**.
- Stampare un file con estensione PDF.
- Localizzato in 18 lingue.
 
