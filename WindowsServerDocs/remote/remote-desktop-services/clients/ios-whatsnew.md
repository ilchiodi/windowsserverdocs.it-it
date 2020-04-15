---
title: Novità del client iOS
description: Informazioni sulle modifiche recenti apportate al client di Desktop remoto per iOS
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/03/2020
ms.localizationpriority: medium
ms.openlocfilehash: 69e33ef5f187672d948b6734a9ae7f68b54b7608
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854654"
---
# <a name="whats-new-in-the-ios-client"></a>Novità del client iOS

Aggiorniamo regolarmente il [client di Desktop remoto per iOS](remote-desktop-ios.md) aggiungendo nuove funzionalità e risolvendo i problemi. In questa pagina sono disponibili gli ultimi aggiornamenti.

>[!IMPORTANT]
>Microsoft si impegna a rendere questa app il miglior client Desktop remoto per iOS e a valorizzare il tuo feedback. Puoi segnalare eventuali problemi tramite **Guida** > **Segnala un problema**.

## <a name="updates-for-version-1005"></a>Aggiornamenti per la versione 10.0.5

*Data di pubblicazione: 09/03/2020*

Per la versione 10.0.5 sono stati messi a punto alcuni aggiornamenti di funzionalità e alcune correzioni di bug. Ecco le novità:

- I file RDP avviati vengono ora importati automaticamente (cerca l'interruttore nelle impostazioni generali).
- Puoi ora avviare i file RDP basati su iCloud che non sono ancora stati scaricati nell'app File.
- La sessione remota può ora estendersi al di sotto dell'indicatore Home negli iPhone (cerca l'interruttore nelle impostazioni di visualizzazione).
- È stato aggiunto il supporto per la digitazione di caratteri compositi con più sequenze di tasti, ad esempio é.
- È stato aggiunto il supporto per la tastiera mobile su schermo iPad.
- È stato aggiunto il supporto per la modifica delle proprietà delle fotocamere reindirizzate da una sessione remota.
- È stato corretto un bug di riconoscimento movimenti che determinava la mancata risposta del client durante la connessione a una sessione remota.
- Puoi ora attivare la modalità di cambio app con un solo scorrimento rapido (eccetto quando è attiva la modalità tocco con la sessione estesa nell'area indicatore Home).
- L'indicatore Home ora si nasconde automaticamente durante la connessione a una sessione remota e viene visualizzato nuovamente quando tocchi lo schermo.
- È stato aggiunto un tasto di scelta rapida per ottenere le impostazioni dell'app nel centro connessioni (**COMANDO +,** ).
- È stato aggiunto un tasto di scelta rapida per aggiornare tutte le aree di lavoro nel centro connessioni (**COMANDO + R**).
- È stato associato il tasto di scelta rapida di sistema per l'uscita durante la connessione a una sessione remota (**COMANDO +.** ).
- Sono stati corretti gli scenari in cui la tastiera su schermo di Windows nella sessione remota era troppo piccola.
- È stato implementato lo stato attivo della tastiera automatica nel centro connessioni per rendere più semplice l'immissione di dati.
- Premendo **INVIO** a una richiesta di credenziali ora la richiesta viene chiusa e riprende il flusso corrente.
- È stato corretto uno scenario in cui il client si arrestava in modo anomalo in caso di pressione di MAIUSC + OPZIONE + Freccia SINISTRA, Freccia SU o Freccia GIÙ.
- È stato corretto un errore di arresto anomalo del sistema che si verificava durante la rimozione di un dispositivo SwiftPoint.
- Sono stati corretti altri errori di arresto anomalo del sistema segnalati dagli utenti dopo il rilascio dell'ultima versione.

Microsoft ringrazia tutti gli utenti che hanno segnalato bug e hanno collaborato per la diagnosi dei problemi.

## <a name="updates-for-version-1004"></a>Aggiornamenti per la versione 10.0.4

*Data di pubblicazione: 03/02/2020*

È il momento di un altro aggiornamento. Microsoft ringrazia tutti gli utenti che hanno segnalato bug e hanno collaborato per la diagnosi dei problemi. Ecco le novità disponibili in questa versione:

- L'interfaccia utente di conferma viene ora visualizzata al momento dell'eliminazione di account utente e gateway.
- L'interfaccia utente di ricerca nel centro connessioni è stata leggermente rielaborata.
- Il suggerimento nome utente, se esistente, viene ora visualizzato nell'interfaccia utente della richiesta di credenziali all'avvio da un URI o file RDP.
- È stato risolto un problema per cui la tastiera estesa su schermo si estendeva al di sotto del notch dell'iPhone.
- È stato corretto un bug per cui le tastiere esterne smettevano di funzionare dopo la disconnessione e la riconnessione.
- È stato aggiunto il supporto per il tasto ESC sulle tastiere esterne.
- È stato corretto un bug per cui venivano visualizzati caratteri latini durante l'immissione di caratteri cinesi.
- È stato corretto un bug per cui un input in cinese rimaneva nella sessione remota dopo l'eliminazione.
- Sono stati corretti altri errori di arresto anomalo del sistema segnalati dagli utenti dopo il rilascio dell'ultima versione.

Microsoft apprezza tutti i commenti inviati tramite l'App Store, il feedback in-app e la posta elettronica e continuerà a dedicarsi al miglioramento dell'app in ogni versione.

## <a name="updates-for-version-1003"></a>Aggiornamenti per la versione 10.0.3

*Data di pubblicazione: 16/01/2020*

È il 2020 ed è il momento del primo rilascio dell'anno, il che significa nuove funzionalità e correzioni di bug. Ecco cosa è incluso in questo aggiornamento:

- È supportato l'avvio di connessioni da file RDP e URI RDP.
- È ora possibile comprimere le intestazioni dell'area di lavoro.
- Sono supportati lo zoom e la panoramica eseguiti contemporaneamente nella modalità puntatore del mouse.
- Un movimento di pressione prolungata nella modalità puntatore del mouse attiverà ora un clic con il pulsante destro del mouse nella sessione remota.
- È stato rimosso il gesto Force Touch per fare clic con il pulsante destro del mouse nella modalità puntatore del mouse.
- La schermata di cambio della modalità nella sessione supporta ora la disconnessione, anche se non è connessa alcuna app.
- La schermata di cambio della modalità nella sessione supporta ora la funzionalità degli elementi che scompaiono quando si tocca lo schermo.
- I PC e le app non vengono più riordinati automaticamente nella schermata di cambio della modalità nella sessione.
- È stata ingrandita l'area di hit test per il menu puntini di sospensione della visualizzazione anteprima del PC.
- Nella pagina delle impostazioni dei dispositivi di input è ora presente un collegamento ai dispositivi supportati.
- È stato corretto un bug che causava la visualizzazione ripetuta dell'interfaccia utente delle autorizzazioni Bluetooth all'avvio per alcuni utenti.
- Sono stati corretti altri errori di arresto anomalo del sistema segnalati dagli utenti dopo il rilascio dell'ultima versione.

## <a name="updates-for-version-1002"></a>Aggiornamenti per la versione 10.0.2

*Data di pubblicazione: 20/12/2019*

Microsoft si è impegnata nella correzione dei bug e nell'aggiunta di funzionalità utili. Ecco le novità di questa versione:

- È supportato l'input in giapponese e cinese sulle tastiere hardware.
- Nella visualizzazione elenco sul PC viene ora visualizzato il nome descrittivo dell'account utente associato, se disponibile.
- Il rendering dell'interfaccia utente delle autorizzazioni in first-run experience viene ora eseguito correttamente in modalità chiara.
- È stato corretto un errore di arresto anomalo che si verificava ogni volta che un utente premeva contemporaneamente OPZIONE e i tasti Freccia SU o GIÙ su una tastiera hardware.
- È stato aggiornato il layout della tastiera su schermo usato nell'interfaccia utente della richiesta di password per semplificare la ricerca del tasto barra rovesciata.
- Sono stati corretti altri errori di arresto anomalo del sistema segnalati dagli utenti dopo il rilascio dell'ultima versione.

## <a name="updates-for-version-1001"></a>Aggiornamenti per la versione 10.0.1

*Data di pubblicazione: 15/12/2019*

Ecco le novità di questa versione:

- Supporto per il servizio Desktop virtuale Windows.
- È stata aggiornata l'interfaccia utente del centro connessioni.
- È stata aggiornata l'interfaccia utente nella sessione.

## <a name="updates-for-version-1000"></a>Aggiornamenti per la versione 10.0.0

*Data di pubblicazione: 13/12/2019*

È passato un anno dall'ultimo aggiornamento del client Desktop remoto per iOS. Tuttavia, è ora disponibile un nuovo entusiasmante aggiornamento e molti altri verranno rilasciati regolarmente. Ecco le novità della versione 10.0.0:

- Supporto per il servizio Desktop virtuale Windows.
- Nuova interfaccia utente del Centro connessioni.
- Nuova interfaccia utente in sessione per il passaggio tra app e PC collegati.
- Nuovo layout per la tastiera su schermo ausiliaria.
- Supporto per la tastiera esterna migliorato.
- Supporto per il mouse Bluetooth SwiftPoint.
- Supporto per il reindirizzamento del microfono.
- Supporto per il reindirizzamento dell'archiviazione locale.
- Supporto per il reindirizzamento della fotocamera (disponibile solo per Windows 10, versione 1809 o successiva).
- Supporto per i nuovi dispositivi iPhone e iPad.
- Supporto per i temi scuro e chiaro.
- Controllo della capacità di blocco del telefono quando è collegato a un'app o a un PC remoto.
- Compressione della barra di connessione in sessione tenendo premuto il pulsante del logo Desktop remoto.

## <a name="updates-for-version-8142"></a>Aggiornamenti alla versione 8.1.42

*Data di pubblicazione: 06/20/2018*

- Risoluzione di problemi e miglioramenti delle prestazioni.

## <a name="updates-for-version-8141"></a>Aggiornamenti alla versione 8.1.41

*Data di pubblicazione: 03/28/2018*

- Aggiornamenti per la correzione oracolo di crittografia CredSSP descritta in CVE-2018-0886.

## <a name="how-to-report-issues"></a>Come segnalare i problemi

Microsoft si impegna a migliorare l'app e a valorizzare il tuo feedback. Per segnalare problemi, puoi passare a **Impostazioni** > **Segnala un problema** nel client.
