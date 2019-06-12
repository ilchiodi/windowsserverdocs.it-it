---
title: getsecurityflags Bitsadmin
description: 'Argomento i comandi di Windows per **bitsadmin getsecurityflags** : segnala i flag di sicurezza HTTP per il reindirizzamento degli URL e controlli eseguiti sul certificato del server durante il trasferimento.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e1db167b12d47afccb8842da617f1e9fe72acff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434967"
---
# <a name="bitsadmin-getsecurityflags"></a>getsecurityflags Bitsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Flag di protezione report HTTP per il reindirizzamento degli URL e i controlli eseguiti sul certificato del server durante il trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi
Nell'esempio seguente recupera i flag securitly da un processo denominato *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


