---
title: manage-bde
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 567e0ed45f6bef42e82c3a68b3c0cbbb352b12d9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724035"
---
# <a name="manage-bde"></a>manage-bde



Utilizzato per attivare o disattivare BitLocker, specificare i meccanismi di sblocco, aggiornare i metodi di ripristino e sbloccare le unità dati protette con BitLocker. Questo strumento da riga di comando può essere utilizzato al posto dell'elemento del pannello di controllo **crittografia unità BitLocker** .

## <a name="syntax"></a>Sintassi

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Manage-bde: status](manage-bde-status.md)|Fornisce informazioni su tutte le unità del computer, indipendentemente dal fatto che siano protette con BitLocker.|
|[Manage-bde: on](manage-bde-on.md)|Crittografa l'unità e consente di attivare BitLocker.|
|[Manage-bde: off](manage-bde-off.md)|Consente di decrittografare l'unità e consente di disattivare BitLocker. Una volta completata la decrittografia, vengono rimosse tutte le protezioni con chiave.|
|[Manage-bde: pause](manage-bde-pause.md)|Sospende la crittografia o la decrittografia.|
|[Manage-bde: resume](manage-bde-resume.md)|Riprende la crittografia o la decrittografia.|
|[Manage-bde: lock](manage-bde-lock.md)|Impedisce l'accesso ai dati protetti da BitLocker.|
|[Manage-bde: unlock](manage-bde-unlock.md)|Consente l'accesso ai dati protetti da BitLocker con una password di ripristino o una chiave di ripristino.|
|[Manage-bde: autounlock](manage-bde-autounlock.md)|Gestisce lo sblocco automatico delle unità dati.|
|[Manage-bde: protectors](manage-bde-protectors.md)|Gestisce i metodi di protezione per la chiave di crittografia.|
|[Manage-bde: tpm](manage-bde-tpm.md)|Configura il Trusted Platform Module (TPM) del computer. Questo comando non è supportato nei computer che eseguono Windows 8 o **win8_server_2**. Per gestire il TPM su questi computer, utilizzare lo snap-in MMC di gestione TPM o i cmdlet di gestione TPM per Windows PowerShell.|
|[Manage-bde: setidentifier](manage-bde-setidentifier.md)|Imposta il campo dell'identificatore di unità sull'unità per il valore specificato nel **agli identificatori univoci per l'organizzazione** impostazione criteri di gruppo.|
|[Manage-bde: ForceRecovery](manage-bde-forcerecovery.md)|Forza un'unità protetta da BitLocker in modalità di ripristino al riavvio. Questo comando Elimina tutte le protezioni con chiave relative al TPM dall'unità. Al riavvio del computer, solo una password o una chiave di ripristino può essere utilizzata per sbloccare l'unità.|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|Modifica la password per un'unità di dati.|
|[Manage-bde: changepin](manage-bde-changepin.md)|Modifica il PIN per un'unità del sistema operativo.|
|[Manage-bde: changekey](manage-bde-changekey.md)|Modifica la chiave di avvio per un'unità del sistema operativo.|
|[Manage-bde: KeyPackage](manage-bde-keypackage.md)|Genera un pacchetto di chiavi per un'unità.|
|[Manage-bde: upgrade](manage-bde-upgrade.md)|Aggiorna la versione di BitLocker.|
|[Manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Cancella lo spazio disponibile in un'unità.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per visualizzare le unità del computer e indica se sono protette da BitLocker e lo stato di crittografia corrente.
```
manage-bde -status
```
Per illustrare l'abilitazione di BitLocker nell'unità C con l'opzione di una password di ripristino. La password di ripristino verrà generata da BitLocker e visualizzata sullo schermo per poterla registrare.
```
manage-bde –on C: -recoverypassword
```
Per illustrare lo sblocco di un'unità protetta da BitLocker utilizzando una password di ripristino.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Abilitazione di BitLocker tramite la riga di comando](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
