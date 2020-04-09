---
title: ntbackup
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b68b4cca579a5fc27f921ce2f4fcc6976d8e5600
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838014"
---
# <a name="ntbackup"></a>ntbackup



Il comando **Ntbackup** non è disponibile in Windows Vista o windows Server 2008. Utilizzare invece il **wbadmin** comando e sottocomandi per eseguire il backup e ripristinare il computer e i file da un prompt dei comandi.

Non è possibile eseguire il ripristino di backup creati con **ntbackup** utilizzando **wbadmin**. Tuttavia, una versione di **Ntbackup** è disponibile come download per gli utenti di windows Server 2008 e Windows Vista che desiderano ripristinare i backup creati utilizzando **Ntbackup**. Questa versione scaricabile di **Ntbackup** consente di eseguire i recuperi solo dei backup legacy e non può essere utilizzata in computer che eseguono windows Server 2008 o Windows Vista per creare nuovi backup. Per scaricare questa versione di **Ntbackup**, vedere [https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917).

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)