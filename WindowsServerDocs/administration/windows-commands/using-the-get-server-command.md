---
title: Get-server
description: Argomento di riferimento per Get-server, che recupera le informazioni dal server di servizi di distribuzione Windows specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a760371797af8eb95da386a3a5b9dbb0dcf7ba3c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719733"
---
# <a name="get-server"></a>Get-server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni dal server di servizi di distribuzione Windows specificato.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ Mostra: {configurazione & #124; Immagini & #124; All}|Specifica il tipo di informazioni da restituire.<p>-   **Config** restituisce le informazioni di configurazione.<br />-   **Images** restituisce informazioni sui gruppi di immagini, le immagini di avvio e le immagini di installazione.<br />-   Restituisce **tutte** le informazioni di configurazione e le informazioni sull'immagine.|
|[/detailed]|È possibile usare questa opzione con **/Show: images** o **/Show: All** per indicare che devono essere restituiti tutti i metadati delle immagini da ogni immagine. Se non si usa l'opzione **/detailed** , il comportamento predefinito consiste nel restituire il nome dell'immagine, la descrizione e il nome del file.|
## <a name="examples"></a>Esempi
Per visualizzare le informazioni sul server, digitare:
```
wdsutil /Get-Server /Show:Config
```
Per visualizzare informazioni dettagliate sul server, digitare:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
di sintassi della riga di comando utilizzando il
[comando Disable-server](using-the-disable-server-command.md)
[utilizzando il comando Enable-Server](using-the-enable-server-command.md)[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
sottocomando[: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: stop-server](subcommand-stop-server.md)
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
