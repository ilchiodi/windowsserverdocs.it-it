---
title: driverquery
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d44a1be300b7178bc2271187344c2fc4ab8815e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377651"
---
# <a name="driverquery"></a>driverquery



Consente agli amministratori di visualizzare un elenco di driver di periferica installati e le relative proprietà. Se utilizzata senza parametri, **driverquery** viene eseguito nel computer locale.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parametri

|         Parametro         |                                                                                                                                         Descrizione                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<sistema >        |                                                                                      Specifica il nome o indirizzo IP di un computer remoto. Non utilizzare le barre rovesciate. Il valore predefinito è il computer locale.                                                                                       |
| /u [\<dominio >\]<Username> | Esegue il comando con le credenziali dell'account utente come specificato dall'utente *o* dal *dominio*\*utente<em>. Per impostazione predefinita, \*\*/s</em>\* usa le credenziali dell'utente attualmente connesso al computer che sta eseguendo il comando. **/u** non può essere utilizzato a meno che non **/s** specificato. |
|      /p \<password >       |                                                                           Specifica la password dell'account utente specificato nella **/u** parametro. **/p** non può essere utilizzato a meno che non **/u** specificato.                                                                            |
|        /fo {tabella         |                                                                                                                                             list                                                                                                                                             |
|            /NH            |                                                                                      Omette la riga di intestazione dalle informazioni sui driver visualizzate. Se non è valido il **/fo** parametro è impostato su **elenco**.                                                                                      |
|            /v             |                                                                                                               Visualizza output dettagliato. **/v** non è valido per i driver firmati.                                                                                                               |
|            /si            |                                                                                                                          Vengono fornite informazioni sui driver firmati.                                                                                                                          |
|            /?             |                                                                                                                             Visualizza la guida al prompt dei comandi.                                                                                                                             |

## <a name="BKMK_examples"></a>Esempi

Per visualizzare un elenco di driver di periferica installati nel computer locale, digitare:
```
driverquery 
```
Per visualizzare l'output in un formato con valori delimitati da virgole (CSV), digitare:
```
driverquery /fo csv 
```
Per nascondere la riga di intestazione nell'output, digitare:
```
driverquery /nh 
```
Utilizzare il **driverquery** comando in un server remoto denominato **server1** utilizzando le credenziali correnti del computer locale, digitare:
```
driverquery /s server1
```
Utilizzare il **driverquery** comando in un server remoto denominato **server1** utilizzando le credenziali **user1** nel dominio **maindom**, tipo:
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)