---
title: Get-AllNamespaces
description: Argomento di riferimento per Get-AllNamespaces, che visualizza informazioni su tutti gli spazi dei nomi in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 710918eb11ef7a746716a1a2bff9200cfa1d98c1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720000"
---
# <a name="get-allnamespaces"></a>Get-AllNamespaces

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parametri

|         Parametro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale. |                        |
| [/ Provider di contenuti:<name>] |                                                        Visualizza gli spazi dei nomi per solo il provider di contenuto specificato.                                                         |                        |
|      [/ Show: client]      |                            Supportato solo per Windows Server 2008. Visualizza informazioni sui computer client connessi allo spazio dei nomi.                             |                        |
|    [/Details: client]     |                           Supportato solo per Windows Server 2008 R2. Visualizza informazioni sui computer client connessi allo spazio dei nomi.                           |                        |
|  [/ExcludedeletePending]  |                                                              Esclude tutte le trasmissioni disattivate dall'elenco.                                                              |                        |

## <a name="examples"></a>Esempi
Per visualizzare tutti gli spazi dei nomi, digitare:
```
wdsutil /Get-AllNamespaces
```
Per visualizzare tutti gli spazi dei nomi ad eccezione di quelli che vengono disattivati, digitare:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /details:Clients /ExcludedeletePending
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Chiave](command-line-syntax-key.md)
  di sintassi della riga di comando
  [con il comando New-Namespace](using-the-new-namespace-command.md)
  [usando il comando Remove-Namespace](using-the-remove-namespace-command.md)[sottocomando: Start-Namespace](subcommand-start-namespace.md)
