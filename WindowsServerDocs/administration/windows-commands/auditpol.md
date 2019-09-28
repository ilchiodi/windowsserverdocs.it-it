---
title: auditpol
description: 'Argomento dei comandi di Windows per **auditpol** : Visualizza informazioni su ed esegue funzioni per modificare i criteri di controllo.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e249a9e2a07505f052b774208c514b4d16879b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382384"
---
# <a name="auditpol"></a>auditpol



Visualizza informazioni su ed esegue funzioni per modificare i criteri di controllo.

Per esempi di come è possibile usare questo comando, vedere la sezione Esempi in ogni argomento.

## <a name="syntax"></a>Sintassi

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parametri

|Sottocomando|Descrizione|
|-----------|-----------|
|/Get|Visualizza i criteri di controllo correnti.</br>Vedere [auditpol get](auditpol-get.md) per la sintassi e le opzioni.|
|/set|Imposta i criteri di controllo.</br>Per la sintassi e le opzioni, vedere [auditpol set](auditpol-set.md) .|
|/List|Visualizza gli elementi del criterio selezionabili.</br>Per la sintassi e le opzioni, vedere [auditpol list](auditpol-list.md) .|
|/backup|Salva i criteri di controllo in un file.</br>Per informazioni sulla sintassi e sulle opzioni, vedere [backup di auditpol](auditpol-backup.md) .|
|/Restore|Ripristina i criteri di controllo da un file creato in precedenza tramite auditpol/backup.</br>Per informazioni sulla sintassi e sulle opzioni, vedere [auditpol restore](auditpol-restore.md) .|
|/Clear|Cancella i criteri di controllo.</br>Vedere [auditpol clear](auditpol-clear.md) per la sintassi e le opzioni.|
|/remove|Rimuove tutte le impostazioni dei criteri di controllo per singolo utente e Disabilita tutte le impostazioni dei criteri di controllo del sistema.</br>Per la sintassi e le opzioni, vedere [auditpol Remove](auditpol-remove.md) .|
|/resourceSACL|Configura gli elenchi di controllo di accesso (SACL) globali del sistema di risorse.</br>Nota: Si applica solo a Windows 7 e Windows Server 2008 R2.</br>Vedere [auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Lo strumento da riga di comando per i criteri di controllo può essere utilizzato per:
-   Impostare ed eseguire una query sui criteri di controllo del sistema.
-   Impostare ed eseguire una query su un criterio di controllo per utente.
-   Opzioni di controllo set e query.
-   Impostare ed eseguire una query sul descrittore di sicurezza utilizzato per delegare l'accesso ai criteri di controllo.
-   Segnalare o eseguire il backup di un criterio di controllo in un file di testo con valori delimitati da virgole (CSV).
-   Caricare i criteri di controllo da un file di testo CSV.
-   Configurare SACL di risorse globali.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)