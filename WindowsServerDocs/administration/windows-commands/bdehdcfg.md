---
title: bdehdcfg
description: 'Argomento dei comandi di Windows per **BdeHdCfg** : prepara un disco rigido con le partizioni necessarie per crittografia unità BitLocker.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acfe2582905402f7be149148520784be6ad47453
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382189"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prepara un disco rigido con le partizioni necessarie per Crittografia unità BitLocker. Per la maggior parte delle installazioni di Windows 7 non è necessario utilizzare questo strumento poiché il programma di installazione di BitLocker include la possibilità di preparare e ripartizionare le unità in modo obbligatorio.

> [!WARNING]
> Esiste un conflitto noto con l'impostazione di Criteri di gruppo **Nega accesso in scrittura per unità fisse non protette da BitLocker** che si trova in **Configurazione computer\Modelli amministrativi\Componenti di Windows\Crittografia unità BitLocker\Unità dati fisse**.</br>> Se BdeHdCfg viene eseguito in un computer quando questa impostazione dei criteri è abilitata, è possibile che si verifichino i problemi seguenti:</br>>: Se si è tentato di compattare l'unità e creare l'unità di sistema, le dimensioni dell'unità verranno ridotte e verrà creata una partizione non elaborata. Tuttavia, la partizione non verrà formattata. Viene visualizzato il messaggio di errore seguente: "Impossibile formattare la nuova unità attiva. Potrebbe essere necessario preparare manualmente l'unità per BitLocker."</br>>: Se si è tentato di utilizzare spazio non allocato per creare l'unità di sistema, verrà creata una partizione non elaborata. Tuttavia, la partizione non verrà formattata. Viene visualizzato il messaggio di errore seguente: "Impossibile formattare la nuova unità attiva. Potrebbe essere necessario preparare manualmente l'unità per BitLocker."</br>>: Se si è tentato di unire un'unità esistente nell'unità di sistema, lo strumento non riuscirà a copiare il file di avvio necessario nell'unità di destinazione per creare l'unità di sistema. Viene visualizzato il messaggio di errore seguente: "Configurazione di BitLocker: impossibile copiare i file di avvio. Potrebbe essere necessario preparare manualmente l'unità per BitLocker."</br>> Se viene applicata questa impostazione di criteri, non è possibile ripartizionare un disco rigido perché l'unità è protetta. Se si esegue l'aggiornamento dei computer dell'organizzazione da una versione precedente di Windows e i computer sono stati configurati con una sola partizione, è necessario creare la partizione del sistema BitLocker richiesta prima di applicare l'impostazione relativa ai criteri dei computer.

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[BdeHdCfg: DriveInfo](bdehdcfg-driveinfo.md)|Consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche delle partizioni dell'unità specificata. Vengono elencate solo le partizioni valide. Lo spazio non allocato non viene elencato se sono già presenti quattro partizioni primarie o estese.|
|[BdeHdCfg: destinazione](bdehdcfg-target.md)|Definisce la parte di un'unità da utilizzare come unità di sistema e rende attiva la parte.|
|[BdeHdCfg: newdriveletter](bdehdcfg-newdriveletter.md)|Assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema.|
|[BdeHdCfg: dimensioni](bdehdcfg-size.md)|Determina la dimensione della partizione di sistema durante la creazione di una nuova unità di sistema.|
|[BdeHdCfg: non interattiva](bdehdcfg-quiet.md)|Impedisce la visualizzazione di tutte le azioni e degli errori nell'interfaccia della riga di comando e indica a BdeHdCfg di usare la risposta "Sì" a qualsiasi richiesta Sì/No che può verificarsi durante la preparazione dell'unità successiva.|
|[BdeHdCfg: riavvio](bdehdcfg-restart.md)|Indica che il computer deve essere riavviato al termine della preparazione dell'unità.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'uso di BdeHdCfg con l'unità predefinita per creare una partizione di sistema di 500 MB. Poiché non è specificata alcuna lettera di unità, la nuova partizione di sistema non avrà una lettera di unità.
```
bdehdcfg -target default -size 500
```
Nell'esempio seguente viene illustrato l'utilizzo di BdeHdCfg con l'unità predefinita per creare una partizione di sistema (P:) delle dimensioni predefinite di 300 MB di spazio non allocato nell'unità. Lo strumento non richiederà alcun ulteriore input da parte dell'utente, né verrà visualizzato alcun errore. Al termine della creazione dell'unità di sistema, il computer verrà riavviato automaticamente.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)