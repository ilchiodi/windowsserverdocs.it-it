---
title: bitsadmin gethelpertokenflags
description: Argomento di riferimento per il comando Bitsadmin gethelpertokenflags, che restituisce i flag di utilizzo per un token Helper associato a un processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 23e93ca71915fc369a940a21ce856b14deced004
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717902"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

Restituisce i flag di utilizzo per un [token](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) Helper associato a un processo di trasferimento BITS.

> [!NOTE]
> Questo comando non è supportato da BITS 3,0 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

### <a name="remarks"></a>Osservazioni

Possibili valori restituiti, tra cui:

- **0x0001.** Il token Helper viene usato per aprire il file locale di un processo di caricamento, per creare o rinominare il file temporaneo di un processo di download o per creare o rinominare il file di risposta di un processo di caricamento-risposta.

- **0x0002.** Il token di supporto viene utilizzato per aprire il file remoto di un processo di caricamento o download SMB (Server Message Block) oppure in risposta a una richiesta di credenziali NTLM o Kerberos implicita da parte di un server HTTP o di un proxy. È necessario chiamare `/SetCredentialsJob TargetScheme NULL NULL` per consentire l'invio delle credenziali tramite http.
  
## <a name="examples"></a>Esempi

Per recuperare i flag di utilizzo per un token Helper associato a un processo di trasferimento BITS denominato *myDownloadJob*:

```
bitsadmin /gethelpertokenflags myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
