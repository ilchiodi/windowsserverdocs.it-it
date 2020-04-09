---
title: get-Namespace
description: Windows Commands argomento per get-Namespace, che visualizza le informazioni su uno spazio dei nomi personalizzato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58e84ec5c82ea3c2663b38bd2e53f65f2cf47519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830884"
---
# <a name="get-namespace"></a>get-Namespace

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su uno spazio dei nomi personalizzato.

## <a name="syntax"></a>Sintassi
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Parametri

|               Parametro               |                                                                                                                                                                                         Descrizione                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Namespace:<Namespace name>      | Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<p>-Server di distribuzione: la sintassi per il nome dello spazio dei nomi è/namspace: WDS:<ImageGroup>/<ImageName>/<Index>. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />-Server trasporto: questo valore deve corrispondere al nome assegnato allo spazio dei nomi quando è stato creato nel server. |
|        [/Server:<Server name>]        |                                                                                                             Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                                              |
| [/Show: client] o [/Details: client] |                                                                                                                                                  Visualizza informazioni sui computer client connessi allo spazio dei nomi specificato.                                                                                                                                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare le informazioni su uno spazio dei nomi, digitare:
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Per visualizzare informazioni su uno spazio dei nomi e i client connessi, digitare uno dei seguenti:
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  usando il comando [get-allnamespaces](using-the-get-allnamespaces-command.md)
  [usando il comando New-Namespace](using-the-new-namespace-command.md)
  [usando il comando Remove-Namespace](using-the-remove-namespace-command.md)
  [sottocomando: Start-Namespace](subcommand-start-namespace.md)
