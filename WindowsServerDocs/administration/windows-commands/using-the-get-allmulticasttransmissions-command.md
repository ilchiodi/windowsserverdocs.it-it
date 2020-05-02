---
title: Get-AllMulticastTransmissions
description: Argomento di riferimento per Get-AllMulticastTransmissions, che consente di visualizzare informazioni su tutte le trasmissioni multicast in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5303618d1021a0c585a2bd6f958f73e145028a09
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720011"
---
# <a name="get-allmulticasttransmissions"></a>Get-AllMulticastTransmissions

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su tutte le trasmissioni multicast in un server.

## <a name="syntax"></a>Sintassi
per Windows Server 2008:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
per Windows Server 2008 R2:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
### <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                                                                                                   Spiegazione                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |                                                                                                                                                                                 Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                                                                                                  |
|         [/Show]         | **Windows Server 2008**<p>/Show:Clients - Visualizza informazioni sui computer client connessi a trasmissioni multicast.<p>**Windows Server 2008 R2**<p>Mostra: {Avvio & #124; Installa & #124; All} - il tipo di immagine da restituire.                                **Avvio** restituisce solo le trasmissioni di immagini di avvio.                                  **Installare** restituisce installare solo le trasmissioni di immagine. **Tutti** restituisce entrambi i tipi di immagine. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details: client     |                                                                                                                                                                                              Supportato solo per Windows Server 2008 R2. Se presente, verranno visualizzati ai client connessi alla trasmissione.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Esclude tutte le trasmissioni disattivate dall'elenco.                                                                                                                                                                                                                                               |

## <a name="examples"></a>Esempi
Per visualizzare informazioni su tutte le trasmissioni, digitare:
- Windows Server 2008:`wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` per visualizzare informazioni su tutte le trasmissioni tranne le trasmissioni disattivate, digitare:
- Windows Server 2008:`wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2:`wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Chiave](command-line-syntax-key.md)
  di sintassi della riga di comando usando il comando[Get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
  [usando il comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
  [usando il comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
  [sottocomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
