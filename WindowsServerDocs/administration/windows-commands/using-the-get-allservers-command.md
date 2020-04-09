---
title: Get-AllServers
description: Windows Commands argomento per Get-AllServers, che recupera informazioni su tutti i server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b400d5a2be69e8e89a05b233cc2e8f29bec848f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831214"
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare informazioni su tutti i server, digitare:
```
WDSUTIL /Get-AllServers /Show:Config
```
Per visualizzare informazioni dettagliate su tutti i server, digitare:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)