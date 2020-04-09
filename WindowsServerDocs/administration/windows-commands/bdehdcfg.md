---
title: bdehdcfg
description: Windows Commands argomento per **BdeHdCfg**, che prepara un disco rigido con le partizioni necessarie per crittografia unità BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9adf8bbfb655e0820fcff6385d3663fc7abbd9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851004"
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

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
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
| /? | Visualizza la Guida dal prompt dei comandi. |

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Nell'esempio seguente viene illustrato l'uso di BdeHdCfg con l'unità predefinita per creare una partizione di sistema di 500 MB. Poiché non è specificata alcuna lettera di unità, la nuova partizione di sistema non avrà una lettera di unità.

```
bdehdcfg -target default -size 500
```

Nell'esempio seguente viene illustrato l'utilizzo di BdeHdCfg con l'unità predefinita per creare una partizione di sistema (P:) delle dimensioni predefinite di 300 MB di spazio non allocato nell'unità. Lo strumento non richiederà alcun ulteriore input da parte dell'utente, né verrà visualizzato alcun errore. Al termine della creazione dell'unità di sistema, il computer verrà riavviato automaticamente.

```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)