---
title: bitsadmin removecredentials
description: Argomento di riferimento per il comando Bitsadmin removecredentials, che consente di rimuovere le credenziali da un processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4dcfaa55847e531871c6a7ad9fd84c3861c4cd9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717050"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Rimuove le credenziali da un processo.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| target | Utilizzare il **Server** o il **proxy**. |
| scheme | Usare uno dei seguenti:<ul><li>**Basic.** Schema di autenticazione in cui il nome utente e la password vengono inviati in testo non crittografato al server o al proxy.</li><li>**DIGEST.** Uno schema di autenticazione di richiesta-risposta che usa una stringa di dati specificata dal server per la richiesta di verifica.</li><li>**NTLM.** Uno schema di autenticazione di richiesta-risposta che usa le credenziali dell'utente per l'autenticazione in un ambiente di rete Windows.</li><li>**NEGOTIATe (noto anche come protocollo di negoziazione semplice e protetto).** Uno schema di autenticazione di richiesta-risposta che negozia con il server o il proxy per determinare lo schema da usare per l'autenticazione. Alcuni esempi sono il protocollo Kerberos e NTLM.</li><li>**Passport.** Servizio di autenticazione centralizzato fornito da Microsoft che offre un unico accesso per i siti membri.</li></ul> |

## <a name="examples"></a>Esempi

Per rimuovere le credenziali dal processo denominato *myDownloadJob*:

```
bitsadmin /removecredentials myDownloadJob SERVER BASIC
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
