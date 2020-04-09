---
title: Glossario
description: Definisce parole, termini e concetti in Servizi MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9a5f76f0f41d9ff1726a1a468fde7f53b6a7634d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859224"
---
# <a name="glossary"></a>Glossario
**associare una stazione**  
Per specificare quale monitor viene utilizzato con la stazione e i dispositivi periferici, ad esempio una tastiera e un mouse. Per le stazioni con connessione video diretta, questa operazione viene eseguita premendo un tasto specificato sulla tastiera della stazione quando richiesto. Per le stazioni USB zero client connesse, questa situazione si verifica in genere automaticamente.  
  
**hub alimentato da bus**  
Hub che disegna tutta la potenza dall'interfaccia USB del computer. Gli hub basati su bus non necessitano di connessioni di alimentazione separate. Tuttavia, molti dispositivi non funzionano con questo tipo di hub perché richiedono una potenza maggiore di quella fornita da questo tipo di hub.  
  
**modalità console**  
È possibile avviare una delle due modalità MultiPoint Services. Quando il sistema è in modalità console, non sono disponibili stazioni da usare. Al contrario, tutti i monitoraggi vengono considerati come un singolo desktop esteso per la sessione della console del computer. La modalità console viene in genere utilizzata per installare, aggiornare o configurare il software, che non può essere eseguita quando il computer è in modalità stazione. Vedere anche: *modalità stazione*.  
  
**Direct-video-stazione connessa**  
Una stazione MultiPoint costituita da un monitoraggio connesso direttamente a un output video nel server e, come minimo, include una tastiera e un mouse connessi al server tramite un hub USB.  
  
**account utente di dominio**  
Un account utente ospitato in un computer di dominio. È possibile accedere agli account utente di dominio da qualsiasi computer connesso al dominio e che non sono collegati a un particolare computer.  
  
**hub downstream**  
Hub connesso a un hub di stazione per aggiungere altre porte disponibili per i dispositivi di stazione. Un hub downstream non deve avere una tastiera collegata.  
  
**hub alimentato esternamente**  
Detto anche hub autoalimentato, questo hub prende la sua potenza da un'unità di alimentazione esterna; Pertanto, può fornire energia completa (fino a 500 mA) a ogni porta. Molti hub possono funzionare come hub alimentato da bus o esternamente.  
  
**Dispositivo di controllo consumer HID**  
Un dispositivo di interfaccia umana (HID) è un dispositivo computer che interagisce direttamente con gli utenti. Potrebbe richiedere input o recapitare l'output agli utenti. Gli esempi sono la tastiera, il mouse, la trackball, il touchpad, il puntamento, la tabella grafica, il joystick, lo scanner di impronte digitali, il gamepad, la webcam, l'auricolare e i dispositivi simulatori Un dispositivo di controllo consumer HID è una classe particolare di dispositivi HID che include controlli volume audio e chiavi di controllo del browser e multimediali.  
  
**hub intermedio**  
Hub tra un hub *radice* nel server e un hub di stazione. Gli hub intermedi vengono in genere usati per aumentare il numero di porte disponibili per gli hub delle stazioni o per estendere la distanza delle stazioni dal computer.  
  
**account utente locale**  
Un account utente in un computer specifico. Un account utente locale è disponibile solo nel computer in cui è definito l'account.  
  
**Hub multifunzione**  
Vedere *USB zero client*.  
  
**Sistema MultiPoint Services**  
Un insieme di hardware e software costituito da un computer in cui è installato Windows Server 2016 con il ruolo Servizi MultiPoint abilitato e almeno una stazione MultiPoint. Per altre informazioni sulle opzioni di layout del sistema, vedere [pianificazione del sito di servizi multipoint](MultiPoint-services-Site-Planning.md)  
  
**partizione**  
Sezione di spazio su un disco fisico che funziona come se fosse un disco separato.  
  
**stazione primaria**  
La stazione che è la prima ad essere avviata quando vengono avviati i servizi MultiPoint. La stazione primaria può essere usata da un amministratore per accedere ai menu e alle impostazioni di avvio. Quando non viene utilizzato dall'amministratore, può essere utilizzato come stazione normale (non è necessario che sia riservato esclusivamente per l'amministrazione). Il monitoraggio della stazione primaria deve essere sempre connesso direttamente a un output video nel computer in cui è in esecuzione MultiPoint Services. Vedere anche: stazione.  
  
**Stazione connessa a RDP-over-LAN**  
Una stazione che è un thin client, un desktop tradizionale o un computer portatile che si connette a MultiPoint Services usando Remote Desktop Protocol (RDP) tramite la rete locale (LAN).  
  
**Hub radice**  
Un hub USB incorporato nel controller host sulla scheda madre di un computer.  
  
**schermata di suddivisione**  
Stazione in cui è possibile usare un singolo monitoraggio per visualizzare due desktop utente indipendenti. Due set di hub, tastiere e topi sono associati a un singolo monitor. Un set è associato al lato sinistro del monitoraggio e l'altro set è associato al lato destro del monitoraggio.  
  
**Stazione standard**  
A differenza della *stazione primaria*, che può essere usata da un amministratore per accedere ai menu di avvio, le stazioni standard non visualizzano i menu di avvio e possono essere usate solo dopo che MultiPoint Services ha completato il processo di avvio. Vedere anche: stazione.  
  
*Stazione*  
Endpoint utente per la connessione al computer che esegue MultiPoint Services. Sono supportati tre tipi di stazione: stazioni connesse Direct-video-connected, USB-zero-client-connected e RDP-over-LAN. Per altre informazioni sulle stazioni, vedere [multipoint stations](MultiPoint-services-Stations.md).  
  
**Hub stazione**  
Un hub USB che è stato associato a un monitoraggio per creare una stazione MultiPoint. Connette i dispositivi USB periferici a MultiPoint Services. Vedere anche: *USB zero client* e *hub USB*.  
  
**modalità stazione**  
È possibile avviare una delle due modalità MultiPoint Services. In genere, il sistema MultiPoint Services è in modalità stazione. Quando si è in modalità stazione, le stazioni di servizi MultiPoint si comportano come se ogni stazione fosse un computer separato che esegue il sistema operativo Windows e più utenti possono utilizzare il sistema nello stesso momento. Vedere anche: *modalità console*.  
  
**Hub USB**  
Hub di espansione USB multiporta generico conforme alle specifiche USB (Universal Serial Bus) 2,0 o versioni successive. Tali Hub sono in genere dotati di diverse porte USB, che consentono la connessione di più dispositivi USB a una singola porta USB del computer. Gli hub USB sono in genere dispositivi distinti che possono essere *alimentati esternamente* o in *bus*. Alcuni altri dispositivi, ad esempio alcune tastiere e monitor video, possono incorporare un hub USB nella propria progettazione. Vedere anche: *USB zero client*.  
  
**USB over Ethernet zero client**  
Un client USB zero che si connette al computer tramite una connessione LAN anziché una porta USB. Questo client viene visualizzato sul server come dispositivo USB anche attraverso i dati inviati tramite la connessione Ethernet.  
  
**USB zero client**  
Un hub di espansione che si connette al computer tramite una porta USB e consente la connessione di un'ampia gamma di dispositivi non USB all'hub. I client USB zero sono prodotti da produttori di hardware specifici e richiedono l'installazione di un driver specifico per il dispositivo. I client USB zero supportano la connessione di un monitor video (tramite VGA, DVI e così via) e le periferiche (tramite USB, a volte PS/2 e audio analogico). Il client USB zero può essere *alimentato esternamente* o *alimentato da bus*. Vedere anche *hub USB*.  
  
**Stazione USB zero client connessa**  
Una stazione MultiPoint Services costituita come minimo da un monitor, una tastiera e un mouse, che sono connessi al server tramite un client USB zero.  
  
