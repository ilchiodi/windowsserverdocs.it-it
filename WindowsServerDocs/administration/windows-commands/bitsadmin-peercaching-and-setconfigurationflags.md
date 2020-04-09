---
title: Bitsadmin peer caching e setconfigurationflags
description: Argomento dei comandi di Windows per **BITSAdmin peer caching** e **setconfigurationflags**, che imposta i flag di configurazione che determinano se il computer è in grado di fornire contenuti ai peer e se può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebaa09da2d4594d2762e67dc5884dd15cf4d1da8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850134"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>Bitsadmin peer caching e setconfigurationflags

Imposta i flag di configurazione che determinano se il computer può gestire il contenuto ai peer e se può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| value | Unsigned Integer con l'interpretazione seguente per i bit nella rappresentazione binaria:<ul><li> Per consentire il download dei dati del processo da un peer, impostare il bit meno significativo.</li><li>Per consentire ai dati del processo di essere serviti ai peer, impostare il secondo bit da destra.</li></ul>|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono specificati i dati del processo da scaricare dai peer per il processo denominato *myDownloadJob*.

```
C:\> bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)