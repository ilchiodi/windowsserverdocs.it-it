---
title: bdehdcfg
description: Argomento i comandi di Windows per **bdehdcfg** -prepara un disco rigido con le partizioni necessarie per crittografia unità BitLocker.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a4fda562f4aa6b8caa885fcd70155b7150c91d12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869702"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prepara un disco rigido con le partizioni necessarie per crittografia unità BitLocker. La maggior parte delle installazioni di Windows 7 non saranno necessario utilizzare questo strumento perché la configurazione di BitLocker consente di preparare e partizionare unità in base alle esigenze.

> [!WARNING]
> Esiste un conflitto noto con l'impostazione di Criteri di gruppo **Nega accesso in scrittura per unità fisse non protette da BitLocker** che si trova in **Configurazione computer\Modelli amministrativi\Componenti di Windows\Crittografia unità BitLocker\Unità dati fisse**.</br>> Se Bdehdcfg viene eseguito in un computer quando questa impostazione dei criteri è abilitata, è possibile riscontrare i problemi seguenti:</br>>: Se si è tentato di compattare l'unità e creare l'unità del sistema, le dimensioni dell'unità verranno correttamente ridotte e sarà possibile creare una partizione raw. Tuttavia, la partizione non verrà formattata. Viene visualizzato il messaggio di errore seguente: "Impossibile formattare la nuova unità attiva. Potrebbe essere necessario preparare manualmente l'unità per BitLocker."</br>>-Se si è tentato di utilizzare spazio non allocato per creare l'unità del sistema, verrà creata una partizione raw. Tuttavia, la partizione non verrà formattata. Viene visualizzato il messaggio di errore seguente: "Impossibile formattare la nuova unità attiva. Potrebbe essere necessario preparare manualmente l'unità per BitLocker."</br>>-Se si è tentato di unire un'unità esistente in unità di sistema, lo strumento non sarà possibile copiare il file di avvio richiesto nell'unità di destinazione per creare l'unità del sistema. Viene visualizzato il messaggio di errore seguente: "Configurazione di BitLocker: impossibile copiare i file di avvio. Potrebbe essere necessario preparare manualmente l'unità per BitLocker."</br>> Se questa impostazione dei criteri è imposta, non è possibile partizionare un disco rigido perché l'unità è protetta. Se si esegue l'aggiornamento dei computer dell'organizzazione da una versione precedente di Windows e i computer sono stati configurati con una sola partizione, è necessario creare la partizione del sistema BitLocker richiesta prima di applicare l'impostazione relativa ai criteri dei computer.

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|Consente di visualizzare la lettera di unità, le dimensioni totali, lo spazio disponibile massimo e le caratteristiche della partizione delle partizioni nell'unità specificata. Vengono elencate solo le partizioni valide. Lo spazio non allocato non viene elencato se sono già presenti quattro partizioni primarie o estese.|
|[Bdehdcfg: target](bdehdcfg-target.md)|Definisce la parte di un'unità da utilizzare come unità di sistema e rende attiva la parte.|
|[Bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|Assegna una nuova lettera di unità alla parte di un'unità usata come unità di sistema.|
|[Bdehdcfg: size](bdehdcfg-size.md)|Determina le dimensioni della partizione di sistema quando viene creata una nuova unità di sistema.|
|[Bdehdcfg: quiet](bdehdcfg-quiet.md)|Evitare la visualizzazione di tutte le azioni e gli errori nell'interfaccia della riga di comando e indirizza Bdehdcfg per usare la risposta "Yes" Sì / prompt che possono verificarsi durante il successivo non unità preparazione.|
|[Bdehdcfg: restart](bdehdcfg-restart.md)|Indirizza il riavvio dopo la preparazione dell'unità del computer.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrata Bdehdcfg usati con l'unità predefinita per creare una partizione di sistema di 500 MB. Poiché non viene specificato lettera di unità, con la nuova partizione di sistema non è una lettera di unità.
```
bdehdcfg -target default -size 500
```
Nell'esempio seguente viene illustrata Bdehdcfg usati con l'unità predefinita per creare una partizione di sistema (p) della dimensione predefinita di 300 MB esaurito lo spazio non allocato nell'unità. Lo strumento non richiederà all'utente di qualsiasi ulteriore input né verranno visualizzati gli eventuali errori. Dopo aver creato l'unità del sistema, il computer verrà riavviato automaticamente.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)