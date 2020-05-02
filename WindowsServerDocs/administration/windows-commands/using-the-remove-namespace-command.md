---
title: Remove-Namespace
description: Argomento di riferimento per Remove-Namespace, che rimuove uno spazio dei nomi personalizzato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef329a53a7f212c1810af2e4ced11a2abf76840
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720330"
---
# <a name="using-the-remove-namespace-command"></a>Utilizzando il comando remove-spazio dei nomi

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove uno spazio dei nomi personalizzato.

## <a name="syntax"></a>Sintassi
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|Namespace<Namespace name>|Specifica il nome dello spazio dei nomi. Non è il nome descrittivo e deve essere univoco.<p>-   **Servizio ruolo server di distribuzione**: la sintassi per il nome dello spazio dei nomi<ImageGroup>/<ImageName>/<Index>è/namespace: WDS:. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />-   **Servizio ruolo server di trasporto**: questo valore deve corrispondere al nome assegnato allo spazio dei nomi quando è stato creato nel server.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/Force|rimuove immediatamente lo spazio dei nomi e termina tutti i client. Si noti che, a meno che non si specifichi **/Force**, i client esistenti possono completare il trasferimento, ma non è possibile aggiungere nuovi client.|
## <a name="examples"></a>Esempi
Per arrestare uno spazio dei nomi (client corrente può completare il trasferimento, ma i nuovi client non sono in grado di creare un join), tipo:
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
Per forzare la terminazione di tutti i client, digitare:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
di sintassi della riga di comando[usando il comando Get-AllNamespaces](using-the-get-allnamespaces-command.md)

[usando il comando New-Namespace](using-the-new-namespace-command.md)[sottocomando: Start-Namespace](subcommand-start-namespace.md)
