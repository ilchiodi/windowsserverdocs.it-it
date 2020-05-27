---
title: inactive
description: Argomento di riferimento per il comando inattivo, che contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo sui dischi MBR (master boot record) di base.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9861a54c284002e53b0a8fc354aa883d80fff0e7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818441"
---
# <a name="inactive"></a>inactive

Contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo sui dischi MBR (master boot record) di base.

Per eseguire questa operazione, è necessario selezionare una partizione di sistema o di avvio attiva. Usare il comando [select partition comando](select-partition.md) per selezionare la partizione attiva e spostare lo stato attivo su di essa.

> [!CAUTION]
> Il computer potrebbe non avviarsi senza una partizione attiva. Non contrassegnare una partizione di sistema o di avvio come inattiva a meno che non si sia un utente esperto con una conoscenza approfondita della famiglia di sistemi operativi Windows.<p>Se non è possibile avviare il computer dopo aver contrassegnato come inattivo la partizione di sistema o di avvio, inserire il CD Installazione di Windows nell'unità CD-ROM, riavviare il computer e quindi ripristinare la partizione usando i comandi **FIXMBR** e **Fixboot** nella console di ripristino di sistema.
>
> Dopo aver contrassegnato la partizione di sistema o la partizione di avvio come inattiva, il computer viene avviato dall'opzione successiva specificata nel BIOS, ad esempio l'unità CD-ROM o un ambiente PXE (pre-boot eXecution Environment).

## <a name="syntax"></a>Sintassi

```
inactive
```

### <a name="examples"></a>Esempi

```
inactive
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando select partition](select-partition.md)

- [Risoluzione dei problemi avanzati per i problemi di avvio di Windows](https://docs.microsoft.com/windows/client-management/advanced-troubleshooting-boot-problems)
