---
title: lpq
description: Argomento di riferimento per il comando lpq, che visualizza lo stato di una coda di stampa in un computer in cui è in esecuzione la funzione LPD (Line Printer Daemon).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ecce9b1b4255e5e769fe76b0f753226d61fa916
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222723"
---
# <a name="lpq"></a>lpq

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare lo stato di una coda di stampa in un computer che esegue LPD (Line Printer Daemon).

## <a name="syntax"></a>Sintassi

```
lpq -S <servername> -P <printername> [-l]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -S`<servername>` | Specifica, tramite nome o indirizzo IP, il computer o dispositivo che ospita la coda di stampa LPD con cui si desidera visualizzare lo stato di condivisione della stampante. Questo parametro è obbligatorio e deve essere in maiuscolo. |
| -P`<Printername>` | Specifica (nome) della stampante per la coda di stampa con uno stato che si desidera visualizzare. Questo parametro è obbligatorio e deve essere in maiuscolo. |
| -l | Specifica che si desidera visualizzare i dettagli relativi allo stato della coda di stampa. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per visualizzare lo stato della coda della stampante *stampa laserprinter1* in un host LPD in *10.0.0.45*, digitare:

```
lpq -S 10.0.0.45 -P Laserprinter1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Informazioni di riferimento sui comandi di stampa](print-command-reference.md)
