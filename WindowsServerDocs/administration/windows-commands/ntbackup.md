---
title: ntbackup
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba3aaaa192283e0e1dc1777a27fc13973949784b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723487"
---
# <a name="ntbackup"></a>ntbackup



Il comando **Ntbackup** non è disponibile in Windows Vista o windows Server 2008. Utilizzare invece il **wbadmin** comando e sottocomandi per eseguire il backup e ripristinare il computer e i file da un prompt dei comandi.

Non è possibile eseguire il ripristino di backup creati con **ntbackup** utilizzando **wbadmin**. Tuttavia, una versione di **Ntbackup** è disponibile come download per gli utenti di windows Server 2008 e Windows Vista che desiderano ripristinare i backup creati utilizzando **Ntbackup**. Questa versione scaricabile di **Ntbackup** consente di eseguire i recuperi solo dei backup legacy e non può essere utilizzata in computer che eseguono windows Server 2008 o Windows Vista per creare nuovi backup. Per scaricare questa versione di **Ntbackup**, vedere [https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917).

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)