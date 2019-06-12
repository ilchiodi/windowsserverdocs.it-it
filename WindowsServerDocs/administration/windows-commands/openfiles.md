---
title: openfiles
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0bec8cf64a3c7f261c792a07da603cba4366e1a7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436431"
---
# <a name="openfiles"></a>openfiles



Consente agli amministratori di eseguire una query, visualizzare o disconnettere i file e directory in cui sono state aperte in un sistema. Inoltre, Abilita o disabilita il flag globale Gestisci elenco di oggetti di sistema.

In questo argomento include informazioni sui comandi seguenti:
-   [OPENFILES /Disconnect](#BKMK_disconnect)
-   [/query openfiles](#BKMK_query)
-   [/local openfiles](#BKMK_local)

## <a name="BKMK_disconnect"></a>OPENFILES /Disconnect

Consente agli amministratori di disconnettere i file e cartelle che sono state aperte in modalità remota tramite una cartella condivisa.

### <a name="syntax"></a>Sintassi

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parametri

|            Parametro             |                                                                                                                                 Descrizione                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<system >           | Specifica il sistema remoto a cui connettersi (per nome o indirizzo IP). Non utilizzare le barre rovesciate. Se non si utilizza il **/s** opzione, il comando viene eseguito nel computer locale per impostazione predefinita. Questo parametro si applica a tutti i file e cartelle in cui sono specificate nel comando. |
|    /u [\<Domain>\]<UserName>     |                                                          Esegue il comando utilizzando le autorizzazioni dell'account utente specificato. Se non si utilizza il **/u** opzione, le autorizzazioni sono utilizzate per impostazione predefinita di sistema.                                                           |
|         /p [\<Password>]         |                                               Specifica la password dell'account utente specificato nella **/u** (opzione). Se non si utilizza il **/p** opzione, un prompt della password viene visualizzata quando viene eseguito il comando.                                                |
|        /ID \<OpenFileID >         |                                       Disconnette i file aperti per l'ID del file specificato. Il carattere jolly ( **&#42;** ) può essere utilizzato con questo parametro.</br>Nota: È possibile usare la **openfiles /query** comando per trovare l'ID file.                                       |
|         /a \<AccessedBy>         |                                                Disconnette tutti i file aperti associati con il nome utente specificato nella *utenteconnesso* parametro. Il carattere jolly ( **&#42;** ) può essere utilizzato con questo parametro.                                                 |
| /o {lettura \| scrivere \| lettura/scrittura} |                                               Disconnette tutti i file aperti con il valore di aprire la modalità specificata. I valori validi sono di lettura, scrittura o lettura/scrittura. Il carattere jolly ( **&#42;** ) può essere utilizzato con questo parametro.                                                |
|         /Op. \<OpenFile >          |                                                           Disconnette tutte le connessioni di aprire file creati da un nome di file specifico. Il carattere jolly ( **&#42;** ) può essere utilizzato con questo parametro.                                                           |
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
Per disconnettere la directory con il nome di File aperti "C:\TestShare\", indipendentemente dal chi accede a tale colonna, tipo:
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
|         /s \<system >         | Specifica il sistema remoto a cui connettersi (per nome o indirizzo IP). Non utilizzare le barre rovesciate. Se non si utilizza il **/s** opzione, il comando viene eseguito nel computer locale per impostazione predefinita. Questo parametro si applica a tutti i file e cartelle in cui sono specificate nel comando. |
|  /u [\<Domain>\]<UserName>   |                                                          Esegue il comando utilizzando le autorizzazioni dell'account utente specificato. Se non si utilizza il **/u** opzione, le autorizzazioni sono utilizzate per impostazione predefinita di sistema.                                                           |
|       /p [\<Password>]       |                                               Specifica la password dell'account utente specificato nella **/u** (opzione). Se non si utilizza il **/p** opzione, un prompt della password viene visualizzata quando viene eseguito il comando.                                                |
| [/fo {tabella \| elenco \| CSV}] |                             Visualizza l'output nel formato specificato. I valori validi per *formato* sono:</br>TAVOLO:  Visualizza l'output in una tabella.</br>ELENCO: Visualizza l'output in un elenco.</br>CSV: Visualizza l'output in formato valori separati da virgole.                              |
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
|[in \| off]|Abilita o disabilita il flag globale Maintain Objects List, che tiene traccia di handle di file locali di sistema.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="remarks"></a>Note

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