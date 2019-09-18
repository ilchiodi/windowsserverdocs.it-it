---
title: Usare i contatori delle prestazioni per diagnosticare i problemi di velocità di risposta dell'applicazione su host sessione Desktop remoto
description: L'app in esecuzione su Servizi Desktop remoto è lenta? Informazioni sui contatori delle prestazioni che è possibile usare per diagnosticare i problemi delle prestazioni dell'app su host sessione Desktop remoto
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 3eb1e4b6da971d788383b8facbf8bbcbe00a5953
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870915"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Usare i contatori delle prestazioni per diagnosticare i problemi di prestazioni dell'applicazione su host sessione Desktop remoto

> Si applica a: Windows Server 2019, Windows 10

Uno dei problemi più difficili da diagnosticare sono le scarse prestazioni dell'applicazione, ovvero quando le applicazioni sono lente o non rispondono. In genere, si avvia la diagnostica per la raccolta di CPU, memoria, input/output su disco e altre metriche e quindi si usano strumenti come Windows Performance Analyzer per tentare di determinare la causa del problema. Purtroppo nella maggior parte delle situazioni questi dati non consentono di identificare la causa radice, perché i contatori del consumo di risorse sono soggetti a variazioni significative e frequenti. Questo rende difficile la lettura dei dati e l'associazione al problema segnalato. Per risolvere più rapidamente i problemi di prestazioni delle app, sono stati aggiunti alcuni nuovi contatori delle prestazioni (disponibili [per il download](#download-windows-server-insider-software) tramite il [programma Windows Insider](https://insider.windows.com)) che misurano i flussi di input dell'utente.

>[!NOTE]
>Il contatore User Input Delay (Ritardo input utente) è compatibile solo con:
> - Windows Server 2019 o versione successiva
> - Windows 10 versione 1809 o successiva

Il contatore di ritardo di input dell'utente consente di identificare rapidamente la causa principale per le esperienze RDP negative dell'utente finale. Questo contatore misura per quanto tempo qualsiasi input dell'utente (ad esempio utilizzo del mouse o tastiera) rimane in coda prima di venire prelevato da un processo e il contatore funziona in sessioni locali e remote.

L'immagine seguente mostra una rappresentazione approssimativa del flusso di input dell'utente dal client all'applicazione.

![Desktop remoto: flussi di input dell'utente dal client Desktop remoto all'applicazione](./media/rds-user-input.png)

Il contatore di ritardo di input utente misura il delta massimo (all'interno di un intervallo di tempo) tra l'input in coda e il momento in cui viene prelevato dall'app in un [ciclo di messaggi tradizionale](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), come illustrato nel diagramma di flusso seguente:

![Desktop remoto: flusso del contatore delle prestazioni ritardo di input dell'utente](./media/rds-user-input-delay.png)

Un dettaglio importante di questo contatore è che segnala il ritardo di input massimo dell'utente all'interno di un intervallo configurabile. Questo è il tempo massimo impiegato da un input per raggiungere l'applicazione, che può influire sulla velocità di operazioni importanti e visibili, ad esempio la digitazione.

Ad esempio, nella tabella seguente, il ritardo di input dell'utente sarebbe stato segnalato come 1.000 millisecondi entro questo intervallo. Il contatore indica il ritardo di input più lento di dell'utente nell'intervallo perché la percezione di "lento" è determinata dal tempo di input più lento (il valore massimo) che si verifica, non dalla velocità media di tutti gli input totali.

|Numero| 0 | 1 | 2 |
|------|---|---|---|
|Ritardo |16 ms| 20 ms| 1\.000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Abilitare e usare i nuovi contatori delle prestazioni

Per usare questi nuovi contatori delle prestazioni, è innanzitutto necessario abilitare una chiave del Registro di sistema eseguendo questo comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Se si usa Windows 10, versione 1809 o versioni successive o Windows Server 2019 o versioni successive, non è necessario abilitare la chiave del Registro di sistema.

Quindi riavviare il server. Quindi, aprire Performance Monitor e selezionare il segno più (+), come illustrato nella schermata seguente.

![Desktop remoto: schermata che illustra come aggiungere il contatore delle prestazioni di ritardo di input dell'utente](./media/rds-add-user-input-counter-screen.png)

Dopo questa operazione, verrà visualizzata la finestra di dialogo Aggiungi contatori in cui è possibile selezionare **Ritardo di input dell'utente per processo** oppure **Ritardo di input dell'utente per sessione**.

![Desktop remoto: schermata che illustra come aggiungere Ritardo di input dell'utente per sessione](./media/rds-user-delay-per-session.png)

![Desktop remoto: schermata che illustra come aggiungere Ritardo di input dell'utente per processo](./media/rds-user-delay-per-process.png)

Se si seleziona **Ritardo di input dell'utente per processo**, si visualizzeranno le **istanze dell'oggetto selezionato** (in altre parole, i processi) in formato ```SessionID:ProcessID <Process Image>```.

Ad esempio, se l'app Calcolatrice è in esecuzione nell'[ID sessione 1](https://msdn.microsoft.com/library/ms524326.aspx), si visualizzerà ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Non tutti i processi sono inclusi. Non sarà possibile vedere tutti i processi in esecuzione come SYSTEM.

Appena aggiunto il contatore inizia a inviare report sul ritardo dell'input dell'utente. La scala massima è impostata su 100 (ms) per impostazione predefinita. 

![Desktop remoto: un esempio di attività per Ritardo di input dell'utente per processo in Performance Monitor](./media/rds-sample-user-input-delay-perfmon.png)

È ora possibile esaminare il **Ritardo dell'input dell'utente per sessione**. Sono presenti istanze per ogni ID sessione e i relativi contatori mostrano il ritardo dell'input dell'utente di tutti i processi all'interno della sessione specificata. Inoltre, sono presenti due istanze denominate "Max" (il ritardo dell'input dell'utente massimo tra tutte le sessioni) e "Average" (la media di tutte le sessioni).

Questa tabella mostra un esempio visivo di queste istanze. (È possibile ottenere le stesse informazioni in Perfmon passando al tipo di grafico Report.)

|Tipo di contatore|Nome istanza|Ritardo segnalata (ms)|
|---------------|-------------|-------------------|
|Ritardo dell'input dell'utente per processo|1:4232 <Calculator.exe>|  200|
|Ritardo dell'input dell'utente per processo|2:1000 <Calculator.exe>|  16|
|Ritardo dell'input dell'utente per processo|1:2000 <Calculator.exe>|  32|
|Ritardo dell'input dell'utente per sessione|1|    200|
|Ritardo dell'input dell'utente per sessione|2|    16|
|Ritardo dell'input dell'utente per sessione|Media|  108|
|Ritardo dell'input dell'utente per sessione|Max|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Contatori utilizzati in un sistema sovraccarico

Ora vediamo cosa verrà visualizzato nel report se le prestazioni di un'app risultano ridotte. Il grafico seguente mostra le letture agli utenti che lavorano in remoto in Microsoft Word. In questo caso, le prestazioni del server host sessione Desktop remoto si riducono nel tempo man mano che più utenti accedono.

![Desktop remoto: un grafico di esempio delle prestazioni per il server host sessione Desktop remoto che esegue Microsoft Word](./media/rds-user-input-perf-graph.png)

Di seguito viene illustrato come leggere le linee del grafico:

- La riga rosa indica il numero di sessioni di accesso al server.
- La linea rossa è l'utilizzo della CPU.
- La linea verde è il ritardo massimo dell'input dell'utente in tutte le sessioni.
- La linea blu (di colore nero in questo grafico) rappresenta il ritardo medio dell'input dell'utente tra tutte le sessioni.

Si noterà che esiste una correlazione tra i picchi di CPU e il ritardo dell'input dell'utente, ovvero quando l'utilizzo della CPU è maggiore , il ritardo dell'input dell'utente aumenta. Inoltre, più utenti vengono aggiunti al sistema, l'utilizzo della CPU si avvicina al 100% con picchi di ritardo dell'input dell'utente più frequenti. Questo contatore è molto utile nei casi in cui il server esaurisca le risorse, ma è possibile usarlo anche per tenere traccia dei ritardi dell'input dell'utente relativi a un'applicazione specifica.

## <a name="configuration-options"></a>Opzioni di configurazione

Un aspetto importante da ricordare quando si usa questo contatore delle prestazioni è che, per impostazione predefinita, segnala il ritardo dell'input dell'utente in un intervallo di 1.000 ms. Se si imposta la proprietà di intervallo campione del contatore delle prestazioni (come illustrato nello screenshot seguente) a qualsiasi elemento diverso, il valore restituito sarà errato.

![Desktop remoto: le proprietà per il monitoraggio delle prestazioni](./media/rds-user-input-perfmon-properties.png)

Per risolvere questo problema, è possibile impostare la chiave del Registro di sistema seguente in modo che corrisponda all'intervallo, in millisecondi, che si desidera utilizzare. Ad esempio, se si modifica Campione ogni x secondi in 5 secondi, è necessario impostare questa chiave su 5.000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Se si usa Windows 10, versione 1809 o versioni successive o Windows Server 2019 o versioni successive, non è necessario impostare LagCounterInterval per risolvere il problema del contatore delle prestazioni.

È stata anche aggiunta una coppia di chiavi che potrebbe risultare utile nella stessa chiave del Registro di sistema:

**LagCounterImageNameFirst** : impostare questa chiave su `DWORD 1` (valore predefinito 0 o la chiave non esiste). In questo modo i nomi dei contatori vengono modificati in "Image Name <SessionID:ProcessId>." Ad esempio, "explorer <1:7964>." Ciò è utile se si desidera eseguire l'ordinamento in base al nome dell'immagine.

**LagCounterShowUnknown** : impostare questa chiave su `DWORD 1` (valore predefinito 0 o la chiave non esiste). Vengono visualizzati tutti i processi in esecuzione come servizi o SYSTEM. Verranno visualizzati alcuni processi con la sessione impostata come "?."

Questo è il risultato si attivano entrambe le chiavi:

![Desktop remoto: il monitoraggio delle prestazioni con entrambe le chiavi attive](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Utilizzo dei nuovi contatori con strumenti non Microsoft

Gli strumenti di monitoraggio possono utilizzare questo contatore con l'[API Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Scaricare il software di Windows Server Insider

Gli utenti Insider registrati possono navigare direttamente nella [pagina di download di Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) per ottenere il software più recente di Insider.  Per informazioni su come registrarsi come Insider, vedere [Introduzione a Server](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Feedback

È possibile inviare commenti e suggerimenti per questa funzionalità tramite l'Hub di Feedback. Selezionare **App > Tutte le altre app** e includere "contatori delle prestazioni di Servizi Desktop remoto: monitoraggio delle prestazioni" nel titolo del post.

Per suggerimenti generali relativi alle funzionalità, visitare la [pagina UserVoice di Servizi Desktop remoto](https://aka.ms/uservoice-rds).
