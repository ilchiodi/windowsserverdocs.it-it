---
title: importazione Logman e Logman Export
description: Argomento di riferimento per l'importazione Logman e Logman Export, che importa un insieme agenti di raccolta dati da un file XML o esporta un insieme agenti di raccolta dati in un file XML.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ad664548dce51d7631a6d1a02d628af91e1921f
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721141"
---
# <a name="logman-import-and-logman-export"></a>importazione Logman e Logman Export

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Importa un insieme agenti di raccolta dati da un file XML o esporta un insieme agenti di raccolta dati in un file XML.

## <a name="syntax"></a>Sintassi

```
logman import <[-n] <name> <-xml <name> [options]
logman export <[-n] <name> <-xml <name> [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -s`<computer name>` | Eseguire il comando nel computer remoto specificato. |
| -config`<value>` | Specifica il file di impostazioni che contiene le opzioni di comando. |
| [-n]`<name>` | Nome dell'oggetto di destinazione. |
| -XML`<name>` | Nome del file XML per importare o esportare. |
| -ets | Invia comandi direttamente alle sessioni di traccia eventi senza salvare o pianificare. |
| -[-] u`<user [password]>` | Specifica l'utente di Esegui come. L'immissione di un oggetto `*` per la password genera una richiesta per la password. La password non viene visualizzata quando la si digita al prompt della password. |
| -y | Risposte sì a tutte le domande senza richiesta di conferma. |
| /? | Vengono visualizzate sensibile al contesto della Guida. |

### <a name="examples"></a>Esempio

Per importare il file XML *c:\windows\perf_log.xml* dal computer *server_1* come insieme agenti di raccolta dati denominato *perf_log*, digitare:

```
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [logman (comando)](logman.md)
