---
title: Bitsadmin peer caching e getconfigurationflags
description: Argomento dei comandi di Windows per **BITSAdmin peer caching** e **getconfigurationflags**, che ottiene i flag di configurazione che determinano se il computer fornisce contenuto ai peer e se può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be8d6a719d63c8e9c6250320560b6ce21274c680
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850174"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>Bitsadmin peer caching e getconfigurationflags

Ottiene i flag di configurazione che determinano se il computer fornisce contenuto ai peer e se può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono ottenuti i flag di configurazione per il processo denominato *myDownloadJob*.

```
C:\> bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)