---
title: bdehdcfg
description: Argomento di riferimento per il comando BdeHdCfg, che prepara un disco rigido con le partizioni necessarie per Crittografia unità BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bcc901847bb8d687d59bc3270dab39de0af8d60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718563"
---
# <a name="bdehdcfg"></a>bdehdcfg

Prepara un disco rigido con le partizioni necessarie per Crittografia unità BitLocker. Per la maggior parte delle installazioni di Windows 7 non è necessario utilizzare questo strumento poiché il programma di installazione di BitLocker include la possibilità di preparare e ripartizionare le unità in modo obbligatorio.

> [!WARNING]
> Esiste un conflitto noto con l'impostazione di Criteri di gruppo **Nega accesso in scrittura per unità fisse non protette da BitLocker** che si trova in **Configurazione computer\Modelli amministrativi\Componenti di Windows\Crittografia unità BitLocker\Unità dati fisse**.
>
>Se BdeHdCfg viene eseguito in un computer quando questa impostazione dei criteri è abilitata, è possibile che si verifichino i problemi seguenti:
>
>- Se si è tentato di ridurre le dimensioni dell'unità e di creare l'unità di sistema, le dimensioni dell'unità verranno correttamente ridotte e sarà possibile creare una partizione raw. Tuttavia, la partizione non verrà formattata. Viene visualizzato il messaggio di errore seguente: Impossibile formattare la nuova unità attiva. Potrebbe essere necessario preparare manualmente l'unità per BitLocker.
>
>- Se si è tentato di utilizzare spazio non allocato per creare l'unità di sistema, le dimensioni dell'unità verranno correttamente ridotte e sarà possibile creare una partizione raw. Tuttavia, la partizione non verrà formattata. Viene visualizzato il messaggio di errore seguente: Impossibile formattare la nuova unità attiva. Potrebbe essere necessario preparare manualmente l'unità per BitLocker.
>
>- Se si è tentato di unire un'unità esistente all'unità di sistema, lo strumento non sarà in grado di copiare il file di avvio richiesto nell'unità di destinazione per creare un'unità di sistema. Viene visualizzato il messaggio di errore seguente: Impossibile copiare i file di avvio. Potrebbe essere necessario preparare manualmente l'unità per BitLocker.
>
>- Se viene applicata questa impostazione relativa ai criteri, non è possibile partizionare un disco rigido perché l'unità è protetta. Se si esegue l'aggiornamento dei computer dell'organizzazione da una versione precedente di Windows e i computer sono stati configurati con una sola partizione, è necessario creare la partizione del sistema BitLocker richiesta prima di applicare l'impostazione relativa ai criteri dei computer.

## <a name="syntax"></a>Sintassi

```
bdehdcfg [–driveinfo <drive_letter>] [-target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}] [–newdriveletter] [–size <size_in_mb>] [-quiet]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |----------- |
| [BdeHdCfg: DriveInfo](bdehdcfg-driveinfo.md) | Consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche delle partizioni dell'unità specificata. Vengono elencate solo le partizioni valide. Lo spazio non allocato non viene elencato se sono già presenti quattro partizioni primarie o estese. |
| [BdeHdCfg: destinazione](bdehdcfg-target.md) | Definisce la parte di un'unità da utilizzare come unità di sistema e rende attiva la parte. |
| [BdeHdCfg: newdriveletter](bdehdcfg-newdriveletter.md) | Assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema. |
| [BdeHdCfg: dimensioni](bdehdcfg-size.md) | Determina la dimensione della partizione di sistema durante la creazione di una nuova unità di sistema. |
| [BdeHdCfg: non interattiva](bdehdcfg-quiet.md) | Impedisce la visualizzazione di tutte le azioni e degli errori nell'interfaccia della riga di comando e indica a BdeHdCfg di usare la risposta sì a qualsiasi richiesta Sì/No che può verificarsi durante la preparazione dell'unità successiva. |
| [BdeHdCfg: riavvio](bdehdcfg-restart.md) | Indica che il computer deve essere riavviato al termine della preparazione dell'unità. |
| /? | Visualizza la Guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
