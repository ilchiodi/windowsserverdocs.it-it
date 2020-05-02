---
title: inactive
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f1c0e0cd5ebbf92638a221852bc3133116f4911
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724838"
---
# <a name="inactive"></a>inactive



Nei dischi MBR (master boot record) di base, contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo.

## <a name="syntax"></a>Sintassi

```
inactive
```

## <a name="remarks"></a>Osservazioni

> [!CAUTION]
> Il computer potrebbe non avviarsi senza una partizione attiva. Non si contrassegna come inattiva una partizione di sistema o di avvio a meno che non si è utenti esperti con una conoscenza approfondita della famiglia di sistemi operativi Windows.</br>> se non è possibile avviare il computer dopo aver contrassegnato la partizione di sistema o di avvio come inattiva, inserire il CD di Installazione di Windows nell'unità CD-ROM, riavviare il computer e quindi ripristinare la partizione utilizzando i comandi **FIXMBR** e **Fixboot** nella console di ripristino.
> -   Dopo aver contrassegnato la partizione di sistema o la partizione di avvio come inattiva, il computer viene avviato dall'opzione successiva specificata nel BIOS, ad esempio l'unità CD-ROM o un ambiente PXE (pre-boot eXecution Environment).
> -   Per eseguire questa operazione, è necessario selezionare una partizione di sistema o di avvio attiva. Utilizzare il **Selezionare partizione** comando per selezionare la partizione attiva e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

```
inactive
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

