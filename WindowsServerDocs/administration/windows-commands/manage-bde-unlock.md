---
title: 'gestione: sblocco BDE'
description: Argomento di riferimento per il comando Manage-bde Unlock, che sblocca un'unità protetta da BitLocker utilizzando una password di ripristino o una chiave di ripristino.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67d4c0ec78870af45f0b98f2ab04d85b19e92af9
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222160"
---
# <a name="manage-bde-unlock"></a>gestione: sblocco BDE

Sblocca un'unità protetta da BitLocker utilizzando una password di ripristino o una chiave di ripristino.

## <a name="syntax"></a>Sintassi

```
manage-bde -unlock {-recoverypassword <password>|-recoverykey <pathtoexternalkeyfile>} <drive> [-certificate {-cf pathtocertificatefile | -ct certificatethumbprint} {-pin}] [-password] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -recoverypassword | Specifica che verrà usata una password di ripristino per sbloccare l'unità. È inoltre possibile utilizzare **- rp** come una versione abbreviata di questo comando. |
| `<password>` | Rappresenta la password di ripristino che può essere utilizzata per sbloccare l'unità. |
| -recoverykey | Specifica un file di chiave esterna di ripristino verrà utilizzato per sbloccare l'unità. È inoltre possibile utilizzare **- rk** come una versione abbreviata di questo comando. |
| `<pathtoexternalkeyfile>` | Rappresenta il file di chiave esterna di ripristino che può essere utilizzato per sbloccare l'unità. |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -certificato | Il certificato utente locale per un certificato BitLocker per sbloccare il volume si trova nell'archivio certificati utente locale. È inoltre possibile utilizzare **-cert** come una versione abbreviata di questo comando. |
| -CF`<pathtocertificatefile>` | Percorso del file di certificato. |
| -CT`<certificatethumbprint>` | Identificazione personale del certificato che può includere facoltativamente il PIN (-pin). |
| -password | Visualizza un prompt dei comandi per la password per sbloccare il volume. È inoltre possibile utilizzare **- pw** come una versione abbreviata di questo comando. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per sbloccare l'unità E con un file di chiave di ripristino salvato in una cartella di backup in un'altra unità, digitare:

```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
