---
title: inattivo
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c8ded732d984830c7892720f75938979f1abb67
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438160"
---
# <a name="inactive"></a>inattivo



Nei dischi MBR (record) di avvio principale base, contrassegna la partizione di sistema o una partizione di avvio con lo stato attivo come inattivo.

## <a name="syntax"></a>Sintassi

```
inactive
```

## <a name="remarks"></a>Note

> [!CAUTION]
> Il computer potrebbe non avviarsi senza una partizione attiva. Non si contrassegna come inattiva una partizione di sistema o di avvio a meno che non si è utenti esperti con una conoscenza approfondita della famiglia di sistemi operativi Windows.</br>> Se non si riesce ad avviare il computer dopo aver contrassegnato la partizione di sistema o di avvio come inattiva, inserire il CD di installazione di Windows nell'unità CD-ROM, riavviare il computer e quindi ripristinare la partizione utilizzando il **viene** e**corso** comandi nella Console di ripristino.
> -   Dopo aver contrassegnato la partizione di sistema o una partizione di avvio come inattivo, il computer viene avviato dall'opzione successiva specificata nel BIOS, ad esempio l'unità CD-ROM o un Pre-Boot eXecution Environment (PXE).
> -   Per eseguire questa operazione, è necessario selezionare una partizione di sistema o di avvio attiva. Utilizzare il **Selezionare partizione** comando per selezionare la partizione attiva e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

```
inactive
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

