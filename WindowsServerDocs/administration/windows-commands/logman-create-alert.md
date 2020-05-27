---
title: Logman crea avviso
description: Argomento di riferimento per il comando logman create Alert, che consente di creare un agente di raccolta dati di avviso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d84dd52dd9d2d8ca45dc42df88a59cd4c16b54c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820481"
---
# <a name="logman-create-alert"></a>Logman crea avviso

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un agente di raccolta dati di avviso.

## <a name="syntax"></a>Sintassi

```
logman create alert <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Eseguire il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
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
| -[-] CNF`<[[hh:]mm:]ss>` | Quando si specifica Time, crea un nuovo file quando è trascorso il tempo specificato. Quando non viene specificato Time, crea un nuovo file quando viene superata la dimensione massima. |
| -y | Risposte sì a tutte le domande senza richiesta di conferma. |
| -CF`<filename>` | Specifica il file di elenco di contatori delle prestazioni da raccogliere. Il file deve contenere un nome di contatore delle prestazioni per ogni riga. |
| -el [-] | Abilita o disabilita la segnalazione di Log eventi. |
| -TH`<threshold [threshold [...]]>` | Specificare i contatori e i relativi valori di soglia per un avviso. |
| -[-] ADR`<name>` | Specifica l'insieme agenti di raccolta dati per l'avvio quando viene generato un avviso. |
| -[-] TN`<task>` | Specifica l'attività da eseguire quando viene generato un avviso. |
| -[-] es`<argument>` | Specifica gli argomenti di attività da utilizzare con l'attività specificata tramite -tn. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

#### <a name="remarks"></a>Osservazioni

- Dove [-] è elencato, l'aggiunta di un trattino aggiuntivo (-) nega l'opzione.

### <a name="examples"></a>Esempi

Per creare un nuovo avviso denominato *new_alert*, che viene attivato quando il contatore delle prestazioni% tempo processore nel gruppo di contatori processore (_Total) supera il valore del contatore 50, digitare:

```
logman create alert new_alert -th \Processor(_Total)\% Processor time>50
```

> [!NOTE]
> Il valore soglia definito è basato sul valore raccolto dal contatore, quindi in questo esempio il valore di 50 equivale a 50% tempo processore.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman](logman.md)
