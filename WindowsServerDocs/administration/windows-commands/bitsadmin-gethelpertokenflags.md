---
title: gethelpertokenflags Bitsadmin
description: Windows Commands Topic for **BITSAdmin gethelpertokenflags**, che restituisce i flag di utilizzo per un token Helper associato a un processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 37e40ea1f71795e0c975988169577deb205c3335
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850664"
---
# <a name="bitsadmin-gethelpertokenflags"></a>gethelpertokenflags Bitsadmin

Restituisce i flag di utilizzo per un [token helper](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) associato a un processo di trasferimento BITS.

> [!NOTE]
> Questo comando non è supportato da BITS 3,0 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Note

Possibili valori restituiti, tra cui:

- **0x0001.** Il token Helper viene usato per aprire il file locale di un processo di caricamento, per creare o rinominare il file temporaneo di un processo di download o per creare o rinominare il file di risposta di un processo di caricamento-risposta.

- **0x0002.** Il token di supporto viene utilizzato per aprire il file remoto di un processo di caricamento o download SMB (Server Message Block) oppure in risposta a una richiesta di credenziali NTLM o Kerberos implicita da parte di un server HTTP o di un proxy. Per consentire l'invio delle credenziali su HTTP, è necessario chiamare/SetCredentialsJob TargetScheme NULL NULL.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
