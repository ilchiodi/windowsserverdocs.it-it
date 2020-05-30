---
title: Logman creare traccia
description: Argomento di riferimento per il comando logman create trace, che consente di creare un agente di raccolta dati di traccia eventi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b4dfecd-6f56-4c51-b622-c2054b4aabd7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 015fb7842146e372b36c71fe95a3598bdfa48676
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222990"
---
# <a name="logman-create-trace"></a>Logman creare traccia

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creare una raccolta dati di traccia eventi.

## <a name="syntax"></a>Sintassi

```
logman create trace <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Esegue il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| -ets | Invia comandi direttamente alle sessioni di traccia eventi senza salvare o pianificare. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -f`<bin|bincirc>` | Specifica il formato di log per l'agente di raccolta dati. |
| -[-] u`<user [password]>` | Specifica l'utente di Esegui come. L'immissione di un oggetto `*` per la password genera una richiesta per la password. La password non viene visualizzata quando la si digita al prompt della password. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Modifiche all'avvio o all'arresto manuale anziché a un'ora di inizio o di fine pianificata. |
| -RF`<[[hh:]mm:]ss>` | Esegue l'agente di raccolta dati per il periodo di tempo specificato. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Inizia la raccolta dei dati all'ora specificata. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Termina la raccolta dei dati all'ora specificata. |
| -o`<path|dsn!log>` | Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL. |
| -[-]r | Ripete l'agente di raccolta dati ogni giorno alle ore di inizio e di fine specificate. |
| -[-]a | Accoda un file di log esistente. |
| -[-] Mostra | Sovrascrive un file di log esistente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Connette le informazioni sul controllo delle versioni dei file alla fine del nome del file di log. |
| -[-] RC`<task>` | Esegue il comando specificato ogni volta che il log viene chiuso. |
| -[-] max`<value>` | Dimensioni massime in MB o numero massimo di record di log di SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Quando si specifica Time, crea un nuovo file quando è trascorso il tempo specificato. Quando non viene specificato Time, crea un nuovo file quando viene superata la dimensione massima. |
| -y | Risposte sì a tutte le domande senza richiesta di conferma. |
| -CT`<perf|system|cycle>` | Specifica il tipo di orologio della sessione di traccia eventi. |
| -ln`<logger_name>` | Specifica il nome del logger per sessioni di traccia eventi. |
| -ft`<[[hh:]mm:]ss>` | Specifica il timer di cancellazione di sessione di traccia eventi. |
| -[-] p`<provider [flags [level]]>` | Specifica un unico provider di traccia eventi per abilitare. |
| -PF`<filename>` | Specifica un file più provider di traccia di eventi da abilitare. Il file deve essere un file di testo contenente un provider per ogni riga. |
| -[-] rt | Esegue la sessione di traccia eventi in modalità in tempo reale. |
| -ul [-] | Esegue la sessione di traccia eventi in User. |
| -BS`<value>` | Specifica la dimensione del buffer di sessione di traccia eventi in kb. |
| -NB`<min max>` | Specifica il numero di buffer di sessione di traccia eventi. |
| -modalità`<globalsequence|localsequence|pagedmemory>` | Specifica la modalità di logger della sessione di traccia eventi, tra cui:<ul><li>**Globalsequence** : specifica che il tracciatore di eventi aggiunge un numero di sequenza a ogni evento ricevuto indipendentemente dalla sessione di traccia che ha ricevuto l'evento.</li><li>**Localsequence** -specifica che l'evento Tracer aggiunge numeri di sequenza per gli eventi ricevuti in una sessione di traccia specifica. Quando si utilizza questa opzione, i numeri di sequenza duplicati possono esistere in tutte le sessioni, ma saranno univoci all'interno di ogni sessione di traccia.</li><li>**Pagedmemory** : specifica che il tracciatore di eventi utilizza la memoria di paging anziché il pool di memoria non di paging predefinito per le allocazioni di buffer interne.</li></ul> |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

#### <a name="remarks"></a>Commenti

- Dove [-] è elencato, l'aggiunta di un trattino aggiuntivo (-) nega l'opzione.

### <a name="examples"></a>Esempi

Per creare un agente di raccolta dati di traccia eventi denominato *trace_log*, usando non meno di 16 e non più di 256 buffer, con ogni buffer di dimensioni pari a 64KB, inserendo i risultati in c:\logfile, digitare:

```
logman create trace trace_log -nb 16 256 -bs 64 -o c:\logfile
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Logman Update Trace](logman-update-trace.md)

- [logman (comando)](logman.md)
