---
title: autochk
description: "Argomento i comandi di Windows per **autochk** : viene eseguito quando il computer viene avviato e prima di Windows Server a partire verificare l'integrità logica di un file system."
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c26d42410e5466950ede4f9aa059e315030588
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435032"
---
# <a name="autochk"></a>autochk



Viene eseguito dopo che il computer viene avviato e versioni precedenti a Windows Server® 2008 R2 a partire verificare l'integrità logica di un file system.

**Autochk.exe** è una versione di **Chkdsk** che viene eseguito solo sui dischi NTFS e solo prima dell'avvio di Windows Server 2008 R2. **Autochk** non può essere eseguito direttamente dalla riga di comando. Al contrario, **Autochk** viene eseguito nelle situazioni seguenti:
-   Se si prova a eseguire **Chkdsk** nel volume di avvio
-   Se **Chkdsk** non è possibile ottenere l'utilizzo esclusivo del volume
-   Se il volume è contrassegnato come dirty

## <a name="remarks"></a>Note

> -   [!WARNING]
>     Il **Autochk** lo strumento da riga di comando non è possibile eseguire direttamente dalla riga di comando. Usare invece i **Chkntfs** dello strumento da riga di comando per configurare nel modo desiderato **Autochk** da eseguire all'avvio.
> -   È possibile usare **Chkntfs** con il **/x** parametro per evitare **Autochk** dall'esecuzione in un volume specifico o più volumi.
> -   Usare la **Chkntfs.exe** dello strumento da riga di comando con il **/t** parametro per modificare il ritardo Autochk da 0 secondi a un massimo di 3 giorni (259200 secondi). Tuttavia, un ritardo prolungato indica che il computer non viene avviato finché non trascorre il tempo o fino a quando non si preme un tasto per annullare **Autochk**.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[CHKDSK](chkdsk.md)

[chkntfs](chkntfs.md)

[Risoluzione dei problemi relativi a dischi e File System](https://go.microsoft.com/fwlink/?LinkId=4527)