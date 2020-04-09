---
title: autoconv
description: Windows Commands Topic for **Autoconv**, che converte i volumi FAT (file allocation table) e Fat32 nel file system NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0e388a1d4fd79567ef0562197e3181bbbc46f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851114"
---
# <a name="autoconv"></a>autoconv

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte i volumi FAT (file allocation table) e FAT32 nel file system NTFS, lasciando intatti i file e le directory esistenti all'avvio dopo l'esecuzione di **Autochk** . non è possibile convertire in FAT o FAT32 i volumi convertiti nel file system NTFS.

## <a name="remarks"></a>Note

Non è possibile eseguire **Autoconv** nella riga di comando. Questa operazione verrà eseguita solo all'avvio, se impostata tramite **Convert. exe**.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [autochk](autochk.md)

- [convert](convert.md)

- [Uso dei file System](https://go.microsoft.com/fwlink/?LinkId=4509)
