---
title: Bitsadmin peer caching e getconfigurationflags
description: Argomento dei comandi di Windows per **BITSAdmin peer caching e getconfigurationflags** -ottiene i flag di configurazione che determinano se il computer serve contenuto ai peer e può scaricare il contenuto dai peer.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94c7eb1a115fe9152b149b8cf65765b179080cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381095"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>Bitsadmin peer caching e getconfigurationflags



Ottiene i flag di configurazione che determinano se il computer fornisce contenuto per peer e può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente ottiene i flag di configurazione per il processo denominato *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)