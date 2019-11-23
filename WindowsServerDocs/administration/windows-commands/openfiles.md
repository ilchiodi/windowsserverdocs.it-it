---
title: openfiles
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38b1d27b86551c6d4cd9e6b1ad87bfc0e8dd221d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372496"
---
# <a name="openfiles"></a>openfiles



Consente agli amministratori di eseguire una query, visualizzare o disconnettere i file e directory in cui sono state aperte in un sistema. Inoltre, Abilita o disabilita il flag globale Gestisci elenco di oggetti di sistema.

In questo argomento include informazioni sui comandi seguenti:
-   [/Disconnect openfiles](#BKMK_disconnect)
-   [/query openfiles](#BKMK_query)
-   [/local openfiles](#BKMK_local)

## <a name="BKMK_disconnect"></a>/Disconnect openfiles

Consente agli amministratori di disconnettere i file e cartelle che sono state aperte in modalità remota tramite una cartella condivisa.

### <a name="syntax"></a>Sintassi

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parametri

|            Parametro             |                                                                                                                                 Descrizione                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<sistema >           | Specifica il sistema remoto a cui connettersi (per nome o indirizzo IP). Non utilizzare le barre rovesciate. Se non si utilizza il **/s** opzione, il comando viene eseguito nel computer locale per impostazione predefinita. Questo parametro si applica a tutti i file e cartelle in cui sono specificate nel comando. |
|    /u [\<dominio >\]<UserName>     |                                                          Esegue il comando utilizzando le autorizzazioni dell'account utente specificato. Se non si utilizza il **/u** opzione, le autorizzazioni sono utilizzate per impostazione predefinita di sistema.                                                           |
|         /p [\<> password]         |                                               Specifica la password dell'account utente specificato nella **/u** (opzione). Se non si utilizza il **/p** opzione, un prompt della password viene visualizzata quando viene eseguito il comando.                                                |
|        /ID \<OpenFileID >         |                                       Disconnette i file aperti per l'ID del file specificato. Con questo parametro è **&#42;** possibile usare il carattere jolly ().</br>Nota: è possibile usare il comando **openfiles/query** per trovare l'ID file.                                       |
|         /a \<utenteconnesso >         |                                                Disconnette tutti i file aperti associati con il nome utente specificato nella *utenteconnesso* parametro. Con questo parametro è **&#42;** possibile usare il carattere jolly ().                                                 |
| /o {Read \| scrittura \| lettura/scrittura} |                                               Disconnette tutti i file aperti con il valore della modalità di apertura specificato. I valori validi sono di lettura, scrittura o lettura/scrittura. Con questo parametro è **&#42;** possibile usare il carattere jolly ().                                                |
|         /op \<> OpenFile          |                                                           Disconnette tutte le connessioni di aprire file creati da un nome di file specifico. Con questo parametro è **&#42;** possibile usare il carattere jolly ().                                                           |
|                /?                |                                                                                                                     Visualizza la guida al prompt dei comandi.                                                                                                                     |

### <a name="examples"></a>Esempi

Per disconnettere tutti i file aperti con il file 26843578 ID, digitare:
```
openfiles /disconnect /id 26843578
```
Per disconnettere tutti i file e directory accessibili all'utente "hiropln", digitare:
```
openfiles /disconnect /a hiropln
```
Per disconnettere tutti i file e directory con la modalità di lettura/scrittura, digitare:
```
openfiles /disconnect /o read/write
```
Per disconnettere la directory con il nome file aperto "C:\TestShare\", indipendentemente dall'utente che vi accede, digitare:
```
openfiles /disconnect /a * /op "c:\testshare\"
```
Per disconnettere tutti i file aperti nel computer remoto "Srvprinc" accessibili dall'utente "hiropln", indipendentemente dal relativo ID, digitare:
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>/query openfiles

Esegue una query e visualizza tutti i file aperti.

### <a name="syntax"></a>Sintassi

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Parametri

|          Parametro           |                                                                                                                                 Descrizione                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<sistema >         | Specifica il sistema remoto a cui connettersi (per nome o indirizzo IP). Non utilizzare le barre rovesciate. Se non si utilizza il **/s** opzione, il comando viene eseguito nel computer locale per impostazione predefinita. Questo parametro si applica a tutti i file e cartelle in cui sono specificate nel comando. |
|  /u [\<dominio >\]<UserName>   |                                                          Esegue il comando utilizzando le autorizzazioni dell'account utente specificato. Se non si utilizza il **/u** opzione, le autorizzazioni sono utilizzate per impostazione predefinita di sistema.                                                           |
|       /p [\<> password]       |                                               Specifica la password dell'account utente specificato nella **/u** (opzione). Se non si utilizza il **/p** opzione, un prompt della password viene visualizzata quando viene eseguito il comando.                                                |
| [/fo {TABLE \| LIST \| CSV}] |                             Visualizza l'output nel formato specificato. I valori validi per *formato* sono:</br>TABELLA: Visualizza l'output in una tabella.</br>ELENCO: Visualizza l'output in un elenco.</br>CSV: Visualizza l'output in formato valori separati da virgola.                              |
|             /NH              |                                                                                Omette le intestazioni di colonna nell'output. Valido solo quando il **/fo** parametro è impostato su **TABELLA** o **CSV**.                                                                                 |
|              /v              |                                                                                                       Specifica le informazioni dettagliate visualizzate nell'output.                                                                                                        |
|              /?              |                                                                                                                     Visualizza la guida al prompt dei comandi.                                                                                                                     |

### <a name="examples"></a>Esempi

Per eseguire una query e visualizzare tutti i file aperti, digitare:
```
openfiles /query
```
Per eseguire query e visualizzare tutti i file aperti in formato di tabella senza intestazioni, digitare:
```
openfiles /query /fo table /nh
```
Per eseguire una query e visualizzare tutti i file aperti in formato di elenco con informazioni dettagliate, digitare:
```
openfiles /query /fo list /v
```
Per eseguire query e visualizzare tutti i file aperti nel sistema remoto "Srvprinc" utilizzando le credenziali per l'utente "hiropln" in "Domprinc", digitare:
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> In questo esempio, la password è specificata nella riga di comando. Per evitare la visualizzazione della password, omettere il **/p** (opzione). Verrà richiesto per la password, che non viene visualizzata sullo schermo.

## <a name="BKMK_local"></a>/local openfiles

Abilita o disabilita il flag globale Gestisci elenco di oggetti di sistema. Se utilizzata senza parametri, **openfiles /local** Visualizza lo stato corrente del flag globale Maintain Objects List.

### <a name="syntax"></a>Sintassi

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[on \| off]|Abilita o disabilita il flag globale Maintain Objects List, che tiene traccia di handle di file locali di sistema.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Osservazioni

-   Abilitare il flag globale Maintain Objects List può rallentare il sistema.
-   Le modifiche apportate mediante la **su** o **off** opzione diventano effettive solo dopo il riavvio del sistema.

### <a name="examples"></a>Esempi

Per controllare lo stato corrente del flag globale Maintain Objects List, digitare:
```
openfiles /local
```
Per impostazione predefinita, il flag globale Maintain Objects List è disabilitato e viene visualizzato il seguente output:
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
Per abilitare il flag globale Maintain Objects List, digitare:
```
openfiles /local on
```
Quando il flag globale è abilitato, viene visualizzato il messaggio seguente:
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
Per disabilitare il flag globale Maintain Objects List, digitare:
```
openfiles /local off
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)