---
title: ripristino auditpol
description: Argomento di riferimento per il comando auditpol restore, che consente di ripristinare le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file sintatticamente coerente con il formato di file con valori delimitati da virgole (CSV) usato dall'opzione/backup.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64605a985c1cff13b842a99ae4ea52485bfc8220
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719053"
---
# <a name="auditpol-restore"></a>ripristino auditpol

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file sintatticamente coerente con il formato di file con valori delimitati da virgole (CSV) usato dall'opzione/backup.

Per eseguire operazioni di *ripristino* sui criteri *per utente* e di *sistema* , è necessario disporre dell'autorizzazione di **controllo completo** o di **scrittura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di *ripristino* se si dispone del diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege), utile per il ripristino del descrittore di sicurezza in caso di errore o attacco dannoso.

## <a name="syntax"></a>Sintassi

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /file | Specifica il file da cui ripristinare i criteri di controllo. Il file deve essere stato creato usando l'opzione/backup o deve essere sintatticamente coerente con il formato di file CSV usato dall'opzione/backup. |
| /? |Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per ripristinare le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file denominato AuditPolicy. csv creato con il comando/backup, digitare:

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [backup auditpol](auditpol-backup.md)

- [comandi auditpol](auditpol.md)
