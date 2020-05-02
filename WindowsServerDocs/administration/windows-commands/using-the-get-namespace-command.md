---
title: get-Namespace
description: Argomento di riferimento per get-Namespace, che visualizza le informazioni su uno spazio dei nomi personalizzato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76980d2add9ee9b7584812c9d366408f8770b681
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719747"
---
# <a name="get-namespace"></a>get-Namespace

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su uno spazio dei nomi personalizzato.

## <a name="syntax"></a>Sintassi
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Parametri

|               Parametro               |                                                                                                                                                                                         Descrizione                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Namespace<Namespace name>      | Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<p>-Server di distribuzione: la sintassi per il nome dello spazio dei nomi<ImageGroup>/<ImageName>/<Index>è/namspace: WDS:. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />-Server trasporto: questo valore deve corrispondere al nome assegnato allo spazio dei nomi quando è stato creato nel server. |
|        [/Server:<Server name>]        |                                                                                                             Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                                              |
| [/Show: client] o [/Details: client] |                                                                                                                                                  Visualizza informazioni sui computer client connessi allo spazio dei nomi specificato.                                                                                                                                                  |

## <a name="examples"></a>Esempi
Per visualizzare le informazioni su uno spazio dei nomi, digitare:
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Per visualizzare informazioni su uno spazio dei nomi e i client connessi, digitare uno dei seguenti:
- Windows Server 2008:`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2:`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Chiave](command-line-syntax-key.md)
  di sintassi della riga di comando usando il comando[Get-AllNamespaces](using-the-get-allnamespaces-command.md)
  [usando il comando New-Namespace](using-the-new-namespace-command.md)
  
  [usando il comando Remove-Namespace](using-the-remove-namespace-command.md)[sottocomando: Start-Namespace](subcommand-start-namespace.md)
