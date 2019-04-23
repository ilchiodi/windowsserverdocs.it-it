---
title: bitsadmin getproxybypasslist
description: Argomento i comandi di Windows per **bitsadmin getproxybypasslist** -recupera l'elenco proxy da ignorare per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 020b8fc0019eb103a0e469258be8705b80dd45de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854112"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera l'elenco proxy da ignorare per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Elenco di esclusione contiene i nomi host o indirizzi IP o entrambi, che non sono devono essere instradati attraverso un proxy. L'elenco può contenere "\<locale >" per fare riferimento a tutti i server alla stessa rete LAN. L'elenco può essere un punto e virgola o delimitati da spazi.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera l'elenco proxy da ignorare per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)