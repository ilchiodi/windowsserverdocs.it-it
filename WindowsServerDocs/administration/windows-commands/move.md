---
title: spostamento
description: Argomento di riferimento per il comando Move, che consente di spostare uno o più file da una directory a un'altra directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 283ee793769d991c1932eb2271c5117354bdf6a4
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354502"
---
# <a name="move"></a>spostamento

Sposta uno o più file da una directory a un'altra directory.

> [!IMPORTANT]
> Se si trasferiscono file crittografati a un volume che non supporta i risultati di Encrypting File System (EFS), verrà generato un errore. È necessario innanzitutto decrittografare i file o spostarli in un volume che supporta EFS.

## <a name="syntax"></a>Sintassi

```
move [{/y|-y}] [<source>] [<target>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /y | Interrompe la richiesta di conferma che si desidera sovrascrivere un file di destinazione esistente. Questo parametro può essere preimpostato nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questo set di impostazioni utilizzando il parametro **-y** . Per impostazione predefinita, viene richiesto di richiedere prima di sovrascrivere i file, a meno che il comando non venga eseguito dall'interno di uno script batch. |
| -y | Avvia la richiesta di conferma che si desidera sovrascrivere un file di destinazione esistente. |
| `<source>` | Specifica il percorso e il nome dei file da spostare. Per spostare o rinominare una directory, l' *origine* deve essere il percorso e il nome della directory corrente. |
| `<target>` | Specifica il percorso e il nome in cui spostare i file. Per spostare o rinominare una directory, la *destinazione* deve essere il percorso e il nome della directory desiderata. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempio

Per spostare tutti i file con estensione xls dalla directory *\Data* alla directory *\ Second_Q \Rapporti* , digitare:

```
move \data\*.xls \second_q\reports\
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
