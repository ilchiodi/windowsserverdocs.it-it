---
title: taskkill
description: Argomento di riferimento per taskkill, che termina una o più attività o processi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 425133f0ec4a9410a83e800f7c252326c9b2f459
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721555"
---
# <a name="taskkill"></a>taskkill

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina una o più attività o processi. È possibile terminare i processi in base al nome di immagine o all'ID di processo. **Taskkill** sostituisce lo strumento **Kill** .

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

### <a name="parameters"></a>Parametri

|         Parametro         |                                                                                                                                        Descrizione                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<> computer       |                                                                                    Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                                                     |
| /u \<dominio>\\ \<nome utente> | Esegue il comando con le autorizzazioni dell'account dell'utente specificato in *nome* utente o *dominio*\\*nomeutente*. **/u** può essere specificato solo se è specificato **/s** . Il valore predefinito è le autorizzazioni dell'utente attualmente connesso al computer che sta eseguendo il comando. |
|      /p \<password>       |                                                                                                   Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                   |
|       > \<filtro/Fi       |          Applica un filtro per selezionare un set di attività. È possibile usare più di un filtro o usare il carattere jolly (**\\**\*) per specificare tutte le attività o i nomi di immagine. Vedere la [tabella seguente per i nomi di filtro](#filter-names-operators-and-values), gli operatori e i valori validi.           |
|     > \<ProcessId/PID     |                                                                                                                 Specifica l'ID del processo da terminare.                                                                                                                 |
|     /im \<imagename>      |                                                                                Specifica il nome dell'immagine del processo da terminare. Usare il carattere jolly (**\\**\*) per specificare tutti i nomi delle immagini.                                                                                |
|            /f             |                                                                    Specifica che i processi verranno interrotti in modo forzato. Questo parametro viene ignorato per i processi remoti. tutti i processi remoti vengono interrotti in modo forzato.                                                                     |
|            /t             |                                                                                                          Termina il processo specificato e tutti i processi figlio avviati da esso.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Filtrare nomi, operatori e valori

| Nome filtro |    Operatori validi     |                                                                Valore/i valido/i                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATO    |         eq, ne         |                                                 ESECUZIONE &#124; NON RISPONDE &#124; SCONOSCIUTA                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  Nome dell'immagine                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valore PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Numero di sessione                                                                |
|   CPUtime   | eq, ne, gt, lt, ge, le | Tempo CPU nel formato <em>HH</em>**:**<em>mm</em>**:**<em>SS</em>, dove *mm* e *SS* sono comprese tra 0 e 59 e *HH* è un numero senza segno |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Utilizzo memoria in KB                                                              |
|  USERNAME   |         eq, ne         |                                               Qualsiasi nome utente*valido (utente o utente di* *dominio*\\ *)*                                               |
|  Servizi   |         eq, ne         |                                                                 Nome del servizio                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Titolo della finestra                                                                 |
|   MODULI   |         eq, ne         |                                                                   Nome DLL                                                                   |

## <a name="remarks"></a>Osservazioni
* I filtri di stato e WINDOWTITLE non sono supportati quando si specifica un sistema remoto.
* Il carattere jolly (**\\**<em>) viene accettato per l'opzione **/im</em> * solo quando viene applicato un filtro.
* La chiusura di processi remoti viene sempre eseguita in modo forzato, indipendentemente dal fatto che sia specificata l'opzione **/f** .
* Se si specifica un nome di computer per il filtro del nome host, si verificherà un arresto e tutti i processi verranno arrestati.
* È possibile utilizzare **TaskList** per determinare l'ID del processo (PID) per il processo da terminare.

## <a name="examples"></a>Esempi

Per terminare i processi con gli ID processo 1230, 1241 e 1253, digitare:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Per terminare in modo forzato il processo Notepad. exe se è stato avviato dal sistema, digitare:

```
taskkill /f /fi USERNAME eq NT AUTHORITY\SYSTEM /im notepad.exe
```

Per terminare tutti i processi nel computer remoto SRVPRINC con il nome di un'immagine che inizia con la nota, usando le credenziali per l'account utente hiropln, digitare:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi IMAGENAME eq note* /im *
```

Per terminare il processo con l'ID di processo 2134 e tutti i processi figlio avviati, ma solo se tali processi sono stati avviati dall'account Administrator, digitare:

```
taskkill /pid 2134 /t /fi username eq administrator
```

Per terminare tutti i processi che hanno un ID processo maggiore o uguale a 1000, indipendentemente dai rispettivi nomi di immagine, digitare:

```
taskkill /f /fi PID ge 1000 /im *
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
