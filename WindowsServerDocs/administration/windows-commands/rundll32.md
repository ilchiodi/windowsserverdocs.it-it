---
title: rundll32
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0639206b26ea58c4ec8473c0a736fda3c435021
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722241"
---
# <a name="rundll32"></a>rundll32



Carica ed esegue le librerie a collegamento dinamico (dll) a 32 bit. Non sono disponibili impostazioni configurabili per rundll32. Sono disponibili informazioni della Guida per una DLL specifica eseguita con il comando **rundll32** .

È necessario eseguire il comando **rundll32** da un prompt dei comandi con privilegi elevati. Per aprire un prompt dei comandi con privilegi elevati, fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **prompt dei comandi**e scegliere **Esegui come amministratore**

## <a name="syntax"></a>Sintassi

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Comandi:

|Parametro|Descrizione|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Visualizza l'interfaccia utente della stampante|

## <a name="remarks"></a>Osservazioni

Rundll32 può chiamare solo le funzioni da una DLL scritta in modo esplicito per essere chiamate da rundll32.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
