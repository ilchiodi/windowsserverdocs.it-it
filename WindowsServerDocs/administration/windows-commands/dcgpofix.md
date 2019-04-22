---
title: dcgpofix
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91fceb429ca00b1b3d9d36d01f5e97cfd464ccb9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825162"
---
# <a name="dcgpofix"></a>dcgpofix



Ricrea il valore predefinito dei criteri di gruppo (GPO) per un dominio. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/IgnoreSchema|Ignora la versione di mc schema Active Directory®</br>Quando si esegue questo comando. In caso contrario, il comando funziona solo nella stessa versione di schema come la versione di Windows in cui è stato fornito il comando.|
|/target {Domain | DC | Entrambi}|Specifica l'oggetto Criteri di gruppo da ripristinare. È possibile ripristinare il criterio dominio predefinito, il controller di dominio predefinito o entrambi.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **dcgpofix** comando è disponibile in Windows Server 2008 R2 e Windows Server 2008, tranne nelle installazioni Server Core.
-   Sebbene la Console Gestione criteri di gruppo (GPMC) viene distribuita con Windows Server 2008 R2 e Windows Server 2008, è necessario installare Gestione criteri di gruppo come funzionalità tramite Server Manager.

## <a name="BKMK_Examples"></a>Esempi

Ripristinare il criterio dominio predefinito per lo stato originale. Si perderanno tutte le modifiche apportate a questo oggetto Criteri di gruppo. Come procedura consigliata, è necessario configurare l'oggetto criterio dominio predefinito solo per gestire le impostazioni di criteri di Account, criteri Password, criterio di blocco Account e criterio Kerberos. In questo esempio, si ignora la versione dello schema di Active Directory in modo che il **dcgpofix** comando non è limitato allo stesso schema della versione di Windows in cui è stato fornito il comando.
```
dcgpofix /ignoreschema /target:Domain
```
Ripristinare il controller criterio dominio predefinito per lo stato originale. Si perderanno tutte le modifiche apportate a questo oggetto Criteri di gruppo. Come procedura consigliata, è necessario configurare il controller criterio dominio predefinito solo per impostare i diritti utente e criteri di controllo. In questo esempio, si ignora la versione dello schema di Active Directory in modo che il **dcgpofix** comando non è limitato allo stesso schema della versione di Windows in cui è stato fornito il comando.
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>Altri riferimenti

-   [TechCenter di criteri di gruppo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)