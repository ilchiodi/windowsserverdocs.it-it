---
title: manage-bde
description: Argomento di riferimento per il comando Manage-bde, che attiva o Disattiva BitLocker, specifica i meccanismi di sblocco, aggiorna i metodi di ripristino e sblocca le unità dati protette da BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84788315a65b32b29a1992580bc6152d15ad02f7
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222062"
---
# <a name="manage-bde"></a>manage-bde

Attiva o Disattiva BitLocker, specifica i meccanismi di sblocco, aggiorna i metodi di ripristino e sblocca le unità dati protette da BitLocker.

> [!NOTE]
> Questo strumento da riga di comando può essere utilizzato al posto dell'elemento del pannello di controllo **crittografia unità BitLocker** .

## <a name="syntax"></a>Sintassi

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm]
[–setidentifier] [-forcerecovery] [–changepassword] [–changepin] [–changekey] [-keypackage] [–upgrade] [-wipefreespace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| [stato gestione-BDE](manage-bde-status.md) | Fornisce informazioni su tutte le unità del computer, indipendentemente dal fatto che siano protette con BitLocker. |
| [Manage-bde on](manage-bde-on.md) | Crittografa l'unità e consente di attivare BitLocker. |
| [Manage-bde disattivato](manage-bde-off.md) | Consente di decrittografare l'unità e consente di disattivare BitLocker. Una volta completata la decrittografia, vengono rimosse tutte le protezioni con chiave. |
| [gestione: Sospendi BDE](manage-bde-pause.md) | Sospende la crittografia o la decrittografia. |
| [Manage-bde resume](manage-bde-resume.md) | Riprende la crittografia o la decrittografia. |
| [gestione-blocco BDE](manage-bde-lock.md) | Impedisce l'accesso ai dati protetti da BitLocker. |
| [gestione: sblocco BDE](manage-bde-unlock.md) | Consente l'accesso ai dati protetti da BitLocker con una password di ripristino o una chiave di ripristino. |
| [sblocco automatico gestione-BDE](manage-bde-autounlock.md) | Gestisce lo sblocco automatico delle unità dati. |
| [protezione da Manage-bde](manage-bde-protectors.md) | Gestisce i metodi di protezione per la chiave di crittografia. |
| [Manage-bde TPM](manage-bde-tpm.md) | Configura il Trusted Platform Module (TPM) del computer. Questo comando non è supportato nei computer che eseguono Windows 8 o **win8_server_2**. Per gestire il TPM su questi computer, utilizzare lo snap-in MMC di gestione TPM o i cmdlet di gestione TPM per Windows PowerShell. |
| [Manage-bde (identificatore)](manage-bde-setidentifier.md)   | Imposta il campo dell'identificatore di unità sull'unità per il valore specificato nel **agli identificatori univoci per l'organizzazione** impostazione criteri di gruppo. |
| [Manage-bde ForceRecovery](manage-bde-forcerecovery.md) | Forza un'unità protetta da BitLocker in modalità di ripristino al riavvio. Questo comando Elimina tutte le protezioni con chiave relative al TPM dall'unità. Al riavvio del computer, solo una password o una chiave di ripristino può essere utilizzata per sbloccare l'unità. |
| [Manage-bde ChangePassword](manage-bde-changepassword.md) | Modifica la password per un'unità di dati. |
| [Manage-bde changepin aggiorna](manage-bde-changepin.md) | Modifica il PIN per un'unità del sistema operativo. |
| [Manage-bde ChangeKey](manage-bde-changekey.md) | Modifica la chiave di avvio per un'unità del sistema operativo. |
| [Manage-bde (pacchetto)](manage-bde-keypackage.md) | Genera un pacchetto di chiavi per un'unità. |
| [aggiornamento di Manage-bde](manage-bde-upgrade.md) | Aggiorna la versione di BitLocker. |
| [Manage-bde WipeFreeSpace](manage-bde-wipefreespace.md) | Cancella lo spazio disponibile in un'unità. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Abilitazione di BitLocker tramite la riga di comando](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
