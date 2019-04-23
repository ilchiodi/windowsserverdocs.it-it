---
title: ntbackup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 783f73eba2aeaf9f30c5c1e12a623f1f87f24ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846342"
---
# <a name="ntbackup"></a>ntbackup



Il **ntbackup** comando non è disponibile in Windows Vista o Windows Server 2008. Utilizzare invece il **wbadmin** comando e sottocomandi per eseguire il backup e ripristinare il computer e i file da un prompt dei comandi.

Non è possibile eseguire il ripristino di backup creati con **ntbackup** utilizzando **wbadmin**. Tuttavia, una versione di **ntbackup** è disponibile come download per Windows Server 2008 e Windows Vista gli utenti che desiderano ripristinare i backup creati utilizzando **ntbackup**. Questa versione scaricabile di **ntbackup** consente di eseguire il ripristino solo di backup legacy e non è utilizzabile nei computer che eseguono Windows Server 2008 o Windows Vista per creare nuovi backup. Per scaricare questa versione di **ntbackup**, vedere [ https://go.microsoft.com/fwlink/?LinkId=82917 ](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)