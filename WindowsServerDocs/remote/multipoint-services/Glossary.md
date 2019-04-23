---
title: Glossario
description: Definisce le parole, termini e concetti in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 4449d2d6fb87f74496b7d482a7a7263f703c7822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831112"
---
# <a name="glossary"></a>Glossario
**associare una stazione**  
Per specificare quale monitoraggio viene usato con quali stazione e periferiche, ad esempio una tastiera e mouse. Per stazioni connesse video dirette, questa operazione viene eseguita premendo una chiave specificata tastiera della stazione quando viene richiesto di farlo. Per USB zero client connesso le stazioni, ciò in genere avviene automaticamente.  
  
**hub bus con tecnologia**  
Hub che consente di disegnare tutte le potenzialità dell'interfaccia USB del computer. Hub bus con tecnologia non è necessario le connessioni di alimentazione separate. Tuttavia, molti dispositivi non funzionano con questo tipo di hub perché richiedono maggiore potenza di questo tipo di hub fornisce.  
  
**modalità console**  
Una delle due modalità di MultiPoint services è possibile iniziare. Quando il sistema è in modalità console, nessuna stazione sono disponibile per l'uso. Al contrario, tutti i monitoraggi vengono considerati come un unico desktop esteso per la sessione della console di sistema del computer. Modalità console viene in genere utilizzata per installare, aggiornare o configurare il software, che non può essere eseguito quando il computer è in modalità stazione. Vedere anche: *modalità stazione*.  
  
**stazione di Direct-video-connessi**  
Una stazione MultiPoint costituito da un monitoraggio che è direttamente connesso a un output video nel server e come minimo, include una tastiera e mouse che sono connessi al server tramite un hub USB.  
  
**Account utente di dominio**  
Un account utente che è ospitato in un computer di dominio. Account utente di dominio è possibile accedere da qualsiasi computer in cui è connesso al dominio e non sono associate a un particolare computer.  
  
**hub downstream**  
Hub che è connesso a un hub di stazione per aggiungere porte non è più disponibile per i dispositivi stazione. Un hub downstream non deve avere una tastiera collegata a esso.  
  
**hub esternamente spenta**  
Noto anche come un hub di alimentazione autonoma, l'hub accetta la potenza da un'unità di alimentazione esterno; Pertanto, può fornire potenza completa (fino a 500 mA) per ogni porta. Molti hub può fungere da hub basati su bus o alimentato esternamente.  
  
**Dispositivo di controllo consumer HID**  
Un HID Human Interface Device () è un dispositivo di computer che interagisce direttamente con gli esseri umani. Può accettare l'input dal o fornire output esseri umani. Esempi sono tastiera, mouse, trackball, touchpad, chiavetta puntamento, tabella grafici, joystick, scanner di impronta digitale, gamepad, webcam, visore VR e simulatori determinante. Un dispositivo di controllo consumer HID è una classe particolare di dispositivi HID che include i controlli volume dell'audio e tasti di scelta del browser e multimedia.  
  
**intermediate hub**  
Un hub di lunghezza compresa tra un *hub radice* sul server e un hub di stazione. Gli hub intermedi in genere vengono usati per aumentare il numero di porte disponibili per gli hub di stazioni o per estendere la distanza delle stazioni dal computer.  
  
**account utente locale**  
Un account utente in un computer specifico. Un account utente locale è disponibile solo nel computer in cui è definito l'account.  
  
**hub multifunzione**  
Visualizzare *USB zero-client*.  
  
**Sistema multiPoint Services**  
Raccolta di hardware e software che è costituito da un computer dotato di Windows Server 2016 già installato con il ruolo di servizi MultiPoint abilitato e almeno una stazione MultiPoint. Per altre informazioni sulle opzioni di layout di sistema, vedere [pianificazione del sito di MultiPoint Services](MultiPoint-services-Site-Planning.md)  
  
**partition**  
Una sezione di spazio su disco fisico che funziona come se si tratta di un disco separato.  
  
**stazione principale**  
Stazione di cui è il primo avvio all'avvio di servizi MultiPoint. La stazione principale è utilizzabile da un amministratore per accedere alle impostazioni e i menu di avvio. Quando non è utilizzato dall'amministratore, può essere utilizzato come stazione normale (non è necessario essere riservate esclusivamente per l'amministrazione). Monitor di stazione principale deve essere sempre connessi direttamente a un output video nel computer che esegue MultiPoint Services. Vedere anche: stazione.  
  
**RDP-over-LAN-stazione connesso**  
Una stazione che è un thin client, un desktop tradizionali o un computer portatile che connette ai servizi MultiPoint utilizzando Remote Desktop Protocol (RDP) tramite la rete locale (LAN).  
  
**hub radice**  
Un hub USB predefinito per il controller host sulla scheda madre del computer.  
  
**Dividi schermo**  
Una stazione in cui un solo monitor può essere utilizzato per visualizzare due desktop degli utenti indipendenti. Due set di hub, tastiere e mouse sono associati a un solo monitor. Uno di essi è associato al lato sinistro della finestra di monitoraggio e l'altro è associato a destra del monitoraggio.  
  
**stazione standard**  
A differenza di *stazione principale*, che può essere usato da un amministratore per i menu di avvio di accesso, stazioni standard non verranno visualizzati i menu di avvio e possono essere usati solo dopo che i servizi MultiPoint ha completato il processo di avvio . Vedere anche: stazione.  
  
*station*  
Endpoint di utente per la connessione al computer che esegue MultiPoint Services. Sono supportati tre tipi di espansione: stazioni direct-video-connessi e connessi tramite USB zero-client RDP-over-LAN-connessi. Per altre informazioni sulle stazioni, vedere [stazioni MultiPoint](MultiPoint-services-Stations.md).  
  
**hub di stazione**  
Un hub USB che è stato associato a un monitoraggio per creare una stazione MultiPoint. Si connette periferiche USB di MultiPoint Services. Vedere anche: *Client USB zero* e *hub USB*.  
  
**modalità stazione**  
Una delle due modalità di MultiPoint services è possibile iniziare. In genere, il sistema MultiPoint Services è in modalità stazione. In modalità stazione, le stazioni MultiPoint Services si comportano come se ogni stazione è un computer separato che esegue il sistema operativo Windows, e più utenti possono usare il sistema nello stesso momento. Vedere anche: *modalità console*.  
  
**Hub USB**  
Un hub USB multiporta generico espansione conforme alle specifiche USB universal serial bus () 2.0 o versione successiva. Tale hub dispongono in genere diverse porte USB, che consente a più dispositivi USB da collegare a una singola porta USB del computer. Gli hub USB sono in genere dispositivi separati che possono essere *spenti esternamente* oppure *basati su bus*. Altri dispositivi, come alcune tastiere e monitor video, possono includere un hub USB in fase di progettazione. Vedere anche: *Client USB zero*.  
  
**USB nel client Ethernet zero**  
Un client USB zero che si connette al computer tramite una connessione LAN anziché a una porta USB. Questo client viene visualizzato al server quando un dispositivo USB anche tramite i dati verrà inviato tramite la connessione Ethernet.  
  
**Client USB zero**  
Un hub di espansione che si connette al computer tramite una porta USB e consente la connessione di un'ampia gamma di dispositivi non USB all'hub. I client USB zero vengono prodotte dai produttori di hardware specifico, e richiedono l'installazione di un driver specifico del dispositivo. I client USB zero supportano la connessione di un monitor video (tramite VGA DVI e così via) e le periferiche (tramite USB, talvolta PS/2 e audio analogico). Il dispositivo USB zero client può essere *esternamente con tecnologia* oppure *basati su bus*. Vedere anche *hub USB*.  
  
**Stazione con connessione USB zero-client**  
Una stazione MultiPoint Services che è costituito da (come minimo) un monitor, tastiera e mouse, che sono connessi al server tramite un client USB zero.  
  
