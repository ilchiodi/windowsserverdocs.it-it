---
title: autochk
description: "Argomento dei comandi di Windows per **Autochk** : viene eseguito quando il computer viene avviato e prima di Windows Server inizia a verificare l'integrità logica di un file System."
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 76e54d14879cefd4661a1ca7f1c3b8ee7ec58de2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382347"
---
# <a name="autochk"></a>autochk



Viene eseguito all'avvio del computer e prima di Windows Server® 2008 R2 per verificare l'integrità logica di un file system.

**Autochk. exe** è una versione di **chkdsk** eseguita solo su dischi NTFS e solo prima dell'avvio di Windows Server 2008 R2. Non è possibile eseguire **Autochk** direttamente dalla riga di comando. Al contrario, **Autochk** viene eseguito nelle situazioni seguenti:
-   Se si tenta di eseguire **chkdsk** sul volume di avvio
-   Se **chkdsk** non può ottenere l'utilizzo esclusivo del volume
-   Se il volume è contrassegnato come Dirty

## <a name="remarks"></a>Note

> -   [!WARNING]
>     Lo strumento da riga di comando **Autochk** non può essere eseguito direttamente dalla riga di comando. Usare invece lo strumento da riga di comando **chkntfs** per configurare il modo in cui si vuole che **Autochk** venga eseguito all'avvio.
> -   È possibile usare **chkntfs** con il parametro **/x** per impedire l'esecuzione di **Autochk** in un volume specifico o in più volumi.
> -   Usare lo strumento da riga di comando **chkntfs. exe** con il parametro **/t** per modificare il ritardo di Autochk da 0 secondi a un massimo di 3 giorni (259.200 secondi). Tuttavia, un ritardo prolungato indica che il computer non viene avviato fino a quando non scade il tempo o fino a quando non si preme un tasto per annullare **Autochk**.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[CHKDSK](chkdsk.md)

[Chkntfs](chkntfs.md)

[Risoluzione dei problemi relativi a dischi e file System](https://go.microsoft.com/fwlink/?LinkId=4527)