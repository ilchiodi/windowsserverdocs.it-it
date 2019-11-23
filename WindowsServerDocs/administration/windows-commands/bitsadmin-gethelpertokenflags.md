---
title: gethelpertokenflags Bitsadmin
description: 'Argomento dei comandi di Windows per **BITSAdmin gethelpertokenflags** : restituisce i flag di utilizzo per un token Helper associato a un processo di trasferimento BITS.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 25d667736d5fdcd018f557b2a5565b94898f6e51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381573"
---
# <a name="bitsadmin-gethelpertokenflags"></a>gethelpertokenflags Bitsadmin

Restituisce i flag di utilizzo per un [token helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) associato a un processo di trasferimento BITS.

**BITS 3,0 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetHelperTokenFlags <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Osservazioni

Tra i possibili valori restituiti sono inclusi i seguenti.

- 0x0001. Il token Helper viene usato per aprire il file locale di un processo di caricamento, per creare o rinominare il file temporaneo di un processo di download o per creare o rinominare il file di risposta di un processo di caricamento-risposta.
- 0x0002. Il token di supporto viene utilizzato per aprire il file remoto di un processo di caricamento o download SMB (Server Message Block) oppure in risposta a una richiesta di credenziali NTLM o Kerberos implicita da parte di un server HTTP o di un proxy. Per consentire l'invio delle credenziali su HTTP, è necessario chiamare/SetCredentialsJob TargetScheme NULL NULL.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
