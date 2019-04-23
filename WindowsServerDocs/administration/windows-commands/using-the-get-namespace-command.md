---
title: Utilizzando il comando get-spazio dei nomi
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd11880b6e733850b522c3a06152ac7ce7a28841
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889662"
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
|Parametro|Descrizione|
|-------|--------|
|/ Namespace:<Namespace name>|Specifica il nome dello spazio dei nomi. Si noti che questo non è il nome descrittivo e deve essere univoco.<br /><br />-Server distribuzione: La sintassi per il nome dello spazio dei nomi è /namspace:<ImageGroup>/<ImageName>/<Index>. Ad esempio: **WDS:ImageGroup1/install.wim/1**<br />-Trasporto Server: Questo valore deve corrispondere al nome specificato per lo spazio dei nomi quando è stato creato nel server.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|[/ Client: show] o [/ Dettagli: client]|Consente di visualizzare informazioni sui computer client connessi allo spazio dei nomi specificato.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni su uno spazio dei nomi, digitare:
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
Per visualizzare informazioni su uno spazio dei nomi e i client connessi, digitare uno dei seguenti:
-   Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
-   Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[utilizzando il comando nuovo Namespace](using-the-new-namespace-command.md)
[Using il comando remove-Namespace](using-the-remove-namespace-command.md)
[sottocomando: start-Namespace](subcommand-start-namespace.md)
