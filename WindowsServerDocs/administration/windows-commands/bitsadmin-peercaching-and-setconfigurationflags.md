---
title: setconfigurationflags e bitsadmin Caching
description: Argomento i comandi di Windows per **bitsadmin caching e setconfigurationflags** -imposta i flag di configurazione che determinano se il computer può gestire il contenuto ai peer e può scaricare contenuto da peer.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22408d4aab7f5ea374511bc16751d911a84644f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813332"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>setconfigurationflags e bitsadmin Caching



Imposta i flag di configurazione che determinano se il computer può gestire il contenuto ai peer e può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Value|Il valore è un intero senza segno con l'interpretazione seguente per i bit nella rappresentazione binaria:</br>-Consente di dati del processo da scaricare da un peer: Impostare il bit meno significativo</br>-Consente di dati del processo di essere servite ai peer: Impostare il bit 2 da destra.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente specifica i dati del processo da scaricare dal peer per il processo denominato *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)