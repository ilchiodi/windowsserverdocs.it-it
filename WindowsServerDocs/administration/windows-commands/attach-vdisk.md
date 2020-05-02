---
title: attach vdisk
description: Argomento di riferimento per il comando collega vdisk, che collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91f988d1f84869874dbd0d6a25dce43ef5138066
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718903"
---
# <a name="attach-vdisk"></a>attach vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale. Se, nel momento in cui viene reso visibile, il VHD dispone già di una partizione e di un volume di file system, al volume verrà assegnata una lettera di unità.

> [!IMPORTANT]
> Per eseguire questa operazione, è necessario scegliere e scollegare un disco rigido virtuale. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| readonly | Connette il VHD come di sola lettura. Qualsiasi operazione di scrittura restituisce un errore. |
| `sd=<SDDL string>` | Imposta il filtro utente sul disco rigido virtuale. La stringa di filtro deve essere nel formato SDDL (Security Descriptor Definition Language). Per impostazione predefinita, il filtro utente consente l'accesso come su un disco fisico. Le stringhe SDDL possono essere complesse, ma nella sua forma più semplice, un descrittore di sicurezza che protegge l'accesso è noto come elenco di controllo di accesso discrezionale (DACL). Usa il formato: `D:<dacl_flags><string_ace1><string_ace2>`...`<string_acen>`<p>I flag DACL comuni sono:<ul><li>**Oggetto**. Consentire l'accesso</li><li>**D**. Rifiutare l'accesso</li></ul>I diritti comuni sono:<ul><li>**GA**. Tutti gli accessi</li><li>**Gr**. accesso in lettura</li><li> **GW**. Accesso in scrittura</li></ul>Gli account utente comuni sono:<ul><li>**BA**. Amministratori predefiniti</li><li>**Au**. utenti autenticati</li><li>**Co**. Proprietario autore</li><li>**WD**. Tutti</li></ul>Esempi:<ul><li>**D:P: (A;; GR;;; AU**. Consente l'accesso in lettura a tutti gli utenti autenticati.</li><li>**D:P: (A;; GA;;; WD**. Consente l'accesso completo a tutti.</li></ul> |
| usefilesd | Specifica che il descrittore di sicurezza nel file con estensione VHD deve essere utilizzato nel disco rigido virtuale. Se il parametro **Usefilesd** non è specificato, il disco rigido virtuale non disporrà di un descrittore di sicurezza esplicito a meno che non sia specificato con il parametro **SD** . |
| NOERR | Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per alleghi il disco rigido virtuale selezionato come di sola lettura, digitare:

```
attach vdisk readonly
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Seleziona vdisk](select-vdisk.md)

- [compatta vdisk](compact-vdisk.md)

- [Dettagli vdisk](detail-vdisk.md)

- [Scollega vdisk](detach-vdisk.md)

- [Espandi vdisk](expand-vdisk.md)

- [Unisci vdisk](merge-vdisk.md)

- [list](list_1.md)
