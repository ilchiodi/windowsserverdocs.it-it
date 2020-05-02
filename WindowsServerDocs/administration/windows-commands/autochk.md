---
title: autochk
description: Argomento di riferimento per il comando Autochk, che viene eseguito all'avvio del computer e prima di Windows Server per verificare l'integrità logica di un file system.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a767e8f6cebee9c946f53e0403198384b7c0b790
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718782"
---
# <a name="autochk"></a>autochk

Viene eseguito quando il computer viene avviato e prima di Windows Server inizia a verificare l'integrità logica di un file system.

**Autochk. exe** è una versione di **chkdsk** eseguita solo su dischi NTFS e solo prima dell'avvio di Windows Server. non è possibile eseguire **Autochk** direttamente dalla riga di comando. Al contrario, **Autochk** viene eseguito nelle situazioni seguenti:

- Se si tenta di eseguire **chkdsk** sul volume di avvio.

- Se **chkdsk** non può ottenere l'utilizzo esclusivo del volume.

- Se il volume è contrassegnato come dirty.

## <a name="remarks"></a>Osservazioni

> [!WARNING]
> Lo strumento da riga di comando **Autochk** non può essere eseguito direttamente dalla riga di comando. Usare invece lo strumento da riga di comando **chkntfs** per configurare il modo in cui si vuole che **Autochk** venga eseguito all'avvio.
>
> - È possibile usare **chkntfs** con il parametro **/x** per impedire l'esecuzione di **Autochk** in un volume specifico o in più volumi.
>
> - Usare lo strumento da riga di comando **chkntfs. exe** con il parametro **/t** per modificare il ritardo di Autochk da 0 secondi a un massimo di 3 giorni (259.200 secondi). Tuttavia, un ritardo prolungato indica che il computer non viene avviato fino a quando non scade il tempo o fino a quando non si preme un tasto per annullare **Autochk**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando chkdsk](chkdsk.md)

- [comando chkntfs](chkntfs.md)
