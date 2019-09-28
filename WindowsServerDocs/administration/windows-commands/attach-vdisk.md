---
title: attach vdisk
description: Windows Commands argomento for **Connetti vdisk** -collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d29eacfc8575ec50859733612a3d58b166d9402d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382645"
---
# <a name="attach-vdisk"></a>attach vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale. Se, nel momento in cui viene reso visibile, il VHD dispone già di una partizione e di un volume di file system, al volume verrà assegnata una lettera di unità.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parametri

|    Parametro     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     readonly     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             connette il VHD come di sola lettura. Qualsiasi operazione di scrittura restituisce un errore.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SD = <SDDL string> | Imposta il filtro utente sul disco rigido virtuale. La stringa di filtro deve essere nel formato SDDL (Security Descriptor Definition Language). Per impostazione predefinita, il filtro utente consente l'accesso come su un disco fisico.<br /><br />Le stringhe SDDL possono essere complesse, ma nella sua forma più semplice, un descrittore di sicurezza che protegge l'accesso è noto come elenco di controllo di accesso discrezionale (DACL). Il formato è il seguente: D: < dacl_flags > < string_ace1 > < string_ace2 >... < string_acen ><br /><br />I flag DACL comuni sono:<br /><br />-   **A** Consenti accesso<br />-   **D** Nega accesso<br /><br />I diritti comuni sono:<br /><br />-   **GA** tutti gli accessi<br />accesso in lettura -   **gr**<br />accesso in scrittura -   **GW**<br /><br />Gli account utente comuni sono:<br /><br />-   **BA** incorporato negli amministratori<br />utenti autenticati -   **au**<br />proprietario**co** Creator -   <br />-   **WD** -Everyone<br /><br />Esempi:<br /><br />**D:P: (A;; GR;;; AU** concede l'accesso in lettura a tutti gli utenti autenticati<br /><br />**D:P: (A;; GA;;; WD** fornisce a tutti l'accesso completo |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          Specifica che il descrittore di sicurezza nel file con estensione VHD deve essere utilizzato nel disco rigido virtuale. Se il parametro **Usefilesd** non è specificato, il disco rigido virtuale non disporrà di un descrittore di sicurezza esplicito a meno che non sia specificato con il parametro **SD** .                                                                                                                                                                                                                                                                                                                                                                                          |
|      NOERR       |                                                                                                                                                                                                                                                                                                                                                                                                           Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Note
- Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
  ## <a name="BKMK_Examples"></a>Esempi
  Per alleghi il disco rigido virtuale selezionato come di sola lettura, digitare:
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
- [compatta vdisk](compact-vdisk.md)

- [Dettagli vdisk](detail-vdisk.md)
- [Scollega vdisk](detach-vdisk.md)
- [Espandi vdisk](expand-vdisk.md)
- [Unisci vdisk](merge-vdisk.md)
- [Seleziona vdisk](select-vdisk.md)
- [list_1](list_1.md)
