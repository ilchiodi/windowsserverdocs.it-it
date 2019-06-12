---
title: Il sottocomando start-Namespace
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9a54f849580a139470c2cca43ba57fee60dc81ec
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441152"
---
# <a name="subcommand-start-namespace"></a>Sottocomando: start-spazio dei nomi

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 viene avviato uno spazio dei nomi multicast pianificato.
> ## <a name="syntax"></a>Sintassi
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |          Parametro          |                                                                                                                                                                                             Descrizione                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / Namespace:<Namespace name> | Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<br /><br />-   **Server di distribuzione**: La sintassi per il nome dello spazio dei nomi è /namspace:<Image group>/<Image name>/<Index>. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />-   **Server di trasporto**: Questo nome deve corrispondere al nome specificato per lo spazio dei nomi quando è stato creato nel server. |
> |   [/Server:<Server name>]   |                                                                                                           Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>Esempi
> Per avviare uno spazio dei nomi, digitare uno dei seguenti:
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>Riferimenti aggiuntivi
> [Sintassi della riga di comando chiave](command-line-syntax-key.md)
> [utilizzando il comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
> [utilizzando il comando nuovo spazio dei nomi](using-the-new-namespace-command.md)
> [utilizzando il comando remove-spazio dei nomi](using-the-remove-namespace-command.md)
