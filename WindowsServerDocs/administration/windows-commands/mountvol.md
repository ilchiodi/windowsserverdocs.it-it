---
title: mountvol
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e31a167d98203b684917aceee2603a29dd478a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846322"
---
# <a name="mountvol"></a>mountvol



Crea, Elimina o elenca un punto di montaggio del volume.

Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
mountvol [<Drive>:]<Path VolumeName>
mountvol [<Drive>:]<Path> /d
mountvol [<Drive>:]<Path> /l
mountvol [<Drive>:]<Path> /p
mountvol /r
mountvol [/n | /e]
mountvol <Drive>: /s
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Drive>:]<Path>|Specifica la directory NTFS esistente in cui risiederà il punto di montaggio.|
|\<VolumeName>|Specifica il nome del volume di destinazione del punto di montaggio. Il nome del volume viene utilizzata la sintassi seguente, dove *GUID* è un identificatore univoco globale:</br>`\\\\?\Volume\{GUID}\`</br>Sono necessarie le parentesi {}.|
|/d|Rimuove il punto di montaggio del volume dalla cartella specificata.|
|/l|Elenca il nome del volume montate per la cartella specificata.|
|/ p|Rimuove il punto di montaggio del volume della directory specificata e lo Smonta il volume di base accetta il volume di base non in linea, rendendolo non montabile. Se altri processi usano il volume **mountvol** chiude tutti gli handle aperti prima di smontare il volume.|
|/r|Rimuove le directory di punto di montaggio di volume e le impostazioni del Registro di sistema per i volumi che non sono più nel sistema, impedisce che venga montato automaticamente e dato il loro volume precedente quando aggiunto nuovamente al sistema i punti di montaggio.|
|/n|Disabilita il montaggio automatico dei nuovi volumi di base. I nuovi volumi non vengono montati automaticamente quando aggiunto al sistema.|
|/e|Abilita nuovamente il montaggio automatico dei nuovi volumi di base.|
|/s|Consente di montare la partizione di sistema EFI nell'unità specificata. Disponibili solo i computer basati su Itanium.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   **Mountvol** consente di collegare i volumi senza che richiede una lettera di unità.
-   I volumi che vengono disinstallati tramite **/p** elencate nell'elenco di volumi come "Non montata UNTIL un punto di montaggio creato". Se il volume ha più di un montaggio punti, usare **/d** per rimuovere i punti di montaggio aggiuntivi prima di usare **/p**. È possibile rendere il volume di base montabile nuovamente mediante l'assegnazione di un punto di montaggio del volume.
-   Se si desidera espandere dello spazio dei volumi senza riformattazioni o sostituire un disco rigido, è possibile aggiungere un percorso di montaggio in un altro volume. Il vantaggio dell'uso di un volume con più percorsi di montaggio prevede che è possibile accedere a tutti i volumi locali usando una singola lettera di unità (ad esempio `C:`). Non è necessario ricordare il volume che corrisponde alla lettera di unità, anche se è comunque possibile montare i volumi locali e assegnare loro le lettere di unità.

## <a name="BKMK_examples"></a>Esempi

Per creare un punto di montaggio, digitare:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)