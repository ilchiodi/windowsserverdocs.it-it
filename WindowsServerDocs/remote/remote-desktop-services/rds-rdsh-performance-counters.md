---
title: Usa i contatori delle prestazioni per diagnosticare i problemi di applicazione velocità di risposta su host sessione Desktop remoto
description: È l'app in esecuzione lenta su Servizi Desktop remoto? Informazioni sui contatori delle prestazioni che puoi usare per diagnosticare i problemi di prestazioni di app su host sessione Desktop remoto
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 241b2b776a68cf5aec68a4d331201a07f0e5ea53
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133717"
---
# Usa i contatori delle prestazioni per diagnosticare i problemi di prestazioni di app su host sessione Desktop remoto

Uno dei problemi più difficili da diagnosticare viene prestazioni scarse, ovvero le applicazioni eseguono lento o non rispondere. In genere, iniziare la diagnosi dalla raccolta della CPU, memoria, disco di input/output e altre metriche e quindi utilizzare strumenti come Windows Performance Analyzer per provare a capire cosa provoca il problema. Purtroppo nella maggior parte dei casi questi dati non consentono di identificare la causa principale perché contatori delle relative risorse sono varianti frequenti e grande. Questo rende difficile da leggere i dati e ne esegue la correlazione con il problema segnalato. Per risolvere più rapidamente i problemi di prestazioni dell'app, abbiamo aggiunto alcune nuove contatori delle prestazioni (disponibile [per il download](#download-windows-server-insider-software) tramite il [Programma Windows Insider](https://insider.windows.com)) che misurano i flussi di input utente.

Il contatore di ritardo di Input utente può aiutarti a identificare rapidamente la causa principale per utente finale non validi che esperienze RDP. Questo contatore misura quanto tutti gli utenti di input (ad esempio, utilizzo della tastiera o mouse) rimangono nella coda prima viene individuato da un processo e il contatore funziona in sessioni locali e remote.

L'immagine seguente mostra una rappresentazione approssimativa del flusso di input utente dal client all'applicazione.

![Desktop remoto - utente i flussi di input dal client Desktop remoto gli utenti all'applicazione](.\media\rds-user-input.png)

Il contatore di ritardo di Input utente misura il delta max (all'interno di un intervallo di tempo) tra l'input viene accodato e quando viene individuato dall'app in un [ciclo di messaggi tradizionali](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), come illustrato nel seguente diagramma di flusso:

![Desktop remoto - flusso contatore delle prestazioni di ritardo l'input dell'utente](.\media\rds-user-input-delay.png)

Un dettaglio importante del contatore è che segnala il ritardo di input utente massimo all'interno di un intervallo di tempo configurabile. Questo è il più lungo tempo che necessario per l'input di raggiungere l'applicazione, che può influire sulla velocità di azioni importanti e visibile come digitazione.

Ad esempio, nella tabella seguente, il ritardo di input utente sarebbe venga segnalato come ms 1.000 entro questo intervallo. Il contatore segnala l'utente più lenta input ritardo nell'intervallo di perché percezione dell'utente di "lento" viene determinata al momento input minima (il valore massimo) si verificano, non la velocità media di tutti gli input totale.

|Numero| 0 | 1 | 2 |
|------|---|---|---|
|Ritardo |16 ms| 20 ms| 1.000 ms|

## Abilitare e usare i nuovo contatori delle prestazioni

Per usare questi nuovi contatori delle prestazioni, è innanzitutto necessario abilitare una chiave del Registro di sistema eseguendo questo comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Se stai usando Windows 10, versione 1809 o versione successiva oppure Windows Server 2019 o versione successiva, non devi abilitare la chiave del Registro di sistema.

Successivamente, riavvia il server. Quindi, Apri il monitoraggio delle prestazioni e seleziona il segno più (+), come illustrato nella schermata seguente.

![Contatore delle prestazioni di ritardo di input di Desktop remoto - uno screenshot che mostra come aggiungere l'utente](.\media\rds-add-user-input-counter-screen.png)

Dopo aver in questo modo, dovresti vedere la finestra di dialogo Aggiungi contatori, in cui è possibile selezionare **Il ritardo di Input utente per ogni processo** o **Il ritardo di Input per ogni sessione utente**.

![Desktop remoto - uno screenshot che mostra come aggiungere il ritardo di input utente per ogni sessione](.\media\rds-user-delay-per-session.png)

![Desktop remoto - uno screenshot che mostra come aggiungere il ritardo di input utente per ogni processo](.\media\rds-user-delay-per-process.png)

Se selezioni **Il ritardo di Input utente per ogni processo**, vedrai le **istanze dell'oggetto selezionato** (in altre parole, i processi) in ```SessionID:ProcessID <Process Image>``` formato.

Ad esempio, se l'app Calcolatrice è in esecuzione in una [sessione ID 1](https://msdn.microsoft.com/library/ms524326.aspx), vedrai ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Non tutti i processi sono inclusi. Non vedrai i processi in esecuzione come sistema.

Il contatore inizia a ritardo di input utente appena aggiungerlo. Tieni presente che la scala massima è impostata su 100 (ms) per impostazione predefinita. 

![Desktop remoto - un esempio di attività per il ritardo di Input utente per ogni processo in Performance Monitor](.\media\rds-sample-user-input-delay-perfmon.png)

Successivamente, esaminiamo il **Ritardo di Input utente per ogni sessione**. Ci sono casi per ogni ID della sessione e relativi contatori mostrano il ritardo di input utente di un processo all'interno della sessione specificata. Inoltre, esistono due istanze denominate "Max" (il numero massimo di utenti input ritardo per tutte le sessioni) e "Media" (l'acorss di Media tutte le sessioni).

Questa tabella mostra un esempio visivo di questi casi. (Puoi ottenere le stesse informazioni PerfMon passando al tipo di Report grafico.)

|Tipo di contatore|Nome dell'istanza|Ritardo segnalato (ms)|
|---------------|-------------|-------------------|
|Ritardo di Input utente per ogni processo|1:4232 < Calculator.exe >|  200|
|Ritardo di Input utente per ogni processo|2:1000 < Calculator.exe >|  16|
|Ritardo di Input utente per ogni processo|1: 2 000 < Calculator.exe >|  32|
|Ritardo di Input per ogni sessione utente|1|    200|
|Ritardo di Input per ogni sessione utente|2|    16|
|Ritardo di Input per ogni sessione utente|Average|  108|
|Ritardo di Input per ogni sessione utente|Max|  200|

## Contatori usati in un sistema di overload

Ora diamo un'occhiata a ciò che verrà visualizzata nel report se le prestazioni di un'app sono danneggiata. Il grafico seguente mostra le letture per gli utenti che lavorano in remoto in Microsoft Word. In questo caso, le prestazioni del server host sessione Desktop remoto peggiorano nel tempo come accedono più utenti.

![Desktop remoto - un grafico di prestazioni di esempio per il server host sessione Desktop remoto in Microsoft Word](.\media\rds-user-input-perf-graph.png)

Ecco come leggere le righe del grafico:

- La riga rosa Mostra il numero di sessioni di accesso nel server.
- La riga rossa è l'utilizzo della CPU.
- La linea verde è il ritardo di input utente massima per tutte le sessioni.
- La riga blu (visualizzata come bianco in questo grafico) rappresenta il ritardo input utente medio per tutte le sessioni.

Potrai notare che esiste una correlazione tra i picchi della CPU e il ritardo input utente, ovvero la CPU aumenta di più sull'utilizzo, l'input dell'utente aumenta di ritardo. Inoltre, poiché verranno aggiunti altri utenti al sistema, utilizzo della CPU ottiene più vicino al 100%, causando picchi di ritardo input utente più frequenti. Mentre il contatore è molto utile nei casi in cui il server viene eseguito all'esterno delle risorse, è possibile utilizzare anche si traccia il ritardo di input utente relative a un'applicazione specifica.

## Opzioni di configurazione

È importante tenere presente quando si utilizza il contatore delle prestazioni è che segnala il ritardo input utente in un intervallo di 1.000 ms per impostazione predefinita. Se si imposta la proprietà di intervallo esempio contatore delle prestazioni (come illustrato nella schermata seguente) per qualsiasi elemento diverso, il valore segnalato sarà non corretto.

![Desktop remoto - le proprietà per il monitoraggio delle prestazioni](.\media\rds-user-input-perfmon-properties.png)

Per risolvere questo problema, è possibile impostare la seguente chiave del Registro di sistema in modo che corrisponda l'intervallo (in millisecondi) che si desidera utilizzare. Ad esempio, se si modifica esempio ogni x secondi per 5 secondi, dobbiamo impostare questa chiave a 5000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Se stai usando Windows 10, versione 1809 o versione successiva oppure Windows Server 2019 o versione successiva, non devi impostare LagCounterInterval per risolvere il contatore delle prestazioni.

Abbiamo anche aggiunto un paio di chiavi che possono rivelarsi utili con la stessa chiave del Registro di sistema:

**LagCounterImageNameFirst** , ovvero impostare questa chiave `DWORD 1` (valore predefinito 0 o la chiave non esiste). In questo modo i nomi dei contatori a "Nome immagine < SessionID:ProcessId >". Ad esempio, "explorer < 1:7964 >". Ciò è utile se vuoi che in base al nome dell'immagine.

**LagCounterShowUnknown** , ovvero impostare questa chiave `DWORD 1` (valore predefinito 0 o la chiave non esiste). Questo mostra i processi in esecuzione e servizi di sistema. Alcuni processi verranno visualizzate con una sessione impostata come "?."

Ecco che aspetto se puoi attivare entrambe le chiavi:

![Desktop remoto - performance monitor con entrambe le chiavi su](.\media\rds-user-input-delay-with-two-counters.png)

## Usa i contatori di nuovo con strumenti non Microsoft

Strumenti di monitoraggio possono utilizzare il contatore tramite l' [API di Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## Scarica il software di Windows Server Insider

Partecipanti al programma Insider registrati possono spostarsi direttamente per la [pagina di download di Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) per ottenere i download di software più recenti di Insider.  Per informazioni su come eseguire la registrazione come un Insider, vedi [Guida introduttiva a Server](https://insider.windows.com/en-us/for-business-getting-started-server/).

## Condividere il tuo feedback

Puoi inviare feedback per questa funzionalità tramite Hub di Feedback. Seleziona **App > tutte le altre App** e Includi "contatori delle prestazioni di servizi desktop remoto, ovvero performance monitor di" nel titolo dell'annuncio.

Per idee funzionalità generali, visita la [pagina UserVoice Servizi Desktop remoto](https://aka.ms/uservoice-rds).
