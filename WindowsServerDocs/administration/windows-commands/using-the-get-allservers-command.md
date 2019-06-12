---
title: Utilizzando il comando get-AllServers
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbccb834f9058f2c3cca097cdf998455f2a6892e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440489"
---
# <a name="using-the-get-allservers-command"></a>Utilizzando il comando get-AllServers



Recupera informazioni su tutti i server di servizi di distribuzione Windows.

> [!NOTE]
> Questo comando può richiedere una quantità estesa di tempo se sono presenti più server di servizi di distribuzione Windows nell'ambiente in uso o se la connessione di rete i server di collegamento è lenta.

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                                 Descrizione                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show:{Config |                                                                                                                    Immagini                                                                                                                    |
|  [/ Dettagliate]  | Quando utilizzato in combinazione con il **/Show:Images** o **/Show:All**, restituisce tutti i metadati di ogni immagine di immagine. Se il **/dettagliate** opzione non è specificata, il comportamento predefinito è per restituire il nome dell'immagine, descrizione e nome file. |
| [/ Insieme di strutture: {Sì |                                                                                                                     No}]                                                                                                                     |

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni su tutti i server, digitare:
```
WDSUTIL /Get-AllServers /Show:Config
```
Per visualizzare informazioni dettagliate su tutti i server, digitare:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)