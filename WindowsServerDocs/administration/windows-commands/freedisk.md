---
title: freedisk
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 138d637ba3da983ccb931d489f7c22b651e07a0c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817222"
---
# <a name="freedisk"></a>freedisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica se è disponibile la quantità specificata di spazio su disco prima di continuare il processo di installazione.

## <a name="syntax"></a>Sintassi
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/s <computer>|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale. Questo parametro si applica a tutti i file e cartelle specificate nel comando.|
|/u [<Domain>\\]<User>|Esegue lo script con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema.|
|/p [<Password>]|Specifica la password dell'account utente specificato nella **/u**.|
|/d <Drive>|Specifica l'unità per il quale si desidera scoprire la disponibilità di spazio libero. È necessario specificare <Drive>per un computer remoto.|
|<Value>|Verifica che una quantità specifica di spazio libero su disco. È possibile specificare <Value>in byte, KB, MB, GB, TB, PB, EB, ZB o YB.|
## <a name="remarks"></a>Note
-   Usando il **/s**, **/u**, e **/p** opzioni della riga di comando sono disponibili solo quando si utilizza **/s**. È necessario utilizzare **/p** con **/u**per fornire la password utente s.
-   per le installazioni automatiche, è possibile usare **freedisk** nei file batch di installazione per verificare la presenza di spazio libero dei prerequisiti prima di continuare con l'installazione.
-   Quando si usa **freedisk** in un file batch, restituisce un **0** se lo spazio sia sufficiente e un **1** se non c'è spazio sufficiente.
## <a name="BKMK_examples"></a>Esempi
Per determinare se sono presenti almeno 50 MB di spazio libero disponibile nell'unità c, digitare:
```
freedisk 50mb 
```
Nella schermata viene visualizzato output simile al seguente:
```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
