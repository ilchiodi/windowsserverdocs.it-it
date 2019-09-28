---
title: bitsadmin getproxybypasslist
description: "Argomento dei comandi di Windows per **BITSAdmin getproxybypasslist** : Recupera l'elenco di bypass proxy per il processo specificato."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87cc131402707eac40329750e98218ec52083b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381425"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera l'elenco di bypass del proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

L'elenco di bypass contiene i nomi host o gli indirizzi IP, o entrambi, che non devono essere instradati tramite un proxy. L'elenco può contenere "\<local >" per fare riferimento a tutti i server nella stessa LAN. L'elenco può essere un punto e virgola o delimitato da spazi.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperato l'elenco di bypass del proxy per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)