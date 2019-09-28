---
title: Utilizzando il comando get-Server
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7e0ee4529858b16cdc63ea1d6d358a190b8b1a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392114"
---
# <a name="using-the-get-server-command"></a>Utilizzando il comando get-Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni dal server di servizi di distribuzione Windows specificato.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ Mostra: {configurazione &#124; Immagini &#124; All}|Specifica il tipo di informazioni da restituire.<br /><br />-   **config** restituisce le informazioni di configurazione.<br />-   **images** restituisce informazioni sui gruppi di immagini, le immagini di avvio e le immagini di installazione.<br />-    restituisce**tutte** le informazioni di configurazione e le informazioni sull'immagine.|
|[/detailed]|È possibile usare questa opzione con **/Show: images** o **/Show: All** per indicare che devono essere restituiti tutti i metadati delle immagini da ogni immagine. Se non si usa l'opzione **/detailed** , il comportamento predefinito consiste nel restituire il nome dell'immagine, la descrizione e il nome del file.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni sul server, digitare:
```
wdsutil /Get-Server /Show:Config
```
Per visualizzare informazioni dettagliate sul server, digitare:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando Enable-Server](using-the-enable-server-command.md)
[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
[sottocomando: set-server](subcommand-set-server.md)
[ Sottocomando: Start-Server](subcommand-start-server.md)1[sottocomando: stop-server](subcommand-stop-server.md)3[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
