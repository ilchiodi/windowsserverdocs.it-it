---
title: attrib
description: Argomento i comandi di Windows per **attrib** -Visualizza, imposta o rimuove gli attributi assegnati a file o directory.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f640af2c7957e43dd3f31dfa732bfc887112651
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841092"
---
# <a name="attrib"></a>attrib



Visualizza, imposta o rimuove gli attributi assegnati a file o directory. Se utilizzata senza parametri, **attrib** consente di visualizzare gli attributi di tutti i file nella directory corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{+\|-} r|Set (**+**) o Cancella (**-**) l'attributo di file di sola lettura.|
|{+\|-}a|Set (**+**) o Cancella (**-**) l'attributo di file di archivio.|
|{+\|-} s|Set (**+**) o Cancella (**-**) l'attributo di file di sistema.|
|{+\|-} h|Set (**+**) o Cancella (**-**) l'attributo file nascosto.|
|{+\|-}i|Set (**+**) o Cancella (**-**) l'attributo di file non contenuti indicizzati.|
|[\<Drive>:][<Path>][<FileName>]|Specifica il percorso e nome della directory, file o gruppo di file per il quale si desidera visualizzare o modificare gli attributi. È possibile utilizzare il **?** e **&#42;** caratteri jolly nel *nomefile* parametro per visualizzare o modificare gli attributi per un gruppo di file.|
|/s|Si applica **attrib** e le opzioni della riga di comando per i file corrispondenti nella directory corrente e tutte le relative sottodirectory.|
|/d|Si applica **attrib** e tutte le opzioni della riga di comando per le directory.|
|/l|Si applica **attrib** e tutte le opzioni della riga di comando per il collegamento simbolico, piuttosto che la destinazione del collegamento simbolico.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   È possibile usare caratteri jolly (**?** e **&#42;**) con le *nomefile* parametro per visualizzare o modificare gli attributi per un gruppo di file.
-   Se il sistema dispone di un file (**s**) o nascosta (**h**) set di attributi, è necessario cancellare l'attributo prima di poter impostare tutti gli altri attributi per il file.
-   L'attributo Archive (**un**) contrassegna i file che sono stati modificati dall'ultima volta in cui è stato eseguito il backup. Si noti che il **xcopy** comandi Usa archivio attributi.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare gli attributi di un file denominato News86 che si trova nella directory corrente, digitare:
```
attrib news86 
```
Per assegnare l'attributo di sola lettura del file txt, digitare:
```
attrib +r report.txt 
```
Per rimuovere l'attributo di sola lettura dai file nella directory pubblica e nelle relative sottodirectory in un disco nell'unità B, digitare:
```
attrib -r b:\public\*.* /s 
```
Per impostare l'attributo Archive per tutti i file sull'unità disco e quindi deselezionare l'attributo Archive per i file con estensione bak, digitare:
```
attrib +a a:*.* & attrib -a a:*.bak 
```
