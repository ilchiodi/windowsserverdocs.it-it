---
title: mountvol
description: Argomento di riferimento per il comando mountvol, che crea, Elimina o elenca un punto di montaggio del volume.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e024ed1e0684da4e1450343dfd097b43fde5c8f4
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354551"
---
# <a name="mountvol"></a>mountvol

Crea, Elimina o elenca un punto di montaggio del volume. È anche possibile collegare volumi senza richiedere una lettera di unità.

## <a name="syntax"></a>Sintassi

```
mountvol [<drive>:]<path volumename>
mountvol [<drive>:]<path> /d
mountvol [<drive>:]<path> /l
mountvol [<drive>:]<path> /p
mountvol /r
mountvol [/n|/e]
mountvol <drive>: /s
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<drive>:]<path>` | Specifica la directory NTFS esistente in cui si trova il punto di montaggio. |
| `<volumename>` | Specifica il nome del volume che rappresenta la destinazione del punto di montaggio. Il nome del volume usa la sintassi seguente, dove *GUID* è un identificatore univoco globale: `\\?\volume\{GUID}\` . Sono necessarie le parentesi quadre `{ }` . |
| /d | Rimuove il punto di montaggio del volume dalla cartella specificata. |
| /l | Elenca il nome del volume montato per la cartella specificata. |
| / p | Rimuove il punto di montaggio del volume dalla directory specificata, smonta il volume di base e porta il volume di base offline, rendendolo non montabile. Se il volume viene usato da altri processi, **mountvol** chiude tutti gli handle aperti prima di smontare il volume. |
| /r | Rimuove le directory del punto di montaggio del volume e le impostazioni del registro di sistema per i volumi che non sono più presenti nel sistema, impedendone l'installazione automatica e assegnando i punti di montaggio del volume precedenti quando vengono aggiunti di nuovo al sistema. |
| /n | Disabilita il montaggio automatico dei nuovi volumi di base. Quando vengono aggiunti al sistema, i nuovi volumi non vengono montati automaticamente. |
| /e | Consente di riattivare il montaggio automatico dei nuovi volumi di base. |
| /s | Monta la partizione di sistema EFI sull'unità specificata. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Commenti

- Se si smonta il volume quando si usa il **/p** parametro, l'elenco dei volumi mostrerà il volume come non montato fino a quando non viene creato un punto di montaggio del volume.

- Se il volume contiene più di un punto di montaggio, utilizzare **/d** per rimuovere i punti di montaggio aggiuntivi prima di utilizzare **/p**. È possibile rendere il volume di base montabile di nuovo assegnando un punto di montaggio del volume.

- Se è necessario espandere lo spazio del volume senza riformattare o sostituire un disco rigido, è possibile aggiungere un percorso di montaggio a un altro volume. Il vantaggio dell'utilizzo di un volume con diversi percorsi di montaggio è che è possibile accedere a tutti i volumi locali utilizzando una singola lettera di unità (ad esempio `C:` ). Non è necessario ricordare quale volume corrisponde alla lettera di unità, sebbene sia ancora possibile montare volumi locali e assegnare loro lettere di unità.

## <a name="examples"></a>Esempio

Per creare un punto di montaggio, digitare:

```
mountvol \sysmount \\?\volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
