---
title: gethelpertokenflags Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin gethelpertokenflags** -restituisce i flag di utilizzo per un token di supporto che è associato a un processo di trasferimento BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: e5665ed4670891dcbecd56215342f3d94e1ed753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885792"
---
# <a name="bitsadmin-gethelpertokenflags"></a>gethelpertokenflags Bitsadmin

Restituisce i flag di utilizzo per un [token helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) associato a un processo di trasferimento BITS.

**BITS 3.0 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetHelperTokenFlags <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

I valori restituiti possibili sono i seguenti.

- 0x0001. Il token di supporto viene usato per aprire il file locale di un processo di caricamento, per creare o rinominare il file temporaneo di un processo di download, oppure per creare o rinominare il file reply di un processo di caricamento-risposta.
- 0x0002. Il token di supporto viene utilizzato per aprire il file remoto di caricamento di un Server Message Block (SMB) o processo di download oppure in risposta a una richiesta di server o un proxy HTTP per implicita NTLM o Kerberos per le credenziali. È necessario chiamare SetCredentialsJob TargetScheme NULL NULL per consentire le credenziali devono essere inviati tramite HTTP.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
