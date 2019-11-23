---
title: bootcfg delete
description: 'Argomento sui comandi di Windows per **bootcfg delete** : Elimina una voce del sistema operativo nella sezione dei sistemi operativi del file Boot. ini.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d2298c07af32e66a2ffcebb85ec780da762be58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380014"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina una voce del sistema operativo nella sezione [Operating Systems] del file Boot. ini.

## <a name="syntax"></a>Sintassi
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parametri

|         Termine         |                                                                                             Definizione                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User>  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>     |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                        |
| <OSEntryLineNum>/ID |        Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da eliminare. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.        |
|          /?          |                                                                                Visualizza la guida al prompt dei comandi.                                                                                 |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /delete**comando:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
