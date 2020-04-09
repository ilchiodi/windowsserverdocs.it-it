---
title: Get-server
description: Windows Commands argomento per Get-server, che recupera le informazioni dal server di servizi di distribuzione Windows specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65903dc89730eb9d1da23be31ecc1909daece9c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830844"
---
# <a name="get-server"></a>Get-server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni dal server di servizi di distribuzione Windows specificato.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ Mostra: {configurazione & #124; Immagini & #124; All}|Specifica il tipo di informazioni da restituire.<p>-   **config** restituisce le informazioni di configurazione.<br />-   **Immagini** restituisce informazioni sui gruppi di immagini, le immagini di avvio e le immagini di installazione.<br />-   restituisce **tutte** le informazioni di configurazione e le informazioni sull'immagine.|
|[/detailed]|È possibile usare questa opzione con **/Show: images** o **/Show: All** per indicare che devono essere restituiti tutti i metadati delle immagini da ogni immagine. Se non si usa l'opzione **/detailed** , il comportamento predefinito consiste nel restituire il nome dell'immagine, la descrizione e il nome del file.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare le informazioni sul server, digitare:
```
wdsutil /Get-Server /Show:Config
```
Per visualizzare informazioni dettagliate sul server, digitare:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[uso del comando disable-server](using-the-disable-server-command.md)
[uso del comando Enable-server](using-the-enable-server-command.md)
[tramite il comando Initialize-Server](using-the-initialize-server-command.md)
[sottocomando: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: Stop-](subcommand-stop-server.md) server
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
