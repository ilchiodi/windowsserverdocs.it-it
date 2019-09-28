---
title: bitsadmin getdisplayname
description: 'Argomento dei comandi di Windows per **BITSAdmin GetDisplayName** : Recupera il nome visualizzato del processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 229bd245f9e810fc6aeb856bbfba253b9ab8a9f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381634"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



Recupera il nome visualizzato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperato il nome visualizzato per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)