---
title: mountvol
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34a98a273274f7982bfdd970710c04178fed4f5a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839384"
---
# <a name="mountvol"></a>mountvol



Crea, Elimina o elenca un punto di montaggio del volume.

Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<unità >:]<Path>|Specifica la directory NTFS esistente in cui si trova il punto di montaggio.|
|\<VolumeName >|Specifica il nome del volume che rappresenta la destinazione del punto di montaggio. Il nome del volume usa la sintassi seguente, dove *GUID* è un identificatore univoco globale:</br>`\\\\?\Volume\{GUID}\`</br>Le parentesi quadre {} sono obbligatorie.|
|/d|Rimuove il punto di montaggio del volume dalla cartella specificata.|
|/l|Elenca il nome del volume montato per la cartella specificata.|
|/p|Rimuove il punto di montaggio del volume dalla directory specificata, smonta il volume di base e porta il volume di base offline, rendendolo non montabile. Se il volume viene usato da altri processi, **mountvol** chiude tutti gli handle aperti prima di smontare il volume.|
|/r|Rimuove le directory del punto di montaggio del volume e le impostazioni del registro di sistema per i volumi che non sono più presenti nel sistema, impedendone l'installazione automatica e assegnando i punti di montaggio del volume precedenti quando vengono aggiunti di nuovo al sistema.|
|/n|Disabilita il montaggio automatico dei nuovi volumi di base. Quando vengono aggiunti al sistema, i nuovi volumi non vengono montati automaticamente.|
|/e|Consente di riattivare il montaggio automatico dei nuovi volumi di base.|
|/s|Monta la partizione di sistema EFI sull'unità specificata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   **Mountvol** consente di collegare volumi senza richiedere una lettera di unità.
-   I volumi smontati con **/p** sono elencati nell'elenco volumi come non montati fino a quando non viene creato un punto di montaggio del volume. Se il volume ha più di un punto di montaggio, utilizzare **/d** per rimuovere i punti di montaggio aggiuntivi prima di utilizzare **/p**. È possibile rendere il volume di base montabile di nuovo assegnando un punto di montaggio del volume.
-   Se è necessario espandere lo spazio del volume senza riformattare o sostituire un disco rigido, è possibile aggiungere un percorso di montaggio a un altro volume. Il vantaggio dell'utilizzo di un volume con diversi percorsi di montaggio è che è possibile accedere a tutti i volumi locali utilizzando una singola lettera di unità, ad esempio `C:`. Non è necessario ricordare quale volume corrisponde alla lettera di unità, sebbene sia ancora possibile montare volumi locali e assegnare loro lettere di unità.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per creare un punto di montaggio, digitare:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
