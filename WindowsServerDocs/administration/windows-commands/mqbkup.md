---
title: mqbkup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a809bfc788ba25afab280e80e5c6f0dc9c70dc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842902"
---
# <a name="mqbkup"></a>mqbkup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il backup del Registro di sistema e file di messaggi MSMQ impostazioni in un dispositivo di archiviazione e consente di ripristinare le impostazioni e i messaggi archiviati in precedenza.   
Il backup e l'operazione di ripristino interrompe il servizio MSMQ locale. Se è stato avviato il servizio MSMQ in anticipo, l'utilità tenterà di riavviare il servizio MSMQ alla fine del backup o l'operazione di ripristino. Se il servizio era già arrestato prima di eseguire l'utilità, viene eseguito alcun tentativo di riavviare il servizio.  
Prima di usare l'utilità di Backup/ripristino messaggio di MSMQ è necessario chiudere tutte le applicazioni locali che usano MSMQ.  
## <a name="syntax"></a>Sintassi  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/ b|Specifica l'operazione di backup|  
|/r|Specifica l'operazione di ripristino|  
|< cartella path_to_storage\_dispositivo >|Specifica il percorso in cui sono archiviate i file dei messaggi MSMQ e le impostazioni del Registro di sistema|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="BKMK_Examples"></a>Esempi  
Eseguire il backup di tutti i file dei messaggi MSMQ e le impostazioni del Registro di sistema e archiviarli nel *Msmqbkup* nell'unità c: cartella.  
```  
mqbkup /b c:\msmqbkup  
```  
Se la cartella specificata non esiste, l'utilità creerà automaticamente uno. Se si sceglie di specificare una cartella esistente, questa cartella deve essere vuota. Se si specifica una cartella non è vuoto, l'utilità eliminerà tutti i file e sottocartella contenuta al suo interno. In questo caso, verrà richiesto di concedere l'autorizzazione per eliminare le sottocartelle e i file esistenti. È possibile usare la **/y** parametro per indicare che l'utente accetti in anticipo l'eliminazione di tutti i file esistenti e le sottocartelle nella cartella specificata.  
Per eliminare tutti i file e sottocartelle nel *Oldbkup* cartella l'unità c: e archivio file di messaggi MSMQ e le impostazioni del Registro di sistema in questa cartella.  
```  
mqbkup /b /y c:\oldbkup  
```  
Per ripristinare i messaggi MSMQ e le impostazioni del Registro di sistema:  
```  
mqbkup /r c:\msmqbkup  
```  
I percorsi delle cartelle utilizzate per archiviare i file di messaggi MSMQ vengono archiviati nel Registro di sistema. Di conseguenza, l'utilità ripristinerà i file di messaggi MSMQ per le cartelle specificate nel Registro di sistema e non per le cartelle di archiviazione usate prima dell'operazione di ripristino. Se le cartelle specificate nel Registro di sistema non esistono, l'operazione di ripristino verrà automaticamente crearli. Se le directory di cartelle sono presenti e non sono vuote, l'utilità richiederà l'autorizzazione per eliminare il contenuto corrente di queste cartelle.  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
