---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: a12647908811066293772ab1e9354a0d67874d88
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720052"
---
# <a name="fsutil"></a>Fsutil

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue le attività correlate ai file system FAT (file allocation table) e NTFS, ad esempio la gestione dei reparse point, la gestione di file sparse o lo smontaggio di un volume. Se viene utilizzato senza parametri, **fsutil** Visualizza un elenco di sottocomandi supportati. 

> [!NOTE] 
> Per utilizzare fsutil, è necessario effettuare l'accesso come amministratore o come membro del gruppo Administrators. Il fsutil comando è piuttosto potente e deve essere utilizzato solo da utenti avanzati che hanno una conoscenza approfondita dei sistemi operativi Windows.
>
>Prima di poter eseguire **fsutil**, è necessario abilitare il sottosistema Windows per Linux. Eseguire il comando seguente come amministratore in PowerShell per abilitare questa funzionalità facoltativa:
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Verrà richiesto di riavviare il computer dopo l'installazione. Dopo il riavvio del computer, sarà possibile eseguire **fsutil** come amministratore.

### <a name="parameters"></a>Parametri

|Sottocomando |Descrizione|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | Esegue una query o modifica le impostazioni per il comportamento del nome breve nel sistema, ad esempio, genera i nomi di file di lunghezza di 8,3 caratteri. Rimuove il nome breve di tutti i file contenuti in una directory. Analizza una directory e identifica le chiavi del Registro di sistema che potrebbero essere interessate se i nomi brevi sono stati rimossi dal file nella directory.|
|[Comportamento fsutil](fsutil-behavior.md) |Esegue una query o imposta il comportamento del volume.|
|[Fsutil dirty](fsutil-dirty.md)| Esegue una query se bit dirty del volume è impostato o imposta un bit del volume danneggiato. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.|
|[Fsutil (file)](fsutil-file.md)|Trova un file in base al nome utente (se sono abilitate le quote disco), esegue query sugli intervalli allocati per un file, imposta il nome breve di un file, imposta la lunghezza dei dati valida di un file, imposta zero dati per un file, crea un nuovo file di una dimensione specificata, trova un ID file se viene specificato il nome o trova un nome di collegamento file per un|
|[Fsutil fsinfo](fsutil-fsinfo.md)|Elenca tutte le unità ed esegue una query sul tipo di unità, informazioni sul volume, informazioni sul volume specifiche di NTFS o statistiche file system.|
|[Fsutil hardlink](fsutil-hardlink.md)|Elenca i collegamenti reali per un file o crea un collegamento reale (una voce di directory per un file). Ogni file può essere considerato disporre di almeno un collegamento fisso. Nei volumi NTFS ogni file può avere più collegamenti reali, quindi un singolo file può essere visualizzato in molte directory (o anche nella stessa directory con nomi diversi). Poiché tutti i collegamenti di riferimento nello stesso file, i programmi possono aprire i collegamenti e modificare il file. Un file viene eliminato dal file system solo dopo l'eliminazione di tutti i collegamenti a esso. Dopo aver creato un collegamento fisso, i programmi possono utilizzarlo come qualsiasi altro nome di file.|
|[ID fsutil](fsutil-objectid.md)|Gestisce gli identificatori di oggetto utilizzati dal sistema operativo Windows per tenere traccia degli oggetti quali file e directory.|
|[Fsutil quota](fsutil-quota.md)|Gestisce le quote disco nei volumi NTFS per fornire un controllo più preciso dell'archiviazione basata sulla rete. Le quote disco sono implementate in base al volume e consentono di implementare i limiti di archiviazione sia rigida che temporanea per ogni singolo utente.|
|[Fsutil Repair](fsutil-repair.md)|Esegue una query o imposta lo stato di correzione automatica del volume. Il file NTFS con riparazione automatica tenta di correggere i danneggiamenti del file system NTFS online senza richiedere l'esecuzione di **chkdsk. exe** . Include l'avvio della verifica su disco e l'attesa del completamento del ripristino.|
|[Fsutil reparsepoint](fsutil-reparsepoint.md)|Esegue una query o Elimina i reparse point (NTFS file system oggetti che dispongono di un attributo definibile contenente dati controllati dall'utente). I reparse point vengono usati per estendere le funzionalità nel sottosistema di input/output (I/O). Vengono utilizzati per i punti di giunzione directory e i punti di montaggio del volume. Vengono inoltre utilizzati dal driver di filtro di file system per contrassegnare determinati file come speciali per tale driver.|
|[Fsutil (risorsa)](fsutil-resource.md)|Crea una Gestione risorse transazionale secondaria, avvia o arresta una Gestione risorse transazionale, Visualizza le informazioni su un Gestione risorse transazionale o ne modifica il comportamento.|
|[Fsutil sparse](fsutil-sparse.md)|Gestisce i file sparse. Un file sparse è un file con una o più aree di dati non allocati. Un programma vedranno tali aree non allocate come contenente byte con valore zero, ma nessuno spazio su disco viene utilizzato per rappresentare questi zeri. Vengono allocati tutti i dati significativi o diversi da zero, mentre tutti i dati non significativi (stringhe di grandi dimensioni di dati costituiti da zeri) non vengono allocati. Quando viene letto un file sparse, i dati allocati vengono restituiti come archiviati e i dati non allocati vengono restituiti come zeri (per impostazione predefinita, in base alla specifica del requisito di sicurezza C2). Supporto file sparse consente di deallocare da ovunque nel file di dati.|
|[Suddivisione in livelli fsutil](fsutil-tiering.md)|Consente la gestione delle funzioni del livello di archiviazione, ad esempio l'impostazione e la disabilitazione dei flag e l'elenco di livelli.|
|[Fsutil transaction](fsutil-transaction.md)|Esegue il commit di una transazione specificata, esegue il rollback di una transazione specificata o Visualizza informazioni sulla transazione.|
|[Fsutil USN](fsutil-usn.md)|Gestisce il Journal delle modifiche USN (Update Sequence Number), che fornisce un log permanente di tutte le modifiche apportate ai file nel volume.|
|[Volume fsutil](fsutil-volume.md)|Gestisce un volume. Smonta un volume, le query per verificare la quantità di spazio libero disponibile su disco, o trova un file che utilizza un cluster specificato.|
|[Fsutil Wim](fsutil-wim.md)|Fornisce funzioni per individuare e gestire i file supportati da WIM.|

## <a name="see-also"></a>Vedere anche
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)