---
title: rundll32
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 391607f6e744fe88a60cb556067cf2699eee25f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835434"
---
# <a name="rundll32"></a>rundll32



Carica ed esegue le librerie a collegamento dinamico (dll) a 32 bit. Non sono disponibili impostazioni configurabili per rundll32. Sono disponibili informazioni della Guida per una DLL specifica eseguita con il comando **rundll32** .

È necessario eseguire il comando **rundll32** da un prompt dei comandi con privilegi elevati. Per aprire un prompt dei comandi con privilegi elevati, fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **prompt dei comandi**e scegliere **Esegui come amministratore**

## <a name="syntax"></a>Sintassi

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Commands

|Parametro|Descrizione|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Visualizza l'interfaccia utente della stampante|

## <a name="remarks"></a>Note

Rundll32 può chiamare solo le funzioni da una DLL scritta in modo esplicito per essere chiamate da rundll32.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
