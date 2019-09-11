---
title: taskkill
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9050788a06b66e54be59c69dd8cc837092123b00
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878139"
---
# <a name="taskkill"></a>taskkill

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina una o più attività o processi. È possibile terminare i processi in base al nome di immagine o all'ID di processo. **Taskkill** sostituisce lo strumento **Kill** .
Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Parametri

|         Parametro         |                                                                                                                                        Descrizione                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<> computer       |                                                                                    Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                                                     |
| /u \<dominio >\\nomeutente>\< | Esegue il comando con le autorizzazioni dell'account dell'utente specificato in *nome* utente o *dominio*\\*nomeutente*. **/u** può essere specificato solo se è specificato **/s** . Il valore predefinito è le autorizzazioni dell'utente attualmente connesso al computer che sta eseguendo il comando. |
|      /p \<password >       |                                                                                                   Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                   |
|       > \<filtro/Fi       |          Applica un filtro per selezionare un set di attività. È possibile usare più di un filtro o usare il carattere jolly ( **\\** \*) per specificare tutte le attività o i nomi di immagine. Vedere la [tabella seguente per i nomi di filtro](#filter-names-operators-and-values), gli operatori e i valori validi.           |
|     > \<ProcessId/PID     |                                                                                                                 Specifica l'ID del processo da terminare.                                                                                                                 |
|     /im \<ImageName >      |                                                                                Specifica il nome dell'immagine del processo da terminare. Usare il carattere jolly ( **\\** \*) per specificare tutti i nomi delle immagini.                                                                                |
|            /f             |                                                                    Specifica che i processi verranno interrotti in modo forzato. Questo parametro viene ignorato per i processi remoti. tutti i processi remoti vengono interrotti in modo forzato.                                                                     |
|            /t             |                                                                                                          Termina il processo specificato e tutti i processi figlio avviati da esso.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Filtrare nomi, operatori e valori

| Nome filtro |    Operatori validi     |                                                                Valore/i valido/i                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATO    |         eq, ne         |                                                 ESECUZIONE &#124; NON RISPOSTA &#124; SCONOSCIUTA                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  Nome immagine                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valore PID                                                                   |
|   SESSIONE   | eq, ne, gt, lt, ge, le |                                                                Numero sessione                                                                |
|   CPUtime   | eq, ne, gt, lt, ge, le | Tempo CPU nel formato <em>HH</em> **:** <em>mm</em> **:** <em>SS</em>, dove *mm* e *SS* sono comprese tra 0 e 59 e *HH* è un numero senza segno |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Utilizzo memoria in KB                                                              |
|  NOME UTENTE   |         eq, ne         |                                               Qualsiasi*nome utente* *valido (utente o utente di* *dominio*\\)                                               |
|  SERVIZI   |         eq, ne         |                                                                 Nome del servizio                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Titolo della finestra                                                                 |
|   MODULI   |         eq, ne         |                                                                   Nome DLL                                                                   |

## <a name="remarks"></a>Note
* I filtri di stato e WINDOWTITLE non sono supportati quando si specifica un sistema remoto.
* Il carattere jolly ( **\\** <em>) viene accettato per l'opzione * */im</em>*  solo quando viene applicato un filtro.
* La chiusura di processi remoti viene sempre eseguita in modo forzato, indipendentemente dal fatto che sia specificata l'opzione **/f** .
* Se si specifica un nome di computer per il filtro del nome host, si verificherà un arresto e tutti i processi verranno arrestati.
* È possibile utilizzare **TaskList** per determinare l'ID del processo (PID) per il processo da terminare.

## <a name="examples"></a>Esempi

Per terminare i processi con gli ID processo 1230, 1241 e 1253, digitare:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Per terminare in modo forzato il processo "Notepad. exe" se è stato avviato dal sistema, digitare:

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Per terminare tutti i processi nel computer remoto "SRVPRINC" con un nome di immagine che inizia con "nota", mentre si usano le credenziali per l'account utente hiropln, digitare:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Per terminare il processo con l'ID di processo 2134 e tutti i processi figlio avviati, ma solo se tali processi sono stati avviati dall'account Administrator, digitare:

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Per terminare tutti i processi che hanno un ID processo maggiore o uguale a 1000, indipendentemente dai rispettivi nomi di immagine, digitare:

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
