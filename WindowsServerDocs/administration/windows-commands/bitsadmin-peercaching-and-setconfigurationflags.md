---
title: Bitsadmin peer caching e setconfigurationflags
description: 'Argomento dei comandi di Windows per **BITSAdmin peer caching e setconfigurationflags** : imposta i flag di configurazione che determinano se il computer può fornire contenuti ai peer e può scaricare il contenuto dai peer.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a65d54bcaa2bce26eb2b7c98250837ab09c7a423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381109"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>Bitsadmin peer caching e setconfigurationflags



Imposta i flag di configurazione che determinano se il computer può gestire il contenuto ai peer e può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Value|Il valore è un intero senza segno con l'interpretazione seguente per i bit nella rappresentazione binaria:</br>-Consentire il download dei dati del processo da un peer: Imposta il bit meno significativo</br>-Consenti ai dati del processo di essere serviti ai peer: Impostare il secondo bit da destra.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente specifica i dati del processo da scaricare dal peer per il processo denominato *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)