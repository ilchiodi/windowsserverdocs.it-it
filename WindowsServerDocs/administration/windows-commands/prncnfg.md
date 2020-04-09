---
title: prncnfg
description: Informazioni su come configurare una stampante usando il comando prncfg.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 3db99c06232e4ed6b3ad5df4ee189d38bffb14c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837324"
---
# <a name="prncnfg"></a>prncnfg

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura o Visualizza le informazioni di configurazione relative a una stampante.

## <a name="syntax"></a>Sintassi
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|-g|Visualizza le informazioni di configurazione relative a una stampante.|
|-t|Configura una stampante.|
|-x|Rinomina una stampante.|
|-S \<nomeserver\>|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.|
|-P \<PrinterName\>|Specifica il nome della stampante che si desidera gestire. Obbligatoria.|
|-z \<NewprinterName\>|Specifica il nuovo nome della stampante. Richiede i parametri **-x** e **-P** .|
|-u \<nome utente\>-w \<password\>|Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando.|
|-r \<Portaname\>|Specifica la porta a cui è connessa la stampante. Se si tratta di una porta parallela o seriale, usare l'ID della porta (ad esempio, LPT1 o COM1). Se si tratta di una porta TCP/IP, utilizzare il nome della porta specificato quando la porta è stata aggiunta.|
|-l \<percorso\>|Specifica il percorso della stampante, ad esempio "Copy Room".|
|-h \<ShareName\>|Specifica il nome della condivisione della stampante.|
|-m \<commento\>|Specifica la stringa di commento della stampante.|
|-f \<SeparatorFileName\>|Specifica un file che contiene il testo visualizzato nella pagina separatore.|
|-y \<DataType\>|Specifica i tipi di dati che la stampante può accettare.|
|-St \<StartTime\>|Configura la stampante per la disponibilità limitata. Specifica l'ora del giorno in cui è disponibile la stampante. Se si invia un documento a una stampante quando non è disponibile, il documento viene mantenuto (con spooling) finché la stampante non diventa disponibile. È necessario specificare l'ora come orario a 24 ore. Ad esempio, per specificare 11:00, digitare **2300**.|
|-UT \<EndTime\>|Configura la stampante per la disponibilità limitata. Specifica l'ora del giorno in cui la stampante non è più disponibile. Se si invia un documento a una stampante quando non è disponibile, il documento viene mantenuto (con spooling) finché la stampante non diventa disponibile. È necessario specificare l'ora come orario a 24 ore. Ad esempio, per specificare 11:00, digitare **2300**.|
|-o \<priorità\>|Specifica una priorità utilizzata dallo spooler per indirizzare i processi di stampa nella coda di stampa. Una coda di stampa con una priorità più alta riceve tutti i processi prima di qualsiasi coda con una priorità più bassa.|
|-i \<DefaultPriority\>|Specifica la priorità predefinita assegnata a ogni processo di stampa.|
|{+&#124;-} condiviso|Specifica se la stampante è condivisa in rete.|
|{+&#124;-} diretta|Specifica se il documento deve essere inviato direttamente alla stampante senza che venga eseguito lo spooling.|
|{+&#124;-} pubblicato|Specifica se la stampante deve essere pubblicata in Active Directory. Se si pubblica la stampante, altri utenti possono cercarla in base alla posizione e alle funzionalità, ad esempio la stampa a colori e la graffatura.|
|{+&#124;-} nascosto|Funzione riservata.|
|{+&#124;-} RawOnly|Specifica se i processi di stampa dei dati non elaborati possono essere sottoposti a spooling in questa coda.|
|{+ &#124; -} in coda|Specifica che la stampante non deve iniziare a stampare fino a quando non viene eseguito lo spooling dell'ultima pagina del documento. Il programma di stampa non è disponibile fino a quando non viene completata la stampa del documento. Tuttavia, l'utilizzo di questo parametro garantisce che l'intero documento sia disponibile per la stampante.|
|{+ &#124; -} KeepPrintedJobs|Specifica se lo spooler deve conservare i documenti dopo la stampa. L'abilitazione di questa opzione consente a un utente di inviare nuovamente un documento alla stampante dalla coda di stampa invece che dal programma di stampa.|
|{+ &#124; -} WorkOffline|Specifica se un utente è in grado di inviare processi di stampa alla coda di stampa se il computer non è connesso alla rete.|
|{+ &#124; -} EnableDevQ|Specifica se i processi di stampa che non corrispondono alla configurazione della stampante, ad esempio i file di PostScript con spooling in stampanti non di post-script, devono essere mantenuti nella coda anziché essere stampati.|
|{+ &#124; -} DoCompleteFirst|Specifica se lo spooler deve inviare processi di stampa con una priorità più bassa che ha completato lo spooling prima di inviare processi di stampa con una priorità più alta che non hanno completato lo spooling. Se questa opzione è abilitata e nessun documento ha completato lo spooling, lo spooler invierà documenti più grandi prima di quelli più piccoli. È consigliabile abilitare questa opzione se si desidera ottimizzare l'efficienza della stampante a scapito della priorità del processo. Se questa opzione è disabilitata, lo spooler invia sempre primi processi con priorità più alta alle rispettive code.|
|{+ &#124; -} EnableBIDI|Specifica se la stampante invia informazioni sullo stato allo spooler.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il comando **prncnfg** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts\\<language>. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file prncnfg o passare alla cartella appropriata. Ad esempio,
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `"computer Name"`.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni di configurazione per la stampante denominata colorprinter_2 con una coda di stampa ospitata dal computer remoto denominato ServerRU, digitare:
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Per configurare una stampante denominata colorprinter_2 in modo che lo spooler nel computer remoto denominato ServerRU mantenga i processi di stampa dopo la stampa, digitare:
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Per modificare il nome di una stampante nel computer remoto denominato ServerRU da colorprinter_2 a colorprinter 3, digitare:
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[riferimento al comando stampa](print-command-reference.md)
