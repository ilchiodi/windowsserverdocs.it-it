---
title: auditpol clear
description: Argomento dei comandi di Windows per **auditpol clear**, che elimina i criteri di controllo per ogni utente per tutti gli utenti, Reimposta (Disabilita) i criteri di controllo del sistema per tutte le sottocategorie e imposta tutte le opzioni di controllo su disabilitato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 971f4ba54d787be29cb9e7d710f556c50c69a8dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851204"
---
# <a name="auditpol-clear"></a>auditpol clear

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i criteri di controllo per utente per tutti gli utenti, Reimposta (Disabilita) i criteri di controllo del sistema per tutte le sottocategorie e imposta tutte le opzioni di controllo su disabilitato.

## <a name="syntax"></a>Sintassi

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ----------- | --------------- |
| /y | Elimina la richiesta di conferma della cancellazione di tutte le impostazioni dei criteri di controllo. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note

Per le operazioni di cancellazione per i criteri per utente e i criteri di sistema, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire l'operazione di cancellazione utilizzando il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di cancellazione.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
