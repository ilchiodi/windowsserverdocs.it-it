---
title: Get-AllServers
description: Argomento di riferimento per Get-AllServers, che consente di recuperare informazioni su tutti i server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b623b5e95e2a57147b7d9d191d42556191dd8e4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719977"
---
# <a name="get-allservers"></a>Get-AllServers

Recupera informazioni su tutti i server di servizi di distribuzione Windows.

> [!NOTE]
> Questo comando può richiedere una quantità estesa di tempo se sono presenti più server di servizi di distribuzione Windows nell'ambiente in uso o se la connessione di rete i server di collegamento è lenta.

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

### <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                                 Descrizione                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show: {config |                                                                                                                    Immagini                                                                                                                    |
|  [/ Dettagliate]  | Quando utilizzato in combinazione con il **/Show:Images** o **/Show:All**, restituisce tutti i metadati di ogni immagine di immagine. Se il **/dettagliate** opzione non è specificata, il comportamento predefinito è per restituire il nome dell'immagine, descrizione e nome file. |
| [/Forest: {Sì |                                                                                                                     No}]                                                                                                                     |

## <a name="examples"></a>Esempi

Per visualizzare informazioni su tutti i server, digitare:
```
WDSUTIL /Get-AllServers /Show:Config
```
Per visualizzare informazioni dettagliate su tutti i server, digitare:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)