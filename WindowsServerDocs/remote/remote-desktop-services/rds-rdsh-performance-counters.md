---
title: Usare i contatori delle prestazioni per diagnosticare i problemi di velocità di risposta dell'applicazione su host sessione Desktop remoto
description: È l'app in esecuzione lenta su Servizi Desktop remoto? Informazioni sui contatori delle prestazioni che è possibile usare per diagnosticare i problemi delle prestazioni dell'app su host sessione Desktop remoto
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: f9aafaa34d5c16e45681e88b1ce60e99a9ad2842
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447088"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Usare i contatori delle prestazioni per diagnosticare i problemi delle prestazioni dell'app su host sessione Desktop remoto

Uno dei problemi più difficili da diagnosticare è scarse prestazioni dell'applicazione, ovvero le applicazioni sono lenti o non risponde. In genere, si avvia la diagnostica per la raccolta della CPU, memoria, input/output su disco e altre metriche e usare quindi strumenti come Windows Performance Analyzer per tentare di determinare la causa del problema. Purtroppo nella maggior parte delle situazioni questi dati non consentono di identificare la causa radice perché contatori del consumo di risorse dispone di grandi dimensioni e frequenti variazioni. Questo rende difficile la lettura dei dati e lo associa al problema segnalato. Che consentono di più rapidamente di risolvere i problemi di prestazioni di app, sono stati aggiunti alcuni nuovi contatori delle prestazioni (disponibile [per scaricare](#download-windows-server-insider-software) tramite il [Windows Insider Program](https://insider.windows.com)) utente misura i flussi di input.

Il contatore di ritardo di Input utente consente di identificare rapidamente la causa principale per l'utente finale non valido che si verifica nel RDP. Questo contatore misura il tempo qualsiasi utente di input (ad esempio utilizzo del mouse o tastiera) rimane nella coda prima che viene prelevato da un processo e il contatore funziona in sessioni locali e remote.

L'immagine seguente mostra una rappresentazione approssimativa del flusso di input utente dal client all'applicazione.

![Desktop remoto - utente i flussi di input dal client Desktop remoto agli utenti all'applicazione](./media/rds-user-input.png)

Il contatore di ritardo di Input utente misura il delta massimo (all'interno di un intervallo di tempo) tra l'input accodata e quando viene prelevato dall'app, in un [ciclo di messaggi tradizionale](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), come illustrato nel diagramma di flusso seguente:

![Desktop remoto - flow di contatore delle prestazioni ritardo l'input dell'utente](./media/rds-user-input-delay.png)

Un dettaglio importante di questo contatore è che segnala il ritardo di input massimo di utenti all'interno di un intervallo configurabile. Questo è il tempo massimo impiegato da un input raggiungere l'applicazione, che possa influire sulla velocità delle operazioni importanti ed è visibile, ad esempio la digitazione.

Ad esempio, nella tabella seguente, il ritardo di input utente sarebbe stato segnalato come 1.000 millisecondi entro questo intervallo. Il contatore indica il più lento di input degli utenti ritardo nell'intervallo poiché la percezione di "lento" è determinata dall'ora di input più lentamente (il valore massimo) si verifica, non la velocità media di tutti gli input totale.

|Numero| 0 | 1 | 2 |
|------|---|---|---|
|Ritardo |16 ms| 20 ms| 1.000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Abilitare e usare i nuovi contatori di prestazioni

Per usare questi nuovi contatori delle prestazioni, è innanzitutto necessario abilitare una chiave del Registro di sistema eseguendo questo comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Se si usa Windows 10, versione 1809 o versioni successive o Windows Server 2019 o in un secondo momento, non sarà necessario abilitare la chiave del Registro di sistema.

Successivamente, riavviare il server. Quindi, aprire Performance Monitor e selezionare il segno più (+), come illustrato nella schermata riportata di seguito.

![Contatore delle prestazioni ritardo di input di Desktop remoto - screenshot che illustra come aggiungere l'utente](./media/rds-add-user-input-counter-screen.png)

Dopo questa operazione, verrà visualizzata la finestra di dialogo Aggiungi contatori in cui è possibile selezionare **ritardo di Input utente per ogni processo** oppure **ritardo di Input utente per ogni sessione**.

![Desktop remoto - screenshot che illustra come aggiungere il ritardo di input utente per ogni sessione](./media/rds-user-delay-per-session.png)

![Desktop remoto - screenshot che illustra come aggiungere il ritardo di input utente per ogni processo](./media/rds-user-delay-per-process.png)

Se si seleziona **ritardo di Input utente per ogni processo**, si noterà il **istanze dell'oggetto selezionato** (in altre parole, i processi) in ```SessionID:ProcessID <Process Image>``` formato.

Ad esempio, se l'app Calcolatrice è in esecuzione in un [1 ID di sessione](https://msdn.microsoft.com/library/ms524326.aspx), si noterà ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Non tutti i processi sono inclusi. Sarà possibile vedere tutti i processi in esecuzione come SYSTEM.

Il contatore inizi a inviare report ritardo input utente, non appena si aggiungerlo. Si noti che la scala massima è impostata su 100 (ms) per impostazione predefinita. 

![Desktop remoto: un esempio di attività per il ritardo di Input utente per ogni processo in Performance Monitor](./media/rds-sample-user-input-delay-perfmon.png)

Successivamente, verrà ora esaminato il **ritardo di Input utente per ogni sessione**. Sono presenti istanze per ogni ID di sessione e i relativi contatori mostrano il ritardo di input utente di tutti i processi all'interno della sessione specificata. Inoltre, sono presenti due istanze denominate "Max" (il massimo input ritardo utente tra tutte le sessioni) e "Average" (il acorss di medie tutte le sessioni).

Questa tabella mostra un esempio visivo di queste istanze. (È possibile ottenere le stesse informazioni in Perfmon passando il tipo di grafico di Report.)

|Tipo di contatore|Nome istanza|Ritardo segnalata (ms)|
|---------------|-------------|-------------------|
|Ritardo di Input utente per ogni processo|1:4232 < Calculator.exe >|  200|
|Ritardo di Input utente per ogni processo|2:1000 < Calculator.exe >|  16|
|Ritardo di Input utente per ogni processo|1: 2 000 < Calculator.exe >|  32|
|Ritardo di Input utente per ogni sessione|1|    200|
|Ritardo di Input utente per ogni sessione|2|    16|
|Ritardo di Input utente per ogni sessione|Media|  108|
|Ritardo di Input utente per ogni sessione|Max|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Contatori utilizzati in un sistema di overload

A questo punto vediamo cosa verrà visualizzato nel report se per un'app per le prestazioni risultano ridotte. Il grafico seguente mostra le letture agli utenti che lavorano in remoto in Microsoft Word. In questo caso, le prestazioni del server host sessione Desktop remoto comporta una riduzione nel tempo come accedono più utenti.

![Desktop remoto: un grafico delle prestazioni di esempio per il server host sessione Desktop remoto che esegue Microsoft Word](./media/rds-user-input-perf-graph.png)

Di seguito viene illustrato come leggere le linee del grafico:

- La riga rosa indica il numero di sessioni di accesso nel server.
- La linea rossa è l'utilizzo della CPU.
- La linea verde è il ritardo di input utente massimo in tutte le sessioni.
- La linea blu (visualizzata come colore nero in questo grafico) rappresenta il ritardo input utente medio tra tutte le sessioni.

Si noterà che vi sia una correlazione tra i picchi di CPU e il ritardo di input utente, ovvero quando la CPU diventa maggiore utilizzo, l'input utente aumenti di ritardo. Inoltre, più utenti vengono aggiunti al sistema, l'utilizzo della CPU ottiene più vicino al 100%, leader a picchi di ritardo input utente più frequenti. Mentre questo contatore è molto utile nei casi in cui il server esaurisce le risorse, è possibile usarlo anche per tenere traccia di ritardo di input utente correlato a un'applicazione specifica.

## <a name="configuration-options"></a>Opzioni di configurazione

Un aspetto importante da ricordare quando si usa questo contatore delle prestazioni è che segnala il ritardo input utente in un intervallo di 1.000 ms per impostazione predefinita. Se si imposta la proprietà di interval campione del contatore delle prestazioni (come illustrato nello screenshot seguente) a qualsiasi elemento diverso, il valore restituito sarà errato.

![Desktop remoto, le proprietà per il monitoraggio delle prestazioni](./media/rds-user-input-perfmon-properties.png)

Per risolvere questo problema, è possibile impostare la chiave del Registro di sistema seguente in modo che corrispondano l'intervallo, in millisecondi, che si desidera utilizzare. Ad esempio, se si modifica esempio ogni x secondi a 5 secondi, è necessario impostare questa chiave su 5000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Se si usa Windows 10, versione 1809 o versioni successive o Windows Server 2019 o in un secondo momento, non è necessario impostare LagCounterInterval per risolvere il contatore delle prestazioni.

È stata anche aggiunta una coppia di chiavi che potrebbe risultare utile nella stessa chiave del Registro di sistema:

**LagCounterImageNameFirst** : impostare questa chiave su `DWORD 1` (valore predefinito 0 o la chiave non esiste). In questo modo i nomi dei contatori a "Nome immagine < SessionID:ProcessId >". Ad esempio, "Esplora < 1:7964 >". Ciò è utile se si desidera eseguire l'ordinamento in base al nome di immagine.

**LagCounterShowUnknown** : impostare questa chiave su `DWORD 1` (valore predefinito 0 o la chiave non esiste). Vengono visualizzati tutti i processi in esecuzione come servizi o di sistema. Verranno visualizzati alcuni processi con la sessione impostata come "?."

Questo è il risultato se attivare entrambe le chiavi:

![Desktop remoto - il monitoraggio delle prestazioni con entrambe le chiavi in](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Usando i nuovi contatori con strumenti non Microsoft

Strumenti di monitoraggio possono utilizzare questo contatore con il [API Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Scaricare il software Server di Windows Insider

Insider registrati possono navigare direttamente nel [pagina di download di Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) per ottenere il software più recente di Insider download.  Per informazioni su come registrarsi come Insider, vedere [Introduzione a Server](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Condividi i tuoi commenti

È possibile inviare commenti e suggerimenti per questa funzionalità tramite l'Hub di commenti e suggerimenti. Selezionare **le app > tutte le altre app** e includono "contatori delle prestazioni di servizi desktop remoto: monitoraggio delle prestazioni" nel titolo del post.

Per suggerimenti generali di funzionalità, visitare il [pagina UserVoice di servizi desktop remoto](https://aka.ms/uservoice-rds).
