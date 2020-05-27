---
title: Manage-bde TPM
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c93a80a076b34ad4e7340387b098042b58103ae8
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820757"
---
# <a name="manage-bde-tpm"></a>Manage-bde: TPM

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
> [!IMPORTANT]
> Questo comando non è supportato per l'utilizzo in computer che eseguono Windows 8, Windows Server 2012 o versioni successive. Per tali computer, è possibile utilizzare il [Gestione TPM cmdlet Windows PowerShell per](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Se si utilizza questo comando in un computer che esegue Windows 7 o Windows Server 2008, è comunque possibile configurare la Trusted Platform Module del computer (TPM) utilizzando questo comando.
> ## <a name="syntax"></a>Sintassi
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> #### <a name="parameters"></a>Parametri
>
> |    Parametro    |                                                                              Descrizione                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     attivazione:     |              Abilita e attiva il TPM, consentendo la password del proprietario del TPM da impostare. È inoltre possibile utilizzare **-t** come una versione abbreviata di questo comando.              |
> | -takeownership  |                      Acquisisce la proprietà del TPM impostando una password del proprietario. È inoltre possibile utilizzare **-o** come una versione abbreviata di questo comando.                       |
> | <OwnerPassword> |                                                      Rappresenta la password del proprietario specificato per il TPM.                                                       |
> |  -computername  | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
> |     <Name>      |    Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.     |
> |    -? o /?     |                                                               Visualizza una breve guida al prompt dei comandi.                                                               |
> |   -Help o-h   |                                                             Visualizza la guida completa al prompt dei comandi.                                                              |
>
> ## <a name="examples"></a>Esempi
> Per illustrare l'uso del comando **-TPM** per attivare il TPM.
> ```
> manage-bde  tpm -turnon
> ```
> Per illustrare l'uso del comando **TPM** per assumere la proprietà del TPM e impostare la password del proprietario su 0wnerP@ss .
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Riferimenti aggiuntivi
> - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
