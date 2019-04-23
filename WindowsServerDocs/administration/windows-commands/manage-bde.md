---
title: manage-bde
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8923177b03f378f8252c532ec386f1808e516e1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874502"
---
# <a name="manage-bde"></a>manage-bde



Consente di attivare o disattivare BitLocker, specificare meccanismi di sblocco, aggiornare metodi di ripristino e sbloccare le unità dati protette da BitLocker. Questo strumento da riga di comando può essere usato al posto del **BitLocker Drive Encryption** elemento del Pannello di controllo. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[gestire-bde: stato](manage-bde-status.md)|Fornisce informazioni su tutte le unità nel computer, se non sono protette da BitLocker.|
|[gestire-bde: in](manage-bde-on.md)|Crittografa l'unità e consente di attivare BitLocker.|
|[Gestire-bde: off](manage-bde-off.md)|Consente di decrittografare l'unità e consente di disattivare BitLocker. Una volta completata la decrittografia, vengono rimosse tutte le protezioni con chiave.|
|[gestire-bde: sospendere](manage-bde-pause.md)|Sospende la crittografia o decrittografia.|
|[Gestire-bde: riprendere](manage-bde-resume.md)|Riprende la crittografia o decrittografia.|
|[gestire-bde: blocco](manage-bde-lock.md)|Impedisce l'accesso ai dati protetti da BitLocker.|
|[gestire-bde: sbloccare](manage-bde-unlock.md)|Consente l'accesso ai dati protetti da BitLocker con una password di ripristino o una chiave di ripristino.|
|[Gestire-bde: lo sblocco automatico](manage-bde-autounlock.md)|Gestisce lo sblocco automatico delle unità dati.|
|[gestire-bde: protezioni](manage-bde-protectors.md)|Gestisce i metodi di protezione della chiave di crittografia.|
|[gestire-bde: tpm](manage-bde-tpm.md)|Configura modulo TPM del computer (Trusted Platform). Questo comando non è supportato nei computer che eseguono Windows 8 o **win8_server_2**. Per gestire il TPM su questi computer, utilizzare lo snap-in MMC di gestione TPM o i cmdlet di gestione TPM per Windows PowerShell.|
|[gestire-bde: setidentifier](manage-bde-setidentifier.md)|Imposta il campo dell'identificatore di unità sull'unità per il valore specificato nel **agli identificatori univoci per l'organizzazione** impostazione criteri di gruppo.|
|[Gestire-bde: ForceRecovery](manage-bde-forcerecovery.md)|Forza un'unità protetta da BitLocker in modalità di ripristino al riavvio. Questo comando Elimina tutte le protezioni con chiave correlati al TPM dal disco. Al riavvio del computer, solo una password o una chiave di ripristino può essere utilizzata per sbloccare l'unità.|
|[Gestire-bde: changepassword](manage-bde-changepassword.md)|Modifica la password per un'unità di dati.|
|[Gestire-bde: changepin Aggiorna](manage-bde-changepin.md)|Modifica il PIN per un'unità del sistema operativo.|
|[gestire-bde: changekey](manage-bde-changekey.md)|Modifica la chiave di avvio per un'unità del sistema operativo.|
|[Gestire-bde: KeyPackage](manage-bde-keypackage.md)|Genera un pacchetto di chiavi per un'unità.|
|[gestire-bde: eseguire l'aggiornamento](manage-bde-upgrade.md)|Aggiorna la versione di BitLocker.|
|[Gestire-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Cancella lo spazio libero su un'unità.|
|-? o /?|Consente di visualizzare breve guida al prompt dei comandi.|
|-help o -h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente visualizza le unità del computer e identifica se sono protette da BitLocker e lo stato di crittografia corrente.
```
manage-bde -status
```
Nell'esempio seguente viene illustrata l'abilitazione di BitLocker nell'unità C con la possibilità di una password di ripristino. La password di ripristino verrà generata da BitLocker e visualizzata sullo schermo, in modo che possono essere registrate.
```
manage-bde –on C: -recoverypassword
```
Nell'esempio seguente viene illustrato come sbloccare un'unità protetta da BitLocker utilizzando una password di ripristino.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Abilitazione di BitLocker tramite la riga di comando](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
