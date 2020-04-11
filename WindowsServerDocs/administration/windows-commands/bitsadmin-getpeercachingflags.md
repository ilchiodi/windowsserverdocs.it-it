---
title: getpeercachingflags Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin getpeercachingflags**, che consente di recuperare i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se BITS è in grado di scaricare il contenuto per il processo da peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59ff3e3a5802c6023d85129c82f19cd7ee625d2f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123128"
---
# <a name="bitsadmin-getpeercachingflags"></a>getpeercachingflags Bitsadmin

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se BITS è in grado di scaricare il contenuto per il processo da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperati i flag per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getpeercachingflags myJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)