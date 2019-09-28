---
title: ntbackup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ebbe71fd5547311beb36747d32d695823e0f0059
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372684"
---
# <a name="ntbackup"></a>ntbackup



Il comando **Ntbackup** non è disponibile in Windows Vista o windows Server 2008. Utilizzare invece il **wbadmin** comando e sottocomandi per eseguire il backup e ripristinare il computer e i file da un prompt dei comandi.

Non è possibile eseguire il ripristino di backup creati con **ntbackup** utilizzando **wbadmin**. Tuttavia, una versione di **Ntbackup** è disponibile come download per gli utenti di windows Server 2008 e Windows Vista che desiderano ripristinare i backup creati utilizzando **Ntbackup**. Questa versione scaricabile di **Ntbackup** consente di eseguire i recuperi solo dei backup legacy e non può essere utilizzata in computer che eseguono windows Server 2008 o Windows Vista per creare nuovi backup. Per scaricare questa versione di **Ntbackup**, vedere [https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)