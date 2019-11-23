---
title: mqbkup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66783e0bbfe5c82971e14fd05e913d485485dc6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373506"
---
# <a name="mqbkup"></a>mqbkup

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di eseguire il backup dei file di messaggi MSMQ e delle impostazioni del registro di sistema in un dispositivo di archiviazione e di ripristinare i messaggi e le impostazioni archiviati in precedenza.   
Il servizio MSMQ locale viene arrestato sia dal backup che dall'operazione di ripristino. Se il servizio MSMQ è stato avviato in anticipo, l'utilità tenterà di riavviare il servizio MSMQ alla fine del backup o dell'operazione di ripristino. Se il servizio è già stato arrestato prima di eseguire l'utilità, non viene eseguito alcun tentativo di riavviare il servizio.  
Prima di utilizzare l'utilità di backup/ripristino messaggi MSMQ, è necessario chiudere tutte le applicazioni locali che utilizzano MSMQ.  
## <a name="syntax"></a>Sintassi  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/ b|Specifica l'operazione di backup|  
|/r|Specifica l'operazione di ripristino|  
|< cartella path_to_storage\_dispositivo >|Specifica il percorso in cui sono archiviati i file dei messaggi MSMQ e le impostazioni del registro di sistema|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="BKMK_Examples"></a>Esempi  
Per eseguire il backup di tutti i file di messaggi MSMQ e le impostazioni del registro di sistema e archiviarli nella cartella *Msmqbkup* nell'unità C:.  
```  
mqbkup /b c:\msmqbkup  
```  
Se la cartella specificata non esiste, l'utilità ne creerà automaticamente una. Se si sceglie di specificare una cartella esistente, questa cartella deve essere vuota. Se si specifica una cartella non vuota, l'utilità eliminerà tutti i file e le sottocartelle in essa contenuti. In tal caso, verrà richiesto di concedere l'autorizzazione per eliminare i file e le sottocartelle esistenti. È possibile utilizzare il parametro **/y** per indicare che si accettano prima di tutto l'eliminazione di tutti i file e le sottocartelle esistenti nella cartella specificata.  
Per eliminare tutti i file e le sottocartelle nella cartella *Oldbkup* nell'unità C: e archiviare i file dei messaggi MSMQ e le impostazioni del registro di sistema in questa cartella.  
```  
mqbkup /b /y c:\oldbkup  
```  
Per ripristinare i messaggi MSMQ e le impostazioni del registro di sistema:  
```  
mqbkup /r c:\msmqbkup  
```  
I percorsi delle cartelle utilizzate per archiviare i file di messaggi MSMQ vengono archiviati nel registro di sistema. In questo modo, l'utilità ripristinerà i file di messaggi MSMQ alle cartelle specificate nel registro di sistema e non alle cartelle di archiviazione utilizzate prima dell'operazione di ripristino. Se le cartelle specificate nel registro di sistema non esistono, l'operazione di ripristino li creerà automaticamente. Se le directory delle cartelle esistono e non sono vuote, l'utilità richiede l'autorizzazione per eliminare il contenuto corrente di queste cartelle.  
## <a name="additional-references"></a>riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
