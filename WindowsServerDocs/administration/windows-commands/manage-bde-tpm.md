---
title: gestire-bde tpm
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2b8390d49257c3c6acd9ed4fd45773e5a65aac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840582"
---
# <a name="manage-bde-tpm"></a>manage-bde: tpm

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

> [!IMPORTANT]
> Questo comando non è supportato per l'utilizzo in computer che eseguono Windows 8, Windows Server 2012 o versioni successive. Per tali computer, è possibile utilizzare il [Gestione TPM cmdlet Windows PowerShell per](https://technet.microsoft.com/library/jj603116.aspx).
Se si utilizza questo comando nel computer che esegue Windows 7 o Windows Server 2008, è comunque possibile configurare modulo TPM del computer (Trusted Platform) con il seguente comando. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).
## <a name="syntax"></a>Sintassi
```
manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|attivazione:|Abilita e attiva il TPM, consentendo la password del proprietario del TPM da impostare. È inoltre possibile utilizzare **-t** come una versione abbreviata di questo comando.|
|-takeownership|Acquisisce la proprietà del TPM impostando una password del proprietario. È inoltre possibile utilizzare **-o** come una versione abbreviata di questo comando.|
|<OwnerPassword>|Rappresenta la password del proprietario specificato per il TPM.|
|-computername|Specifica che gestire bde.exe verrà usato per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Consente di visualizzare breve guida al prompt dei comandi.|
|-help o -h|Visualizza la Guida al prompt dei comandi completa.|
## <a name="BKMK_Examples"></a>Esempi
Nell'esempio seguente viene illustrato l'utilizzo di **- tpm** comando per attivare il TPM.
```
manage-bde  tpm -turnon
```
Nell'esempio seguente viene illustrato l'utilizzo di * * comando tpm * * per acquisire la proprietà del TPM e impostare la password del proprietario 0wnerP@ss.
```
manage-bde  tpm  takeownership 0wnerP@ss
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
