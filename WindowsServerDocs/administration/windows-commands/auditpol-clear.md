---
title: auditpol chiaro
description: Argomento i comandi di Windows per **auditpol Cancella** -Elimina i criteri di controllo per utente per tutti gli utenti, il sistema di controllo dei criteri per tutte le sottocategorie e set di tutte le opzioni di controllo su disabilitato la reimpostazione (disattiva).
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 86b56386ba9bed2486cdf8cdbb4486fcec6c6265
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435156"
---
# <a name="auditpol-clear"></a>auditpol chiaro

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i criteri di controllo per utente per tutti gli utenti, il sistema di controllo dei criteri per tutte le sottocategorie e set di tutte le opzioni di controllo su disabilitato la reimpostazione (disattiva).

## <a name="syntax"></a>Sintassi
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parametri

| Parametro |                                   Descrizione                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | Elimina la richiesta di confermare se tutte le impostazioni di criteri di controllo devono essere cancellate. |
|    /?     |                       Visualizza la guida al prompt dei comandi.                       |

## <a name="remarks"></a>Note
per operazioni di cancellazione per il criterio per ogni utente e criteri di sistema, è necessario avere scrivere o dell'autorizzazione controllo completo su tale oggetto impostato nel descrittore di sicurezza. È anche possibile eseguire l'operazione di cancellazione dal desiderio di **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione di cancellazione.
## <a name="BKMK_examples"></a>Esempi
Per eliminare i criteri di controllo per utente per tutti gli utenti, reimpostazione (disattiva) i criteri di controllo di sistema per tutte le sottocategorie, quindi impostare le impostazioni di criteri di controllo su disabilitato, al prompt di conferma, digitare:
```
auditpol /clear
```
Per eliminare i criteri di controllo per utente per tutti gli utenti, reimpostare le impostazioni dei criteri di controllo di sistema per tutte le sottocategorie e impostare tutto il controllo impostazioni di criteri a disabilitato, senza una richiesta di conferma, digitare:
```
auditpol /clear /y
```
> [!NOTE]
> Nell'esempio precedente è utile quando si usa uno script per eseguire questa operazione.
> #### <a name="additional-references"></a>Riferimenti aggiuntivi
> [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
