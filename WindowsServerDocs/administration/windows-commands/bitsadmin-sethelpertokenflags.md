---
title: sethelpertokenflags Bitsadmin
description: Windows Commands Topic for **BITSAdmin sethelpertokenflags**, che imposta i flag di utilizzo per un token Helper associato a un processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 77fac03dba2bb0686a98206405622e2eb398953e
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122978"
---
# <a name="bitsadmin-sethelpertokenflags"></a>sethelpertokenflags Bitsadmin

Imposta i flag di utilizzo per un [token helper](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) associato a un processo di trasferimento BITS.

> [!NOTE]
> Questo comando non è supportato da BITS 3,0 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /sethelpertokenflags <job> <flags>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| flags | Possibili valori dei token helper, tra cui:<ul><li>**0x0001.** Consente di aprire il file locale di un processo di caricamento, di creare o rinominare il file temporaneo di un processo di download o di creare o rinominare il file di risposta di un processo di caricamento-risposta.</li><li>**0x0002.** Consente di aprire il file remoto di un processo di caricamento o download SMB (Server Message Block) oppure in risposta a una richiesta di credenziali NTLM o Kerberos implicita da parte di un server HTTP o di un proxy.</li></ul>È necessario chiamare `/setcredentialsjob targetscheme null null` per inviare le credenziali tramite HTTP. |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
