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
ms.openlocfilehash: 080836e406b329cf8c15f95ef6afc99973bb3e4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853092"
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

|Parametro|Descrizione|
|---------|-----------|
|/Show:{Config | Immagini | all}|Specifica il tipo di informazioni da restituire.</br>-   **Configurazione** restituisce informazioni di configurazione server.</br>-   **Le immagini** restituisce informazioni sui gruppi di immagini, immagini di avvio e le immagini di installazione nel server.</br>-   **Tutti i** restituisce le informazioni di configurazione e l'immagine server.|
|[/ Dettagliate]|Quando utilizzato in combinazione con il **/Show:Images** o **/Show:All**, restituisce tutti i metadati di ogni immagine di immagine. Se il **/dettagliate** opzione non è specificata, il comportamento predefinito è per restituire il nome dell'immagine, descrizione e nome file.|
|[/ Insieme di strutture: {Sì | No}]|Specifica se restituire le informazioni per l'intera foresta o il dominio locale. Se non viene specificato un valore per questa opzione, per restituire i server nel dominio locale è il comportamento predefinito.|

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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)