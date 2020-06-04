---
title: mstsc
description: Argomento di riferimento per il comando mstsc, che consente di creare connessioni ai server host sessione Desktop remoto o ad altri computer remoti, di modificare un file di configurazione con estensione RDP (Connessione Desktop remoto) esistente e di migrare i file di connessione legacy creati con gestione connessione client in nuovi file di connessione RDP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6620cc2f954e43a6e68369f9b1f3480c1fc508c
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354351"
---
# <a name="mstsc"></a>mstsc

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di creare connessioni a server host sessione Desktop remoto o altri computer remoti, modificare un file di configurazione di Connessione Desktop remoto (con estensione RDP) esistente e migrare i file di connessione legacy creati con gestione connessione client in nuovi file di connessione RDP.

## <a name="syntax"></a>Sintassi

```
mstsc.exe [<connectionfile>] [/v:<server>[:<port>]] [/admin] [/f] [/w:<width> /h:<height>] [/public] [/span]
mstsc.exe /edit <connectionfile>
mstsc.exe /migrate
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ------------|
| `<connectionfile>` | Specifica il nome di un file con estensione RDP per la connessione. |
| /v:`<server>[:<port>]` | Specifica il computer remoto e, facoltativamente, il numero di porta a cui si desidera connettersi. |
| /admin | Consente di connettersi a una sessione per l'amministrazione del server. |
| /f | Avvia Connessione Desktop remoto in modalità schermo intero. |
| /w`<width>` | Specifica la larghezza della finestra di Desktop remoto. |
| /h`<height>` | Specifica l'altezza della finestra di Desktop remoto. |
| /Public | Esegue Desktop remoto in modalità pubblica. In modalità pubblica, le password e le bitmap non vengono memorizzate nella cache. |
| /span | Consente di abbinare la larghezza e l'altezza Desktop remoto al desktop virtuale locale, che si estende tra più monitor, se necessario. |
| /Edit`<connectionfile>` | Apre il file con estensione rdp specificato per la modifica. |
| /migrate | Esegue la migrazione dei file di connessione legacy creati con gestione connessione client a nuovi file di connessione RDP. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Default. RDP viene archiviato per ogni utente come file nascosto nella cartella **documenti** dell'utente.

- I file con estensione RDP creati dall'utente vengono salvati per impostazione predefinita nella cartella **documenti** dell'utente, ma possono essere salvati ovunque.

- Per estendersi tra i monitoraggi, è necessario che i monitoraggi usino la stessa risoluzione e siano allineati orizzontalmente, ovvero side-by-side. Attualmente non è disponibile alcun supporto per l'espansione verticale di più monitoraggi nel sistema client.

### <a name="examples"></a>Esempio

Per connettersi a una sessione in modalità a schermo intero, digitare:

```
mstsc /f
```

Per aprire un file denominato *nomefile. RDP* per la modifica, digitare:

```
mstsc /edit filename.rdp
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
