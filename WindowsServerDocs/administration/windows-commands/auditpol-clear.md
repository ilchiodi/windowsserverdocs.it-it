---
title: auditpol clear
description: Argomento di riferimento per il comando auditpol clear, che elimina i criteri di controllo per ogni utente per tutti gli utenti, Reimposta (Disabilita) i criteri di controllo del sistema per tutte le sottocategorie e imposta tutte le opzioni di controllo su disabilitato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3d4765907f1dd614f5d0a61585ea09069652ecb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719143"
---
# <a name="auditpol-clear"></a>auditpol clear

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i criteri di controllo per utente per tutti gli utenti, Reimposta (Disabilita) i criteri di controllo del sistema per tutte le sottocategorie e imposta tutte le opzioni di controllo su disabilitato.

Per eseguire operazioni di *cancellazione* sui criteri *per utente* e di *sistema* , è necessario disporre dell'autorizzazione di **controllo completo** o di **scrittura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di *cancellazione* se si dispone del diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire le operazioni di *cancellazione* generali.

## <a name="syntax"></a>Sintassi

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ----------- | --------------- |
| /y | Elimina la richiesta di conferma della cancellazione di tutte le impostazioni dei criteri di controllo. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comandi auditpol](auditpol.md)
