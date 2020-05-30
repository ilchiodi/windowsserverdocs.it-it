---
title: Logman update contatore
description: Argomento di riferimento per il comando logman update counter, che aggiorna le proprietà di un agente di raccolta dati del contatore esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f482e58417e5e3246989169bbb01917fcb6503b0
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222763"
---
# <a name="logman-update-counter"></a>Logman update contatore

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiorna le proprietà di un agente di raccolta dati del contatore esistente.

## <a name="syntax"></a>Sintassi

```
logman update counter <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri


| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Eseguire il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -f`<bin|bincirc>` | Specifica il formato di log per l'agente di raccolta dati. |
| -[-] u`<user [password]>` | Specifica l'utente di Esegui come. L'immissione di un oggetto `*` per la password genera una richiesta per la password. La password non viene visualizzata quando la si digita al prompt della password. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Modifiche all'avvio o all'arresto manuale anziché a un'ora di inizio o di fine pianificata. |
| -RF`<[[hh:]mm:]ss>` | Esegue l'agente di raccolta dati per il periodo di tempo specificato. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Inizia la raccolta dei dati all'ora specificata. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Termina la raccolta dei dati all'ora specificata. |
| -si`<[[hh:]mm:]ss>` | Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni. |
| -o`<path|dsn!log>` | Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL. |
| -[-]r | Ripete l'agente di raccolta dati ogni giorno alle ore di inizio e di fine specificate. |
| -[-]a | Accoda un file di log esistente. |
| -[-] Mostra | Sovrascrive un file di log esistente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Connette le informazioni sul controllo delle versioni dei file alla fine del nome del file di log. |
| -[-] RC`<task>` | Esegue il comando specificato ogni volta che il log viene chiuso. |
| -[-] max`<value>` | Dimensioni massime in MB o numero massimo di record di log di SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Quando viene specificata l'ora, creare un nuovo file quando è trascorso il tempo specificato. Quando non viene specificata l'ora, creare un nuovo file quando viene superata la dimensione massima. |
| -y | Risposte sì a tutte le domande senza richiesta di conferma. |
| -CF`<filename>` | Specifica il file di elenco di contatori delle prestazioni da raccogliere. Il file deve contenere un nome di contatore delle prestazioni per ogni riga. |
| -c`<path [path [ ]]>` | Specifica i contatori delle prestazioni per raccogliere. |
| -SC`<value>` | Specifica il numero massimo di campioni da raccogliere con un agente di raccolta dati del contatore delle prestazioni. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

#### <a name="remarks"></a>Commenti

- Dove [-] è elencato, l'aggiunta di un trattino aggiuntivo (-) nega l'opzione.

### <a name="examples"></a>Esempi

Per creare un contatore denominato *perf_log* utilizzando il contatore% tempo processore dalla categoria del contatore processore (_Total), digitare:

```
logman create counter perf_log -c \Processor(_Total)\% Processor time
```

Per aggiornare un contatore esistente denominato *perf_log*, impostando l'intervallo di campionamento su 10, il formato del log su CSV e aggiungendo il controllo delle versioni al nome del file di log nel formato mmgghhmm, digitare:

```
logman update counter perf_log -si 10 -f csv -v mmddhhmm
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando logman create counter](logman-create-counter.md)

- [logman (comando)](logman.md)
