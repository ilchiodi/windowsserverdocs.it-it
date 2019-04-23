---
title: autoconv
description: 'Argomento i comandi di Windows per **autoconv** : converte file tabella di allocazione (Fat) e volumi Fat32 in NTFS i file system.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d135da085558f12a51c8febfd72aa805e1d12f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872492"
---
# <a name="autoconv"></a>autoconv

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte file allocation table (Fat) e volumi Fat32 al file system NTFS, lasciando intatti file e directory esistenti all'avvio dopo **autochk** viene eseguito. Impossibile convertire volumi convertiti nel file system NTFS nuovamente in Fat o Fat32.
## <a name="remarks"></a>Note
Non è possibile eseguire **autoconv** dalla riga di comando. Questo verrà eseguito solo all'avvio, se impostata tramite **convert.exe**.
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[autochk](autochk.md)
[Converti](convert.md)
[funziona con File System](https://go.microsoft.com/fwlink/?LinkId=4509)
