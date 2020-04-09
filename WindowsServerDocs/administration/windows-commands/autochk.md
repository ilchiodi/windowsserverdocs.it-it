---
title: autochk
description: Argomento dei comandi di Windows per **Autochk**, che viene eseguito all'avvio del computer e prima di Windows Server per verificare l'integrità logica di un file System.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30ccdbb6aad6e116988ae9c782e3ff359642453e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851124"
---
# <a name="autochk"></a>autochk

Viene eseguito all'avvio del computer e prima di Windows Server&reg; 2008 R2 per verificare l'integrità logica di un file system.

**Autochk. exe** è una versione di **chkdsk** eseguita solo su dischi NTFS e solo prima dell'avvio di Windows Server 2008 R2. Non è possibile eseguire **Autochk** direttamente dalla riga di comando. Al contrario, **Autochk** viene eseguito nelle situazioni seguenti:

- Se si tenta di eseguire **chkdsk** sul volume di avvio.

- Se **chkdsk** non può ottenere l'utilizzo esclusivo del volume.

- Se il volume è contrassegnato come dirty.

## <a name="remarks"></a>Note

> [!WARNING]
> Lo strumento da riga di comando **Autochk** non può essere eseguito direttamente dalla riga di comando. Usare invece lo strumento da riga di comando **chkntfs** per configurare il modo in cui si vuole che **Autochk** venga eseguito all'avvio.
> -  È possibile usare **chkntfs** con il parametro **/x** per impedire l'esecuzione di **Autochk** in un volume specifico o in più volumi.
>
> - Usare lo strumento da riga di comando **chkntfs. exe** con il parametro **/t** per modificare il ritardo di Autochk da 0 secondi a un massimo di 3 giorni (259.200 secondi). Tuttavia, un ritardo prolungato indica che il computer non viene avviato fino a quando non scade il tempo o fino a quando non si preme un tasto per annullare **Autochk**.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [CHKDSK](chkdsk.md)

- [Chkntfs](chkntfs.md)

- [Risoluzione dei problemi relativi a dischi e file System](https://go.microsoft.com/fwlink/?LinkId=4527)