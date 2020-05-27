---
title: cacls
description: Argomento di riferimento per il comando cacls. Questo comando è stato deprecato e non è garantito che sia supportato nelle versioni future di Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8602157bf87e523d6d842d5636031c61b52e8ef4
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819251"
---
# <a name="cacls"></a>cacls

>[!IMPORTANT]
> Questo comando è stato deprecato. Usare invece [icacls](icacls.md) .

Visualizza o modifica gli elenchi di controllo di accesso discrezionale (DACL) sui file specificati.

## <a name="syntax"></a>Sintassi

```
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<filename>` | Obbligatorio. Visualizza gli ACL dei file specificati. |
| /t | Modifica gli ACL dei file specificati nella directory corrente e in tutte le sottodirectory. |
| /m | Modifica gli ACL dei volumi montati in una directory. |
| /l | Funziona sul collegamento simbolico anziché sulla destinazione. |
| /s: SDDL | Sostituisce gli ACL con quelli specificati nella stringa SDDL. Questo parametro non è valido per l'uso con i parametri **/e**, **/g**, **/r**, **/p**o **/d** . |
| /e | Modificare un ACL anziché sostituirlo. |
| /C | Continua in seguito a errori di accesso negato. |
| `/g user:<perm>` | Concede i diritti di accesso utente specificati, inclusi i valori validi per l'autorizzazione:<ul><li>**n** -None</li><li>lettura **r**</li><li>**w** -Write</li><li>**c** -modifica (scrittura)</li><li>**f** -controllo completo</li></ul> |
| /r (utente) [...] | Revocare i diritti di accesso dell'utente specificato. Valido solo se usato con il parametro **/e** . |
| `[/p user:<perm> [...]` | Sostituire i diritti di accesso dell'utente specificato, inclusi i valori validi per l'autorizzazione:<ul><li>**n** -None</li><li>lettura **r**</li><li>**w** -Write</li><li>**c** -modifica (scrittura)</li><li>**f** -controllo completo</li></ul> |
| [/d utente [...] | Negare l'accesso utente specificato. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="sample-output"></a>Output di esempio

| Output | La voce di controllo di accesso (ACE) si applica a |
-------- | ------------------------------------- |
| OI | Oggetto che eredita. Questa cartella e i file. |
| CI | Il contenitore eredita. Questa cartella e le relative sottocartelle. |
| IO | Solo ereditarietà. La voce ACE non si applica al file o alla directory corrente. |
| Nessun messaggio di output | Solo questa cartella. |
| OI Ci | Questa cartella, le sottocartelle e i file. |
| OI Ci Io | Solo sottocartelle e file. |
| Ci Io | Solo sottocartelle. |
| OI Io | Solo file. |

#### <a name="remarks"></a>Osservazioni

- È possibile utilizzare caratteri jolly (**?** e **&#42;**) per specificare più file.

- È possibile specificare più di un utente.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [icacls](icacls.md)
