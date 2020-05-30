---
title: Logman aggiornare api
description: Argomento di riferimento per il comando logman update API, che aggiorna le proprietà di un agente di raccolta dati di rilevamento API esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0087edc7cd96bf2bf7611d9a3975d97384c02949
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222775"
---
# <a name="logman-update-api"></a>Logman aggiornare api

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiorna le proprietà di un agente di raccolta dati di traccia API esistente.

## <a name="syntax"></a>Sintassi

```
logman update api <[-n] <name>> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Esegue il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -f`<bin|bincirc>` | Specifica il formato di log per l'agente di raccolta dati. |
| -[-] u`<user [password]>` | Specifica l'utente di Esegui come. L'immissione di un oggetto `*` per la password genera una richiesta per la password. La password non viene visualizzata quando la si digita al prompt della password. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | È stato modificato per avviare o arrestare manualmente anziché un'ora di inizio o di fine pianificata. |
| -RF`<[[hh:]mm:]ss>` | Eseguire l'agente di raccolta dati per il periodo di tempo specificato. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Avviare la raccolta dei dati all'ora specificata. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Terminare la raccolta dei dati all'ora specificata. |
| -si`<[[hh:]mm:]ss>` | Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni. |
| -o`<path|dsn!log>` | Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL. |
| -[-]r | Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine. |
| -[-]a | Accodare un file di log esistente. |
| -[-] Mostra | Sovrascrivere un file di log esistente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Connette le informazioni sul controllo delle versioni dei file alla fine del nome del file di log. |
| -[-] RC`<task>` | Eseguire il comando specificato ogni volta che il log viene chiuso. |
| -[-] max`<value>` | Dimensioni massime in MB o numero massimo di record di log di SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Quando si specifica Time, crea un nuovo file quando è trascorso il tempo specificato. Quando non viene specificato Time, crea un nuovo file quando viene superata la dimensione massima. |
| -y | Rispondere Sì a tutte le domande senza chiedere conferma. |
| -mods`<path [path [...]]>` | Specifica l'elenco dei moduli per registrare le chiamate API da. |
| -inAPI` <module!api [module!api [...]]>` | Specifica l'elenco di chiamate API da includere nella registrazione. |
| -exapis`<module!api [module!api [...]]>` | Specifica l'elenco di chiamate API da escludere dalla registrazione. |
| -ano [-] | Log (-ano) solo nomi di API o non registrare solo i nomi delle API (-ano). |
| -ricorsive [-] | Log (-ricorsivo) o non registrare (-ricorsive) le API in modo ricorsivo oltre il primo livello. |
| -exe`<value>` | Specifica il percorso completo di un file eseguibile per la traccia API. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

#### <a name="remarks"></a>Commenti

- Dove [-] è elencato, l'aggiunta di un trattino aggiuntivo (-) nega l'opzione.

### <a name="examples"></a>Esempi

Per aggiornare un contatore di traccia API esistente denominato *trace_notepad*, per il file eseguibile c:\Windows\Notepad.exe., escludendo la chiamata API TlsGetValue prodotta dal modulo Kernel32. dll, digitare:

```
logman update api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando logman create API](logman-create-api.md)

- [logman (comando)](logman.md)
