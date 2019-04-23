---
title: attach vdisk
description: Argomento i comandi di Windows per **collegare vdisk** -collega (talvolta denominate punti di montaggio o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato come un'unità disco rigido locale del computer host.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e715b36f8d9d8b416311567c2455920243dfc7a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885612"
---
# <a name="attach-vdisk"></a>attach vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Collega (talvolta denominate punti di montaggio o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato come un'unità disco rigido locale del computer host. Se, nel momento in cui viene reso visibile, il VHD dispone già di una partizione e di un volume di file system, al volume verrà assegnata una lettera di unità.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|readonly|Collega il disco rigido virtuale in sola lettura. Qualsiasi scrittura operazione restituisce un errore.|
|sd=<SDDL string>|Imposta il filtro utente nel disco rigido virtuale. La stringa di filtro deve essere nel formato Security Descriptor Definition Language (SDDL). Per impostazione predefinita il filtro utente consente l'accesso, ad esempio su un disco fisico.<br /><br />Le stringhe SDDL possono essere complesse, ma nella sua forma più semplice, un descrittore di sicurezza che protegge l'accesso è noto come un elenco di controllo di accesso discrezionale (DACL). È nel formato: Unità d: < dacl_flags >< string_ace1 >< string_ace2 >... < string_acen ><br /><br />Flag DACL comuni sono:<br /><br />-   **Oggetto** consentire l'accesso<br />-   **1!d** negare l'accesso<br /><br />Diritti comuni sono:<br /><br />-   **GA** tutti gli accessi<br />-   **GR** accesso in lettura<br />-   **GW** accesso in scrittura<br /><br />Gli account utente comuni sono:<br /><br />-   **BA** compilato negli amministratori<br />-   **AU** agli utenti autenticati<br />-   **CO** Creator owner<br />-   **WD** -tutti gli utenti<br /><br />Esempi:<br /><br />**D:P:(A;; GR;; AU** offre accesso in lettura a tutti gli utenti autenticati<br /><br />**D:P:(A;; DISPONIBILITÀ GENERALE;; WD** fornisce tutti gli utenti accesso completo|
|usefilesd|Specifica che il descrittore di sicurezza nel file VHD deve essere utilizzato del disco rigido virtuale. Se il **Usefilesd** parametro viene omesso, il disco rigido virtuale non avrà un descrittore di sicurezza espliciti a meno che non viene specificata con il **Sd** parametro.|
|NOERR|Usato solo negli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|
## <a name="remarks"></a>Note
-   Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
## <a name="BKMK_Examples"></a>Esempi
Per collegare il disco rigido virtuale selezionato in sola lettura, digitare:
```
attach vdisk readonly
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [compact vdisk](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [Detach vdisk](detach-vdisk.md)
-   [expand vdisk](expand-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [Selezionare vdisk](select-vdisk.md)
-   [list_1](list_1.md)
