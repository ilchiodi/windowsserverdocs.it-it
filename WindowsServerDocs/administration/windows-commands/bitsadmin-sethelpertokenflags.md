---
title: sethelpertokenflags Bitsadmin
description: Windows Commands Topic for Bitsadmin sethelpertokenflags, che imposta i flag di utilizzo per un token Helper associato a un processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c644e82026cfc1d62f3fb5d20e3925002b871036
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849494"
---
# <a name="bitsadmin-sethelpertokenflags"></a>sethelpertokenflags Bitsadmin

Imposta i flag di utilizzo per un [token helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) associato a un processo di trasferimento BITS.

**BITS 3,0 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o GUID del processo.|
|Flag|I valori possibili sono i seguenti. 0x0001&mdash;il token Helper viene usato per aprire il file locale di un processo di caricamento, per creare o rinominare il file temporaneo di un processo di download o per creare o rinominare il file di risposta di un processo di caricamento-risposta. 0x0002&mdash;il token Helper viene usato per aprire il file remoto di un processo di caricamento o download SMB (Server Message Block) o in risposta a una richiesta di credenziali NTLM o Kerberos implicita per il server o il proxy HTTP. È necessario chiamare `/SetCredentialsJob TargetScheme NULL NULL` per consentire l'invio delle credenziali tramite HTTP.|

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
