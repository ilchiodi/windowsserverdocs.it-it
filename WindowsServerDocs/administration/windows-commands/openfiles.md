---
title: openfiles
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 050e3f31435949ecce2a9a06a70d86eacd745687
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723395"
---
# <a name="openfiles"></a>openfiles



Consente agli amministratori di eseguire una query, visualizzare o disconnettere i file e directory in cui sono state aperte in un sistema. Inoltre, Abilita o disabilita il flag globale Gestisci elenco di oggetti di sistema.

In questo argomento include informazioni sui comandi seguenti:
-   [OPENFILES /Disconnect](#BKMK_disconnect)
-   [/query openfiles](#BKMK_query)
-   [/local openfiles](#BKMK_local)

## <a name="openfiles-disconnect"></a><a name=BKMK_disconnect></a>OPENFILES /Disconnect

Consente agli amministratori di disconnettere i file e cartelle che sono state aperte in modalità remota tramite una cartella condivisa.

### <a name="syntax"></a>Sintassi

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

#### <a name="parameters"></a>Parametri

|            Parametro             |                                                                                                                                 Descrizione                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<> di sistema           | Specifica il sistema remoto a cui connettersi (per nome o indirizzo IP). Non utilizzare le barre rovesciate. Se non si utilizza il **/s** opzione, il comando viene eseguito nel computer locale per impostazione predefinita. Questo parametro si applica a tutti i file e cartelle in cui sono specificate nel comando. |
|    /u [\<dominio>\]<UserName>     |                                                          Esegue il comando utilizzando le autorizzazioni dell'account utente specificato. Se non si utilizza il **/u** opzione, le autorizzazioni sono utilizzate per impostazione predefinita di sistema.                                                           |
|         /p [\<password>]         |                                               Specifica la password dell'account utente specificato nella **/u** (opzione). Se non si utilizza il **/p** opzione, un prompt della password viene visualizzata quando viene eseguito il comando.                                                |
|        /ID \<OpenFileID>         |                                       Disconnette i file aperti per l'ID del file specificato. Con questo parametro è possibile usare il carattere jolly (**&#42;**).</br>Nota: è possibile utilizzare il **openfiles /query** comando per individuare l'ID del file.                                       |
|         /a \<utenteconnesso>         |                                                Disconnette tutti i file aperti associati con il nome utente specificato nella *utenteconnesso* parametro. Con questo parametro è possibile usare il carattere jolly (**&#42;**).                                                 |
| /o { \| lettura/ \| scrittura lettura/scrittura} |                                               Disconnette tutti i file aperti con il valore della modalità di apertura specificato. I valori validi sono di lettura, scrittura o lettura/scrittura. Con questo parametro è possibile usare il carattere jolly (**&#42;**).                                                |
|         > \<OpenFile/op          |                                                           Disconnette tutte le connessioni di aprire file creati da un nome di file specifico. Con questo parametro è possibile usare il carattere jolly (**&#42;**).                                                           |
|                /?                |                                                                                                                     Visualizza la guida al prompt dei comandi.                                                                                                                     |

### <a name="examples"></a>Esempi

Per disconnettere tutti i file aperti con il file 26843578 ID, digitare:
```
openfiles /disconnect /id 26843578
```
Per disconnettere tutti i file e le directory aperti accessibili dall'utente hiropln, digitare:
```
openfiles /disconnect /a hiropln
```
Per disconnettere tutti i file e directory con la modalità di lettura/scrittura, digitare:
```
openfiles /disconnect /o read/write
```
Per disconnettere la directory con il nome file aperto\, C:\TestShare indipendentemente dall'utente che vi accede, digitare:
```
openfiles /disconnect /a * /op c:\testshare\
```
Per disconnettere tutti i file aperti nel computer remoto SRVPRINC a cui si accede dall'utente hiropln, indipendentemente dal relativo ID, digitare:
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="openfiles-query"></a><a name=BKMK_query></a>/query openfiles

Esegue una query e visualizza tutti i file aperti.

### <a name="syntax"></a>Sintassi

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

#### <a name="parameters"></a>Parametri

|          Parametro           |                                                                                                                                 Descrizione                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<> di sistema         | Specifica il sistema remoto a cui connettersi (per nome o indirizzo IP). Non utilizzare le barre rovesciate. Se non si utilizza il **/s** opzione, il comando viene eseguito nel computer locale per impostazione predefinita. Questo parametro si applica a tutti i file e cartelle in cui sono specificate nel comando. |
|  /u [\<dominio>\]<UserName>   |                                                          Esegue il comando utilizzando le autorizzazioni dell'account utente specificato. Se non si utilizza il **/u** opzione, le autorizzazioni sono utilizzate per impostazione predefinita di sistema.                                                           |
|       /p [\<password>]       |                                               Specifica la password dell'account utente specificato nella **/u** (opzione). Se non si utilizza il **/p** opzione, un prompt della password viene visualizzata quando viene eseguito il comando.                                                |
| [/FO {elenco \| \| tabella CSV}] |                             Visualizza l'output nel formato specificato. I valori validi per *formato* sono:</br>TABELLA: Visualizza l'output in una tabella.</br>ELENCO: Visualizza l'output in un elenco.</br>CSV: Visualizza l'output in formato valori separati da virgola.                              |
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
Per eseguire una query e visualizzare tutti i file aperti nel SRVPRINC di sistema remoto usando le credenziali per l'utente hiropln nel dominio Domprinc, digitare:
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> In questo esempio, la password è specificata nella riga di comando. Per evitare la visualizzazione della password, omettere il **/p** (opzione). Verrà richiesto per la password, che non viene visualizzata sullo schermo.

## <a name="openfiles-local"></a><a name=BKMK_local></a>/local openfiles

Abilita o disabilita il flag globale Gestisci elenco di oggetti di sistema. Se utilizzata senza parametri, **openfiles /local** Visualizza lo stato corrente del flag globale Maintain Objects List.

### <a name="syntax"></a>Sintassi

```
openfiles /local [on | off]
```

#### <a name="parameters"></a>Parametri

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)