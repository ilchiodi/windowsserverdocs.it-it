---
title: bootcfg addsw
description: Argomento i comandi di Windows per **bootcfg addsw** -consente di aggiungere opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 768e9c5bcf8a5d272927d013ff4accf5c3237219
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862872"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di aggiungere opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parametri
|Nome|Definizione|
|----|-------|
|/s <computer>|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.|
|/u <Domain>\\<User>|Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.|
|/p <Password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/mm <MaximumRAM>|Specifica la quantità massima di RAM, in megabyte, che è possibile utilizzare il sistema operativo. Il valore deve essere uguale o maggiore di 32 MB.|
|/BV|Aggiunge il **/basevideo** opzione specificata <OSEntryLineNum>, indicando al sistema operativo da usare la modalità VGA standard per il driver video installato.|
|/SO|Aggiunge il **/sos** opzione specificata *NumRigaVoceSO*, indicando al sistema operativo per visualizzare i nomi dei driver di dispositivo durante il caricamento.|
|/NG|Aggiunge il **/noguiboot** opzione specificata <OSEntryLineNum>, disabilitare l'indicatore di stato che precede il CTRL + ALT + CANC dei messaggi di richiesta.|
|/id <OSEntryLineNum>|Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /addsw** comando:
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
