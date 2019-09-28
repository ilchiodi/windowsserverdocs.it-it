---
title: Utilizzando il comando remove-spazio dei nomi
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b4c087442c43fd885fe4554cb29f9b2788420e05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362787"
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
|/Namespace: <Namespace name>|Specifica il nome dello spazio dei nomi. Non è il nome descrittivo e deve essere univoco.<br /><br />**servizio ruolo server di distribuzione**-   : La sintassi per il nome dello spazio dei nomi è/namespace: WDS: <ImageGroup> @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4. Esempio: **WDS: ImageGroup1/install. wim/1**<br />**servizio ruolo server di trasporto**-   : Questo valore deve corrispondere al nome assegnato allo spazio dei nomi al momento della creazione nel server.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/Force|rimuove immediatamente lo spazio dei nomi e termina tutti i client. Si noti che, a meno che non si specifichi **/Force**, i client esistenti possono completare il trasferimento, ma non è possibile aggiungere nuovi client.|
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
