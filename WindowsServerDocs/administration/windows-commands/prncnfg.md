---
title: prncnfg
description: Informazioni su come configurare una stampante usando il comando prncfg.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6426b053a5c56918768f82cbd7631631abcf0a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859152"
---
# <a name="prncnfg"></a>prncnfg

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura o Visualizza le informazioni di configurazione di una stampante.

## <a name="syntax"></a>Sintassi
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|-g|Visualizza le informazioni di configurazione di una stampante.|
|-t|Consente di configurare una stampante.|
|-x|Rinomina una stampante.|
|-S \<ServerName\>|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene utilizzato il computer locale.|
|-P \<printerName\>|Specifica il nome della stampante che si desidera gestire. Obbligatorio.|
|-z \<NewprinterName\>|Specifica il nuovo nome della stampante. Richiede la **- x** e **-P** parametri.|
|-u \<nomeutente\> -w \<Password\>|Specifica un account con autorizzazioni sufficienti per connettersi al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è anche possibile concedere le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario essere connessi con un account con queste autorizzazioni per il comando lavorare.|
|-r \<PortName\>|Specifica la porta in cui è connesso alla stampante. Se si tratta di una porta seriale o parallelo, usare l'ID della porta (ad esempio, COM1 o LPT1). Se si tratta di una porta TCP/IP, usare il nome della porta che è stato specificato quando è stata aggiunta la porta.|
|-l \<percorso\>|Specifica il percorso della stampante, ad esempio "copia chat Room."|
|-h \<nomecondivisione\>|Specifica il nome di condivisione della stampante.|
|-m \<commento\>|Specifica la stringa di commento della stampante.|
|-f \<SeparatorFileName\>|Specifica un file che contiene il testo visualizzato nella pagina di separazione.|
|-y \<Datatype\>|Specifica i tipi di dati che può accettare la stampante.|
|-st \<starttime\>|Consente di configurare la stampante per la disponibilità limitata. Specifica l'ora del giorno che la stampante è disponibile. Se si invia un documento a una stampante quando non è disponibile, il documento viene memorizzato nello spooler finché la stampante non è disponibile. È necessario specificare tempo come un formato a 24 ore. Ad esempio, per specificare 11.00 P.M., digitare **2300**.|
|-ut \<Endtime\>|Consente di configurare la stampante per la disponibilità limitata. Specifica l'ora del giorno che della stampante non è più disponibile. Se si invia un documento a una stampante quando non è disponibile, il documento viene memorizzato nello spooler finché la stampante non è disponibile. È necessario specificare tempo come un formato a 24 ore. Ad esempio, per specificare 11.00 P.M., digitare **2300**.|
|-o \<priorità\>|Specifica una priorità che lo spooler utilizza per inviare i processi di stampa nella coda di stampa. Una coda di stampa con una priorità più alta riceve tutti i processi prima di qualsiasi altra coda con priorità più bassa.|
|-i \<DefaultPriority\>|Specifica la priorità predefinita assegnata a ogni processo di stampa.|
|{+&#124;-} condiviso|Specifica se la stampante è condivisa in rete.|
|{+&#124;-} diretto|Specifica se il documento deve essere inviato direttamente alla stampante senza in corso lo spooling.|
|{+&#124;-}published|Specifica se questa stampante deve essere pubblicata in active directory. Se si pubblica la stampante, altri utenti possono cercare, in base alla posizione e le funzionalità (ad esempio stampa a colori e associazione).|
|{+&#124;-} nascosti|Funzione riservato.|
|{+&#124;-} rawonly|Specifica se solo i processi di stampa dei dati non elaborati possono essere eseguito lo spooling in questa coda.|
|{+ &#124; -}queued|Specifica che la stampante non deve iniziare stampare fino a dopo l'ultima pagina del documento viene eseguito lo spooling. Il programma stampa non è disponibile finché non stampa il documento è terminata. Tuttavia, Usa questo parametro assicura che l'intero documento è disponibile per la stampante.|
|{+ &#124; -}keepprintedjobs|Specifica se lo spooler deve mantenere i documenti che sono stati stampati. L'abilitazione di questa opzione consente all'utente di inviare nuovamente un documento sulla stampante dalla coda di stampa anziché dal programma di stampa.|
|{+ &#124; -}workoffline|Specifica se un utente è in grado di inviare i processi di stampa alla coda di stampa, se il computer non è connesso alla rete.|
|{+ &#124; -}enabledevq|Specifica se i processi di stampa che non corrispondono alla configurazione della stampante (ad esempio, i file di spooling alle stampanti non PostScript PostScript) devono essere mantenuti nella coda piuttosto che da stampare.|
|{+ &#124; -} docompletefirst|Specifica se lo spooler deve inviare i processi di stampa con una priorità più bassa che hanno completato lo spooling prima di inviare i processi di stampa con una priorità più alta che non hanno completato lo spooling. Se questa opzione è abilitata e documenti non hanno completato lo spooling, lo spooler invierà documenti di grandi dimensioni prima di quelli più piccoli. È consigliabile abilitare questa opzione se si desidera ottimizzare l'efficienza della stampante al costo di priorità del processo. Se questa opzione è disabilitata, lo spooler invia sempre i processi con priorità superiore alle rispettive code prima di tutto.|
|{+ &#124; -}enablebidi|Specifica se la stampante invia le informazioni sullo stato allo spooler.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il **prncnfg** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo del file prncnfg, o cambiare le directory nella cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).

## <a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni di configurazione per la stampante denominata StampanteColori_2 con una coda di stampa ospitata dal computer remoto denominato ServerRU, digitare:
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Per configurare una stampante denominata StampanteColori_2 in modo che lo spooler nel computer remoto denominato ServerRU mantiene i processi di stampa dopo che sono state stampate, digitare:
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Per modificare il nome di una stampante nel computer remoto denominato HRServer da StampanteColori_2 per StampanteColori 3, tipo:
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[riferimenti ai comandi di stampa](print-command-reference.md)
