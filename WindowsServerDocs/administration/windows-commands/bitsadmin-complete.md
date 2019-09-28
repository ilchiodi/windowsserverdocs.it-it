---
title: bitsadmin complete
description: 'Argomento dei comandi di Windows per **BITSAdmin completato** : completa il processo. I file scaricati non sono disponibili finché non si usa questa opzione.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5a1dc5dbbf2d5b3207b5423f338e0caf4412599
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381824"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa il processo. I file scaricati non sono disponibili finché non si usa questa opzione. Usare questa opzione dopo che il processo è stato spostato sullo stato trasferito. In caso contrario, saranno disponibili solo i file che sono stati trasferiti correttamente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Quando lo stato del processo viene trasferito, BITS ha trasferito correttamente tutti i file nel processo. Tuttavia, i file non sono disponibili finché non si usa l'opzione **/completo** . Se più processi usano *myDownloadJob* come nome, è necessario sostituire *MYDOWNLOADJOB* con il GUID del processo per identificare in modo univoco il processo.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)