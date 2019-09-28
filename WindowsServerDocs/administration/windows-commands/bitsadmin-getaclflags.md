---
title: bitsadmin getaclflags
description: "Argomento dei comandi di Windows per **BITSAdmin GETACLFLAGS** : recupera i flag di propagazione dell'elenco di controllo di accesso."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad98cd742161ae06be5cba7acde7b810eaf199d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381794"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera i flag di propagazione dell'elenco di controllo di accesso (ACL).

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Visualizza uno o più dei seguenti valori di flag:
-   O Copiare le informazioni sul proprietario con il file.
-   G: Copia le informazioni sul gruppo con il file.
-   D: Copiare le informazioni DACL con file.
-   S: Copiare le informazioni SACL con il file.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono recuperati i flag di propagazione dell'elenco di controllo di accesso per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)