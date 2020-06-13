---
title: netsh
description: Argomento di riferimento per il comando netsh, che è un'utilità di scripting da riga di comando che consente di visualizzare o modificare la configurazione di rete di un computer in esecuzione in locale o in remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 96fc069d-53c0-4d0a-9f7f-f9f3d49a02bd carmonmills
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c538dd10f86d252390a4e862e7b97204d1c945c9
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721104"
---
# <a name="netsh"></a>netsh

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Utilità di scripting da riga di comando della shell di rete che consente di visualizzare o modificare la configurazione di rete di un computer attualmente in esecuzione in locale o in remoto. È possibile avviare questa utilità dal prompt dei comandi o in Windows PowerShell.

## <a name="syntax"></a>Sintassi

```
netsh [-a <Aliasfile>][-c <Context>][-r <Remotecomputer>][-u [<domainname>\<username>][-p <Password> | [{<NetshCommand> | -f <scriptfile>}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -a`<Aliasfile>` | Specifica che viene restituito il prompt netsh dopo l'esecuzione di Aliasfile e il nome del file di testo che contiene uno o più comandi netsh. |
| -c`<Context>` | Specifica che netsh immette il contesto netsh specificato e il contesto netsh da immettere. |
| -r`<Remotecomputer>` | Specifica il computer remoto da configurare.<p>**Importante:** Se si utilizza questo parametro, è necessario assicurarsi che il servizio Registro di sistema remoto sia in esecuzione nel computer remoto. Se non è in esecuzione, in Windows viene visualizzato un messaggio di errore "percorso di rete non trovato". |
| -u`<domainname>\<username>` | Specifica il dominio e il nome dell'account utente da utilizzare durante l'esecuzione del comando netsh in un account utente. Se si omette il dominio, per impostazione predefinita viene utilizzato il dominio locale. |
| -p`<Password>` | Specifica la password per l'account utente specificato dal `-u <username>` parametro. |
| `<NetshCommand>` | Specifica il comando netsh da eseguire. |
| -f`<scriptfile>` | Chiude il comando netsh dopo l'esecuzione del file di script specificato. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Se si specifica **-r** seguito da un altro comando, netsh esegue il comando nel computer remoto e quindi torna al prompt dei comandi Cmd.exe. Se si specifica **-r** senza un altro comando, netsh si apre in modalità remota. Il processo è simile all'uso di **set machine** al prompt dei comandi netsh. Quando si usa **-r**, impostare solo il computer di destinazione per l'istanza corrente di netsh. Dopo aver chiuso e riaperto netsh, il computer di destinazione viene reimpostato come computer locale. Puoi eseguire i comandi netsh in un computer remoto specificando un nome di computer archiviato in WINS, un nome UNC, un nome Internet che verrà risolto dal server DNS o un indirizzo IP.

- Se il valore stringa contiene spazi tra i caratteri, è necessario racchiudere il valore stringa tra virgolette. Ad esempio: `-r "contoso remote device"`

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
