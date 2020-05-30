---
title: Logman update cfg
description: Argomento di riferimento per il comando logman update cfg, che aggiorna le proprietà di un agente di raccolta dati di configurazione esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9da4e8b4-3be5-42d3-b0b4-c429630c35c4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af4d61372aa5b10d5bb2a5c93e16df391eadfd2f
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222771"
---
# <a name="logman-update-cfg"></a>Logman update cfg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiorna le proprietà di un agente di raccolta dati di configurazione esistente.

## <a name="syntax"></a>Sintassi

```
logman update cfg <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri


| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Esegue il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -[-] u`<user [password]>` | Specifica l'utente di Esegui come. L'immissione di un oggetto \* per la password genera una richiesta per la password. La password non viene visualizzata quando la si digita al prompt della password. |
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
| -[-] CNF`<[[hh:]mm:]ss>` | Quando si specifica Time, crea un nuovo file quando è trascorso il tempo specificato. Quando non viene specificato Time, crea un nuovo file quando viene superata la dimensione massima. |
| -y | Risposte sì a tutte le domande senza richiesta di conferma. |
| -[-] ni | Abilita (-NI) o Disabilita (-NI) la query dell'interfaccia di rete. |
| -reg`<path [path [...]]>` | Specifica i valori del Registro di sistema per raccogliere. |
| -Gest`<query [query [...]]>` | Specifica gli oggetti WMI per raccogliere mediante il linguaggio di query SQL. |
| -FTC`<path [path [...]]>` | Specifica il percorso completo per i file da raccogliere. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

#### <a name="remarks"></a>Commenti

- Dove [-] è elencato, l'aggiunta di un trattino aggiuntivo (-) nega l'opzione.

### <a name="examples"></a>Esempi

Per aggiornare un agente di raccolta dati di configurazione denominato *cfg_log*, per raccogliere la chiave del registro di sistema `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\` , digitare:

```
logman update cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando logman create cfg](logman-create-cfg.md)

- [logman (comando)](logman.md)
