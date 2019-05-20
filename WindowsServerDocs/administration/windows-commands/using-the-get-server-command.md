---
title: Utilizzando il comando get-Server
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8bf0fc6e31bd8d0079933f1d7c529c4fe96f42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870982"
---
# <a name="using-the-get-server-command"></a>Utilizzando il comando get-Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni dal server Servizi di distribuzione Windows specificato.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ Mostra: {configurazione &#124; Immagini &#124; All}|Specifica il tipo di informazioni da restituire.<br /><br />-   **Configurazione** restituisce le informazioni di configurazione.<br />-   **Le immagini** restituisce informazioni sui gruppi di immagini, immagini di avvio e le immagini di installazione.<br />-   **Tutti i** restituisce le informazioni di configurazione e le informazioni sull'immagine.|
|[/ dettagliate]|È possibile usare questa opzione con **/Show:Images** oppure **/Show:All** per indicare che tutti i metadati delle immagini da ogni immagine devono essere restituito. Se il **/ dettagliate** opzione non viene utilizzato, il comportamento predefinito è per restituire il nome dell'immagine, descrizione e nome file.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni sul server, digitare:
```
wdsutil /Get-Server /Show:Config
```
Per visualizzare informazioni dettagliate sul server, digitare:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando enable-Server](using-the-enable-server-command.md)
[tramite il Comando Initialize-Server](using-the-initialize-server-command.md)
[sottocomando: set-Server](subcommand-set-server.md)
[sottocomando: start-Server](subcommand-start-server.md) 
 [ Sottocomando: stop-Server](subcommand-stop-server.md)
[il Server uninitialize opzione](the-uninitialize-server-option.md)
