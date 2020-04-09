---
title: attach vdisk
description: Windows Commands argomento for **alleghi vdisk**, che collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3a903ed231e34ac902ce10b5342f27e772ac89f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851264"
---
# <a name="attach-vdisk"></a>attach vdisk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale. Se, nel momento in cui viene reso visibile, il VHD dispone già di una partizione e di un volume di file system, al volume verrà assegnata una lettera di unità.

> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| readonly | Connette il VHD come di sola lettura. Qualsiasi operazione di scrittura restituisce un errore. |
| `sd=<SDDL string>` | Imposta il filtro utente sul disco rigido virtuale. La stringa di filtro deve essere nel formato SDDL (Security Descriptor Definition Language). Per impostazione predefinita, il filtro utente consente l'accesso come su un disco fisico. Le stringhe SDDL possono essere complesse, ma nella sua forma più semplice, un descrittore di sicurezza che protegge l'accesso è noto come elenco di controllo di accesso discrezionale (DACL). Il formato è il seguente: `D:<dacl_flags><string_ace1><string_ace2>`... `<string_acen>`<p>I flag DACL comuni sono:<p>-  **.** Consenti accesso<p>- **D**. Nega accesso<p>I diritti comuni sono:<p>- **GA**. Tutti gli accessi<p>- **gr**. accesso in lettura<p>- **GW**. Accesso in scrittura<p>Gli account utente comuni sono:<p>- **BA**. Amministratori predefiniti<p>- **au**. Authenticated users<p>- **co**. Proprietario autore<p>- **WD**. Tutti<p>Esempi:<p>**D:P: (A;; GR;;; AU** concede l'accesso in lettura a tutti gli utenti autenticati.<p>**D:P: (A;; GA;;; WD** consente a tutti l'accesso completo. |
| usefilesd | Specifica che il descrittore di sicurezza nel file con estensione VHD deve essere utilizzato nel disco rigido virtuale. Se il parametro **Usefilesd** non è specificato, il disco rigido virtuale non disporrà di un descrittore di sicurezza esplicito a meno che non sia specificato con il parametro **SD** . |
| NOERR | Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="remarks"></a>Note

- Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Per alleghi il disco rigido virtuale selezionato come di sola lettura, digitare:

```
attach vdisk readonly
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [compatta vdisk](compact-vdisk.md)

- [Dettagli vdisk](detail-vdisk.md)

- [Scollega vdisk](detach-vdisk.md)

- [Espandi vdisk](expand-vdisk.md)

- [Unisci vdisk](merge-vdisk.md)

- [Seleziona vdisk](select-vdisk.md)

- [elenco](list_1.md)
