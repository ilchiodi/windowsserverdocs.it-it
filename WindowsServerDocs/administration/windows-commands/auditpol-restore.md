---
title: ripristino auditpol
description: Windows Commands argomento for **auditpol restore**, che ripristina le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file sintatticamente coerente con il formato di file con valori delimitati da virgole (CSV) usato dall'opzione/backup.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd166ca6f3a268015e5cd25bd1fbdd78e1a9eed7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851164"
---
# <a name="auditpol-restore"></a>ripristino auditpol

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file sintatticamente coerente con il formato di file con valori delimitati da virgole (CSV) usato dall'opzione/backup.

## <a name="syntax"></a>Sintassi

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /file | Specifica il file da cui ripristinare i criteri di controllo. Il file deve essere stato creato usando l'opzione/backup o deve essere sintatticamente coerente con il formato di file CSV usato dall'opzione/backup. |
| /? |Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note

Per le operazioni di ripristino per i criteri per utente e i criteri di sistema, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire l'operazione di ripristino possedendo il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). SeSecurityPrivilege è utile per il ripristino del descrittore di sicurezza in caso di errore accidentale o di attacchi dannosi.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per ripristinare le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file denominato AuditPolicy. csv creato con il comando/backup, digitare:

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [backup auditpol](auditpol-backup.md) '    '    