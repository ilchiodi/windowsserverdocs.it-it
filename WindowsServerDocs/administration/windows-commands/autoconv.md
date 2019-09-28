---
title: autoconv
description: Argomento dei comandi di **Windows per la** conversione della tabella di allocazione file (FAT) e dei volumi Fat32 nel file system NTFS.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bf36be6bcf3dd8f6c61c6ab0d8780ed77dd8903a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383454"
---
# <a name="autoconv"></a>autoconv

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

converte i volumi FAT (file allocation table) e FAT32 nel file system NTFS, lasciando intatti i file e le directory esistenti all'avvio dopo l'esecuzione di **Autochk** . non è possibile convertire in FAT o FAT32 i volumi convertiti nel file system NTFS.
## <a name="remarks"></a>Note
Non è possibile eseguire **Autoconv** nella riga di comando. Questa operazione verrà eseguita solo all'avvio, se impostata tramite **Convert. exe**.
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[Autochk](autochk.md)
[Convert](convert.md)
[utilizzo di file System](https://go.microsoft.com/fwlink/?LinkId=4509)
