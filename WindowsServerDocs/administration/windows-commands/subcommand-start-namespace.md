---
title: Avvio sottocomando-spazio dei nomi
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55fe4a6136fe4f8e886dc62fff746a1e5ff1898f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370755"
---
# <a name="subcommand-start-namespace"></a>Sottocomando: start-spazio dei nomi

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 avvia uno spazio dei nomi multicast pianificato.
> ## <a name="syntax"></a>Sintassi
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |          Parametro          |                                                                                                                                                                                             Descrizione                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /Namespace:<Namespace name> | Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<br /><br />**server di distribuzione**-   : la sintassi per il nome dello spazio dei nomi è/NAMSPACE: WDS:<Image group>/<Image name>/<Index>. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />**server di trasporto**-   : il nome deve corrispondere al nome assegnato allo spazio dei nomi al momento della creazione nel server. |
> |   [/Server:<Server name>]   |                                                                                                           Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>Esempi
> Per avviare uno spazio dei nomi, digitare uno dei seguenti:
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>riferimenti aggiuntivi
> [Sintassi della riga di comando chiave](command-line-syntax-key.md)
> [utilizzando il comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
> [utilizzando il comando nuovo spazio dei nomi](using-the-new-namespace-command.md)
> [utilizzando il comando remove-spazio dei nomi](using-the-remove-namespace-command.md)
