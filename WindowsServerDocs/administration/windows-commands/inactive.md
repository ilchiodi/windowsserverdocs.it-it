---
title: inactive
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38a4f731bef9515a387b0343eaf4cb06142e6620
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842124"
---
# <a name="inactive"></a>inactive



Nei dischi MBR (master boot record) di base, contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo.

## <a name="syntax"></a>Sintassi

```
inactive
```

## <a name="remarks"></a>Note

> [!CAUTION]
> Il computer potrebbe non avviarsi senza una partizione attiva. Non si contrassegna come inattiva una partizione di sistema o di avvio a meno che non si è utenti esperti con una conoscenza approfondita della famiglia di sistemi operativi Windows.</br>> Se non è possibile avviare il computer dopo aver contrassegnato la partizione di sistema o di avvio come inattiva, inserire il CD di Installazione di Windows nell'unità CD-ROM, riavviare il computer e quindi ripristinare la partizione utilizzando i comandi **FIXMBR** e **Fixboot** nella console di ripristino.
> -   Dopo aver contrassegnato la partizione di sistema o la partizione di avvio come inattiva, il computer viene avviato dall'opzione successiva specificata nel BIOS, ad esempio l'unità CD-ROM o un ambiente PXE (pre-boot eXecution Environment).
> -   Per eseguire questa operazione, è necessario selezionare una partizione di sistema o di avvio attiva. Utilizzare il **Selezionare partizione** comando per selezionare la partizione attiva e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

```
inactive
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

