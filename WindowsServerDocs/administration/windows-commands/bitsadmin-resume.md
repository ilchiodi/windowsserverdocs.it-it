---
title: bitsadmin resume
description: Argomento i comandi di Windows per **Riprendi bitsadmin** -attiva un processo nuovo o sospeso nella coda di trasferimento.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 76027ac927f8a9bb2558e3ce6d75e4f6692e56e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842032"
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)