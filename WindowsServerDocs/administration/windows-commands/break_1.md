---
title: break
description: Argomento dei comandi di Windows per break_1, che consente di impostare o deselezionare il controllo esteso CTRL + C nei sistemi MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 809a9321b8b4f8b2d201582f767da132076826d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848364"
---
# <a name="break"></a>break

Imposta o cancella il controllo esteso di CTRL + C nei sistemi MS-DOS. Se utilizzata senza parametri, **Interrompi** consente di visualizzare l'impostazione corrente.

> [!NOTE]
> Questo comando non è più in uso. Il comando è incluso solo per mantenere la compatibilità con file MS-DOS esistenti, ma non ha effetti a livello di riga di comando perché la funzionalità è automatica.

## <a name="syntax"></a>Sintassi

```
break=[on|off]
```

## <a name="remarks"></a>Note

Se le estensioni dei comandi sono abilitate e in esecuzione nella piattaforma Windows, l'inserimento del comando **Interrompi** in un file batch passa a un punto di interruzione hardcoded se viene sottoposto a debug da un debugger.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)