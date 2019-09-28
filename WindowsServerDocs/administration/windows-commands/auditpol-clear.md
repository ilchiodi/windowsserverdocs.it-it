---
title: auditpol clear
description: Argomento dei comandi di Windows per **auditpol clear** -Elimina i criteri di controllo per utente per tutti gli utenti, Reimposta (Disabilita) i criteri di controllo del sistema per tutte le sottocategorie e imposta tutte le opzioni di controllo su disabilitato.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fd2cce2b860ee41725b698dcd36ca38b2c4c6a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382420"
---
# <a name="auditpol-clear"></a>auditpol clear

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i criteri di controllo per utente per tutti gli utenti, Reimposta (Disabilita) i criteri di controllo del sistema per tutte le sottocategorie e imposta tutte le opzioni di controllo su disabilitato.

## <a name="syntax"></a>Sintassi
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parametri

| Parametro |                                   Descrizione                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | Elimina la richiesta di conferma della cancellazione di tutte le impostazioni dei criteri di controllo. |
|    /?     |                       Visualizza la guida al prompt dei comandi.                       |

## <a name="remarks"></a>Note
per le operazioni di cancellazione per i criteri per utente e i criteri di sistema, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire l'operazione di cancellazione utilizzando il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di cancellazione.
## <a name="BKMK_examples"></a>Esempi
Per eliminare i criteri di controllo per utente per tutti gli utenti, reimpostare (disabilitare) i criteri di controllo del sistema per tutte le sottocategorie e impostare tutte le impostazioni dei criteri di controllo su disabilitato, al prompt di conferma, digitare:
```
auditpol /clear
```
Per eliminare i criteri di controllo per utente per tutti gli utenti, reimpostare le impostazioni dei criteri di controllo del sistema per tutte le sottocategorie e impostare tutte le impostazioni dei criteri di controllo su disabilitato, senza una richiesta di conferma, digitare:
```
auditpol /clear /y
```
> [!NOTE]
> L'esempio precedente è utile quando si usa uno script per eseguire questa operazione.
> #### <a name="additional-references"></a>Riferimenti aggiuntivi
> [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
