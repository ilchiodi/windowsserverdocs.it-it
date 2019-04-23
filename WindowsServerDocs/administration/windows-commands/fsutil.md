---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: df8d25b01b67010734deb8dd7e42f3233e6011fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866812"
---
# <a name="fsutil"></a>Fsutil

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue attività correlate al file allocation table (FAT) e file system NTFS, ad esempio la gestione di analisi fa riferimento, la gestione dei file sparse o lo smontaggio di un volume. Se utilizzata senza parametri, **Fsutil** Visualizza un elenco di sottocomandi supportati. 

> [!Note] 
> È necessario essere connessi come amministratore o un membro del gruppo Administrators usare Fsutil. Il comando Fsutil è uno strumento potente e deve essere utilizzato solo da utenti esperti che hanno una conoscenza approfondita dei sistemi operativi Windows.
>
>È necessario abilitare il sottosistema Windows per Linux prima di poter eseguire **Fsutil**. Eseguire il comando seguente come amministratore in PowerShell per abilitare questa funzionalità facoltativa:
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Verrà richiesto di riavviare il computer dopo l'installazione. Dopo aver riavviato il computer, sarà possibile eseguire **Fsutil** come amministratore.

## <a name="parameters"></a>Parametri

|Sottocomando |Descrizione|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | Esegue una query o modifica le impostazioni per il comportamento di nome breve nel sistema, ad esempio, genera nomi di file di lunghezza dei caratteri in formato 8.3. Rimuove il nome breve di tutti i file contenuti in una directory. Analizza una directory e identifica le chiavi del Registro di sistema che potrebbero essere interessate se i nomi brevi sono stati rimossi dal file nella directory.|
|[Comportamento fsutil](fsutil-behavior.md) |Esegue una query o imposta il comportamento di volume.|
|[fsutil dirty](fsutil-dirty.md)| Esegue una query se bit dirty del volume è impostato o imposta un bit del volume danneggiato. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.|
|[Fsutil file](fsutil-file.md)|Trova un file in base al nome utente (se sono abilitate le quote disco), gli intervalli allocati per un file di una query, imposta nome breve del file, Imposta lunghezza dei dati validi di un file, imposta su zero i dati per un file, crea un nuovo file di dimensioni specificate, consente di trovare un ID di file assegnato il nome , o trova un nome di collegamento di file per un ID di file specificato.|
|[Fsinfo fsutil](fsutil-fsinfo.md)|Elenca tutte le unità e una query sul tipo di unità, informazioni sul volume, informazioni sul volume NTFS specifiche o le statistiche di sistema di file.|
|[Fsutil hardlink](fsutil-hardlink.md)|Elenca i collegamenti reali per un file oppure crea un collegamento fisso (una voce di directory per un file). Ogni file può essere considerato disporre di almeno un collegamento fisso. Nei volumi NTFS, i file possono avere più collegamenti fissi, pertanto un singolo file possono essere visualizzati in diverse directory (o persino nella stessa directory, con nomi diversi). Poiché tutti i collegamenti di riferimento nello stesso file, i programmi possono aprire i collegamenti e modificare il file. Un file viene eliminato dal file system solo dopo l'eliminazione di tutti i collegamenti a esso. Dopo aver creato un collegamento fisso, i programmi possono utilizzarlo come qualsiasi altro nome di file.|
|[Objectid fsutil](fsutil-objectid.md)|Gestisce gli identificatori di oggetto utilizzati dal sistema operativo Windows per tenere traccia degli oggetti quali file e directory.|
|[Quota di fsutil](fsutil-quota.md)|Consente di gestire le quote disco nei volumi NTFS per fornire un controllo più preciso dell'archiviazione basata su rete. Le quote disco sono implementate in base al volume e abilitano entrambi i limiti rigidi e soft-archiviazione essere implementata in una base per utente.|
|[Ripristina fsutil](fsutil-repair.md)|Esegue una query o imposta lo stato del volume di riparazione automatica. Riparazione automatica di NTFS tenta di correggere i danneggiamenti del file system NTFS, online senza richiedere **Chkdsk.exe** da eseguire. Include avvio verifica su disco e in attesa del completamento di ripristino.|
|[Reparsepoint fsutil](fsutil-reparsepoint.md)|Le query o le eliminazioni reparse point (NTFS oggetti del file system che dispongono di un attributo definibile contenente dati controllata dall'utente). Analisi punti vengono utilizzati per estendere le funzionalità del sottosistema di input/output (i/o). Vengono utilizzati per i punti di giunzione directory e i punti di montaggio del volume. Vengono inoltre utilizzati dal driver di filtro di file system per contrassegnare determinati file come speciali per tale driver.|
|[Fsutil risorse](fsutil-resource.md)|Crea un gestore di risorse transazionale secondario, avvia o arresta un gestore di risorse transazionale, vengono visualizzate informazioni su un gestore di risorse transazionale o modifica il comportamento.|
|[Fsutil tipo sparse](fsutil-sparse.md)|Gestisce i file sparse. Un file sparse è un file con una o più aree di dati non allocati. Un programma vedranno tali aree non allocate come contenente byte con valore zero, ma nessuno spazio su disco viene utilizzato per rappresentare questi zeri. Tutti i dati significativi o diverso da zero è allocato, mentre tutti i dati non significativi (stringhe di grandi dimensioni dei dati è costituite da zeri) non è allocata. Quando viene letto un file sparse, dati allocati vengono restituiti come archiviati e dati non allocati vengono restituiti come zeri (per impostazione predefinita conforme alla specifica di requisiti di sicurezza C2). Supporto file sparse consente di deallocare da ovunque nel file di dati.|
|[La suddivisione in livelli di fsutil](fsutil-tiering.md)|Abilita la gestione di funzioni di livello di archiviazione, ad esempio l'impostazione e la disabilitazione di flag e visualizzazione di un elenco dei livelli.|
|[Fsutil transazione](fsutil-transaction.md)|Esegue il commit di una transazione specificata, il rollback di una transazione specificata o la visualizzazione di informazioni sulla transazione.|
|[Fsutil usn](fsutil-usn.md)|Gestisce l'aggiornamento sequenza USN (number) journal delle modifiche, che fornisce un registro permanente di tutte le modifiche apportate ai file nel volume.|
|[Volume di fsutil](fsutil-volume.md)|Gestisce un volume. Smonta un volume, le query per verificare la quantità di spazio libero disponibile su disco, o trova un file che utilizza un cluster specificato.|
|[File wim di fsutil](fsutil-wim.md)|Fornisce funzioni per individuare e gestire i file supportato da WIM.|

## <a name="see-also"></a>Vedere anche
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)