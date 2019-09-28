---
title: break
description: "Argomento dei comandi di Windows per **break_1** -imposta o cancella il controllo esteso CTRL + C nei sistemi MS-DOS. Se utilizzata senza parametri, **Interrompi** consente di visualizzare l'impostazione corrente. "
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73afdac29efbfd9efec88d297cf4185ca1b92d62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380493"
---
# <a name="break"></a>break



Imposta o cancella il controllo esteso di CTRL + C nei sistemi MS-DOS. Se utilizzata senza parametri, **Interrompi** consente di visualizzare l'impostazione corrente.

> [!NOTE]
> Questo comando non è più in uso. È stato incluso al solo scopo di mantenere la compatibilità con file MS-DOS esistenti, ma non ha produce alcun effetto sulla riga di comando perché la funzionalità è automatica.

## <a name="syntax"></a>Sintassi

```
break=[on|off]
```

## <a name="remarks"></a>Note

Se le estensioni dei comandi sono abilitate e in esecuzione nella piattaforma Windows, l'inserimento del comando **Interrompi** in un file batch passa a un punto di interruzione hardcoded se viene sottoposto a debug da un debugger.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)