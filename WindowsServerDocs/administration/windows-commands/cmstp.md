---
title: cmstp
description: Argomento di riferimento per cmstp, che consente di installare o rimuovere un profilo del servizio di gestione connessione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11d2ec5b09cfd9440eb22d66578061ddfb157539
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712091"
---
# <a name="cmstp"></a>cmstp

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installa o rimuove un profilo del servizio di gestione connessione. Se utilizzato senza parametri facoltativi, **cmstp** viene installato un profilo del servizio con le impostazioni predefinite adatte per il sistema operativo e le autorizzazioni dell'utente.

## <a name="syntax"></a>Sintassi

Sintassi 1: questa è la sintassi tipica utilizzata in un'applicazione di installazione personalizzata. Per usare questa sintassi, è necessario eseguire **cmstp** dalla directory che contiene il `<serviceprofilefilename>.exe` file.

```
<serviceprofilefilename>.exe /q:a /c:cmstp.exe <serviceprofilefilename>.inf [/nf] [/s] [/u]
```

Sintassi 2
```
cmstp.exe [/nf] [/s] [/u] [drive:][path]serviceprofilefilename.inf
```

#### <a name="parameters"></a>Parametri
| Parametro | Descrizione |
| --------- | ----------- |
| `<serviceprofilefilename>.exe` | Specifica il nome, il pacchetto di installazione che contiene il profilo che si desidera installare.<p>Obbligatorio per la sintassi 1, ma non valido per la sintassi 2. |
| /q: un | Specifica che il profilo deve essere installato senza chiedere conferma all'utente. Verrà comunque visualizzato il messaggio di verifica che l'installazione ha avuto esito positivo.<p>Obbligatorio per la sintassi 1, ma non valido per la sintassi 2. |
| [unità:] percorso`<serviceprofilefilename>.inf` | Obbligatorio. Specifica il nome, il file di configurazione che determina come deve essere installato il profilo.<p>Il parametro [unità:] [percorso] non è valido per la sintassi 1. |
| /Nf. | Specifica che i file di supporto non devono essere installati. |
| /s | Specifica che il profilo del servizio deve essere installato o disinstallato automaticamente (senza chiedere conferma per la risposta dell'utente o visualizzare messaggio di verifica). Si tratta dell'unico parametro che è possibile usare in combinazione con **/u**.|
| /U | Specifica che il profilo del servizio deve essere disinstallato. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per installare il profilo del servizio *fittizio* senza alcun file di supporto, digitare:

```
fiction.exe /c:cmstp.exe fiction.inf /nf
```

Per installare automaticamente il profilo del servizio *fittizio* per un singolo utente, digitare:

```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```

Per disinstallare automaticamente il profilo del servizio *fittizio* , digitare:

```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
