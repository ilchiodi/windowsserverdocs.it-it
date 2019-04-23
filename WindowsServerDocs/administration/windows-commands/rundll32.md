---
title: rundll32
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b1f288d21a1dcac25ecc00f685ea179d8a6542f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835032"
---
# <a name="rundll32"></a>rundll32



Carica ed esegue librerie a collegamento dinamico (DLL) a 32 bit. Sono presenti impostazioni configurabili per Rundll32. Le informazioni della Guida viene fornite per una DLL specifica si esegue con il **rundll32** comando.

È necessario eseguire la **rundll32** comando al prompt dei comandi con privilegi elevati. Per aprire un prompt dei comandi con privilegi elevati, fare clic su **avviare**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.

## <a name="syntax"></a>Sintassi

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Comandi

|Parametro|Descrizione|
|---------|-----------|
|[Rundll32 printui.dll,PrintUIEntry](rundll32-printui.md)|Visualizza l'interfaccia utente stampante|

## <a name="remarks"></a>Note

Rundll32 può chiamare solo funzioni da una DLL che vengono scritti in modo esplicito che verrà chiamata da Rundll32. Per altre informazioni su Rundll32 requisiti, vedere [articolo 164787](https://go.microsoft.com/fwlink/?LinkID=165773) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkID=165773).

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)