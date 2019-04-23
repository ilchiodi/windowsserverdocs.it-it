---
title: bitsadmin getaclflags
description: Argomento i comandi di Windows per **bitsadmin getaclflags** -recupera i contrassegni propagazioni dell'elenco di accesso controllo.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 185445a97168344f910abc0e644718296de2c712
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861452"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera i contrassegni propagazioni dell'elenco (ACL) di accesso controllo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Visualizza uno o più dei valori di flag seguenti:
-   O: Copiare informazioni sul proprietario con file.
-   G: Copiare le informazioni sui gruppi con file.
-   D: Copiare informazioni DACL con file.
-   S: Copiare informazioni SACL con file.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera i flag di propagazione elenco di controllo di accesso per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)