---
title: Inattivo
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9ce91b6a024c165e3aa63148b9ad6dfcc4db7a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375368"
---
# <a name="inactive"></a>Inattivo



Nei dischi MBR (master boot record) di base, contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo.

## <a name="syntax"></a>Sintassi

```
inactive
```

## <a name="remarks"></a>Note

> [!CAUTION]
> Il computer potrebbe non avviarsi senza una partizione attiva. Non si contrassegna come inattiva una partizione di sistema o di avvio a meno che non si è utenti esperti con una conoscenza approfondita della famiglia di sistemi operativi Windows.</br>> Se non è possibile avviare il computer dopo aver contrassegnato la partizione di sistema o di avvio come inattiva, inserire il CD Installazione di Windows nell'unità CD-ROM, riavviare il computer e quindi ripristinare la partizione usando i comandi **FIXMBR** e **Fixboot** nel Console di ripristino.
> -   Dopo aver contrassegnato la partizione di sistema o la partizione di avvio come inattiva, il computer viene avviato dall'opzione successiva specificata nel BIOS, ad esempio l'unità CD-ROM o un ambiente PXE (pre-boot eXecution Environment).
> -   Per eseguire questa operazione, è necessario selezionare una partizione di sistema o di avvio attiva. Utilizzare il **Selezionare partizione** comando per selezionare la partizione attiva e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

```
inactive
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

