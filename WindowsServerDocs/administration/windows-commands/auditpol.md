---
title: auditpol
description: Argomento i comandi di Windows per **auditpol** - Visualizza informazioni su ed esegue le funzioni per gestire i criteri di controllo.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7e8364be977e868ac161704e67c37ec5c400e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849222"
---
# <a name="auditpol"></a>auditpol



Visualizza informazioni su ed esegue le funzioni per gestire i criteri di controllo.

Per esempi di come è possibile utilizzare questo comando, vedere la sezione esempi in ogni argomento.

## <a name="syntax"></a>Sintassi

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parametri

|Sottocomando|Descrizione|
|-----------|-----------|
|/get|Consente di visualizzare i criteri di controllo corrente.</br>Visualizzare [Auditpol get](auditpol-get.md) per la sintassi e opzioni.|
|/set|Imposta i criteri di controllo.</br>Visualizzare [Auditpol set](auditpol-set.md) per la sintassi e opzioni.|
|/list|Consente di visualizzare gli elementi selezionabili.</br>Visualizzare [Auditpol list](auditpol-list.md) per la sintassi e opzioni.|
|/backup|Salva il criterio di controllo in un file.</br>Visualizzare [Auditpol backup](auditpol-backup.md) per la sintassi e opzioni.|
|/Restore.|Ripristina i criteri di controllo da un file che è stato creato in precedenza tramite auditpol /backup.</br>Visualizzare [Auditpol ripristino](auditpol-restore.md) per la sintassi e opzioni.|
|/clear|Cancella i criteri di controllo.</br>Visualizzare [Auditpol Cancella](auditpol-clear.md) per la sintassi e opzioni.|
|/remove|Rimuove tutte le impostazioni di criteri di controllo per utente e disabilita tutte le impostazioni di criteri di controllo del sistema.</br>Visualizzare [Auditpol remove](auditpol-remove.md) per la sintassi e opzioni.|
|/resourceSACL|Consente di configurare gli elenchi di controllo accesso risorse globali del sistema (SACL).</br>Nota: Si applica solo a Windows 7 e Windows Server 2008 R2.</br>Visualizzare [Auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Lo strumento da riga di comando di criteri di controllo è utilizzabile per:
-   Impostare ed eseguire query di un criterio di controllo di sistema.
-   Impostare ed eseguire query di un criterio di controllo per utente.
-   Impostare le opzioni di controllo di query.
-   Impostare ed eseguire query il descrittore di sicurezza usato per delegare l'accesso a un criterio di controllo.
-   Segnalare o eseguire il backup di un criterio di controllo in un file di testo con valori delimitati da virgole (CSV).
-   Caricare un criterio di controllo da un file di testo CSV.
-   Configurare la risorsa globale SACL.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)