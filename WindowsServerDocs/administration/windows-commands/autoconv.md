---
title: autoconv
description: Argomento di riferimento per il comando Autoconv, che converte i volumi FAT (file allocation table) e FAT32 nel file system NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea8f55270435c6632be2e527569b4a4b4ca81136
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718781"
---
# <a name="autoconv"></a>autoconv

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte i volumi FAT (file allocation table) e FAT32 nel file system NTFS, lasciando intatti i file e le directory esistenti all'avvio dopo l'esecuzione di **Autochk** . non è possibile convertire in FAT o FAT32 i volumi convertiti nel file system NTFS.

> [!IMPORTANT]
> Non è possibile eseguire **Autoconv** dalla riga di comando. Questa operazione può essere eseguita solo all'avvio, se impostata tramite **Convert. exe**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Autochk](autochk.md)

- [Convert (comando)](convert.md)
