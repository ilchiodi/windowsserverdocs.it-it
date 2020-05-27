---
title: Logman creare cfg
description: Argomento di riferimento per il comando logman create cfg, che consente di creare un agente di raccolta dati di configurazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bfc87093-3ff5-4e19-aa93-d185fb8e2239
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38d6b02351e515230a0041369217ba248ada4793
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820461"
---
# <a name="logman-create-cfg"></a>Logman creare cfg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un agente di raccolta dati di configurazione.

## <a name="syntax"></a>Sintassi

```
logman create cfg <[-n] <name>> [options]
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

#### <a name="remarks"></a>Osservazioni

- Dove [-] è elencato, l'aggiunta di un trattino aggiuntivo (-) nega l'opzione.

### <a name="examples"></a>Esempi

Per creare un agente di raccolta dati di configurazione denominato cfg_log, usando la chiave del registro di sistema `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\` , digitare:

```
logman create cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\\
```

Per creare un agente di raccolta dati di configurazione denominato cfg_log, che registra tutti gli oggetti WMI dalla `root\wmi` colonna di database `MSNdis_Vendordriverversion` , digitare:

```
logman create cfg cfg_log -mgt root\wmi:select * FROM MSNdis_Vendordriverversion
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman](logman.md)
