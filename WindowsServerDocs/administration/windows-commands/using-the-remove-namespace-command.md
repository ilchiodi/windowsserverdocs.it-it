---
title: Utilizzando il comando remove-spazio dei nomi
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115c0a90a60e18ee4b89758200773d1dfec2163f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842042"
---
# <a name="using-the-remove-namespace-command"></a>Utilizzando il comando remove-spazio dei nomi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove uno spazio dei nomi personalizzato.
## <a name="syntax"></a>Sintassi
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/ Namespace:<Namespace name>|Specifica il nome dello spazio dei nomi. Non è il nome descrittivo e deve essere univoco.<br /><br />-   **Servizio ruolo Server di distribuzione**: La sintassi per il nome dello spazio dei nomi è /namespace:<ImageGroup>/<ImageName>/<Index>. Ad esempio:  **WDS:ImageGroup1/install.wim/1**<br />-   **Servizio ruolo Server di trasporto**: Questo valore deve corrispondere al nome specificato per lo spazio dei nomi quando è stato creato nel server.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|[/force]|Rimuove lo spazio dei nomi immediatamente e termina tutti i client. Si noti che, a meno che non si specifica **/Force**, i client esistenti possono completare il trasferimento, ma non sono in grado di aggiungere nuovi client.|
## <a name="BKMK_examples"></a>Esempi
Per arrestare uno spazio dei nomi (client corrente può completare il trasferimento, ma i nuovi client non sono in grado di creare un join), tipo:
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
Per forzare la terminazione di tutti i client, digitare:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[utilizzando il comando nuovo spazio dei nomi](using-the-new-namespace-command.md)
[sottocomando: start-spazio dei nomi](subcommand-start-namespace.md)
