---
title: mqbkup
description: Argomento di riferimento per il comando Mqbkup, che consente di eseguire il backup dei file di messaggi MSMQ e delle impostazioni del registro di sistema in un dispositivo di archiviazione e di ripristinare i messaggi e le impostazioni archiviati in precedenza.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c07dd5f912a70157052017fc17875c00eaedd3b
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354422"
---
# <a name="mqbkup"></a>mqbkup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di eseguire il backup dei file di messaggi MSMQ e delle impostazioni del registro di sistema in un dispositivo di archiviazione e di ripristinare i messaggi e le impostazioni archiviati in precedenza.

Il servizio MSMQ locale viene arrestato sia dal backup che dalle operazioni di ripristino. Se il servizio MSMQ è stato avviato in anticipo, l'utilità tenterà di riavviare il servizio MSMQ alla fine del backup o dell'operazione di ripristino. Se il servizio è già stato arrestato prima di eseguire l'utilità, non viene eseguito alcun tentativo di riavviare il servizio.

Prima di utilizzare l'utilità di backup/ripristino messaggi MSMQ, è necessario chiudere tutte le applicazioni locali che utilizzano MSMQ.

## <a name="syntax"></a>Sintassi

```
mqbkup {/b | /r} <folder path_to_storage_device>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| / b | Specifica l'operazione di backup. |
| /r | Specifica l'operazione di ripristino. |
| `<folder path_to_storage_device>` | Specifica il percorso in cui sono archiviati i file dei messaggi MSMQ e le impostazioni del registro di sistema. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Se una cartella specificata non esiste durante l'esecuzione dell'operazione di backup o ripristino, la cartella viene creata automaticamente dall'utilità.

- Se si sceglie di specificare una cartella esistente, deve essere vuota. Se si specifica una cartella non vuota, l'utilità eliminerà tutti i file e le sottocartelle in essa contenuti. In questo caso, verrà richiesto di concedere l'autorizzazione per eliminare i file e le sottocartelle esistenti. È possibile utilizzare il parametro **/y** per indicare che si accettano prima di tutto l'eliminazione di tutti i file e le sottocartelle esistenti nella cartella specificata.

- I percorsi delle cartelle utilizzate per archiviare i file di messaggi MSMQ vengono archiviati nel registro di sistema. Pertanto, l'utilità Ripristina i file dei messaggi MSMQ nelle cartelle specificate nel registro di sistema e non nelle cartelle di archiviazione utilizzate prima dell'operazione di ripristino.

### <a name="examples"></a>Esempio

Per eseguire il backup di tutti i file di messaggi MSMQ e le impostazioni del registro di sistema e per archiviarli nella cartella *msmqbkup* dell'unità C:, digitare:

```
mqbkup /b c:\msmqbkup
```

Per eliminare tutti i file e le sottocartelle esistenti nella cartella *oldbkup* nell'unità C: e quindi archiviare i file dei messaggi MSMQ e le impostazioni del registro di sistema nella cartella, digitare:

```
mqbkup /b /y c:\oldbkup
```

Per ripristinare i messaggi MSMQ e le impostazioni del registro di sistema, digitare:

```
mqbkup /r c:\msmqbkup
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Guida di riferimento a MSMQ PowerShell](https://docs.microsoft.com/powershell/module/msmq/?view=win10-ps)
