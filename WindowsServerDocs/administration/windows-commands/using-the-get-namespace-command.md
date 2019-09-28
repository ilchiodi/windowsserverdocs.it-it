---
title: Utilizzando il comando get-spazio dei nomi
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 607fb758db64cfc938a08b070b520fe2950aa482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363076"
---
# <a name="using-the-get-namespace-command"></a>Utilizzando il comando get-spazio dei nomi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Parametri

|               Parametro               |                                                                                                                                                                                         Descrizione                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Namespace: <Namespace name>      | Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<br /><br />-Server di distribuzione: La sintassi per il nome dello spazio dei nomi è/namspace: WDS: <ImageGroup> @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4. Esempio: **WDS: ImageGroup1/install. wim/1**<br />-Server trasporto: Questo valore deve corrispondere al nome assegnato allo spazio dei nomi quando è stato creato nel server. |
|        [/Server:<Server name>]        |                                                                                                             Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                                              |
| [/Show: client] o [/Details: client] |                                                                                                                                                  Visualizza informazioni sui computer client connessi allo spazio dei nomi specificato.                                                                                                                                                  |

## <a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni su uno spazio dei nomi, digitare:
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
Per visualizzare informazioni su uno spazio dei nomi e i client connessi, digitare uno dei seguenti:
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
  #### <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [usando il comando get-allnamespaces](using-the-get-allnamespaces-command.md)
  [usando il comando New-Namespace](using-the-new-namespace-command.md)
  [usando il comando Remove-Namespace](using-the-remove-namespace-command.md)
  [sottocomando: Start-](subcommand-start-namespace.md) Namespace
