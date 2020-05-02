---
title: Avvio sottocomando-spazio dei nomi
description: Argomento di riferimento per il sottocomando Start-Namespace, che avvia uno spazio dei nomi multicast pianificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1562fcb6c61533fcc9994e9011bf7d61154c06f7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721665"
---
# <a name="subcommand-start-namespace"></a>Sottocomando: start-spazio dei nomi

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia uno spazio dei nomi multicast pianificato.

## <a name="syntax"></a>Sintassi
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>Parametri

|          Parametro          |                                                                                                                                                                                             Descrizione                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace: nome dello spazio dei nomi <| Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<p>-   **Server di distribuzione**: la sintassi per il nome dello spazio dei nomi<Image group>/<Image name>/<Index>è/namspace: WDS:. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />-   **Server di trasporto**: questo nome deve corrispondere al nome assegnato allo spazio dei nomi al momento della creazione nel server. |
|   [/Server:<Server name>]   |                                                                                                           Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                           |

## <a name="examples"></a>Esempi
Per avviare uno spazio dei nomi, digitare uno dei seguenti:
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usando il comando New-Namespace](using-the-new-namespace-command.md)
[usando il comando Remove-Namespace](using-the-remove-namespace-command.md)
