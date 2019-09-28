---
title: bitsadmin resume
description: 'Argomento dei comandi di Windows per **BITSAdmin Resume** : attiva un processo nuovo o sospeso nella coda di trasferimento.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1393e959980b72de09c546ced763a506d334b56c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380773"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



Attiva un processo nuovo o sospeso nella coda di trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente riprende il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)