---
title: Simulare il ripristino
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09b626939c13d4e38a983435b45d8c47ee2b93a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817252"
---
# <a name="simulate-restore"></a>Simulare il ripristino



Testa un coinvolgimento dell'agente di scrittura nelle sessioni di ripristino nel computer senza emettere **PreRestore** oppure **PostRestore** per gli autori di eventi.

## <a name="syntax"></a>Sintassi

```
simulate restore
```

## <a name="remarks"></a>Note

-   **Simulare il ripristino** viene usato per testare il ripristino con i writer può essere esito positivo o meno.
-   Prima di poter usare **simulare restore**, è necessario caricare un file di metadati DiskShadow tramite il **caricare i metadati** comando. Ciò consente di caricare il writer selezionato e i componenti per il ripristino.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)