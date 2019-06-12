---
title: Utilizzando il comando get-AllNamespaces
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b77bb80238ee63cc0d71d88592d75850720e33b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440518"
---
# <a name="using-the-get-allnamespaces-command"></a>Utilizzando il comando get-AllNamespaces

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su tutti gli spazi dei nomi in un server.
## <a name="syntax"></a>Sintassi
Windows Server 2008:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
## <a name="parameters"></a>Parametri

|         Parametro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale. |                        |
| [/ Provider di contenuti:<name>] |                                                        Visualizza gli spazi dei nomi per solo il provider di contenuto specificato.                                                         |                        |
|      [/ Show: client]      |                            Supportato solo per Windows Server 2008. Visualizza informazioni sui computer client connessi allo spazio dei nomi.                             |                        |
|    [/ Dettagli: client]     |                           Supportato solo per Windows Server 2008 R2. Visualizza informazioni sui computer client connessi allo spazio dei nomi.                           |                        |
|  [/ExcludedeletePending]  |                                                              Esclude tutte le trasmissioni disattivate dall'elenco.                                                              |                        |

## <a name="BKMK_examples"></a>Esempi
Per visualizzare tutti gli spazi dei nomi, digitare:
```
wdsutil /Get-AllNamespaces
```
Per visualizzare tutti gli spazi dei nomi ad eccezione di quelli che vengono disattivati, digitare:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>Riferimenti aggiuntivi
  [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [utilizzando il comando nuovo spazio dei nomi](using-the-new-namespace-command.md)
  [utilizzando il comando remove-Namespace](using-the-remove-namespace-command.md)
  [sottocomando: start-spazio dei nomi](subcommand-start-namespace.md)
