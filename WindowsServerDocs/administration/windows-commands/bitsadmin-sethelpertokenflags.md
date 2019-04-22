---
title: sethelpertokenflags Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin sethelpertokenflags** -imposta i flag di utilizzo per un token di supporto che è associato a un processo di trasferimento BITS.
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
ms.openlocfilehash: cc9652afe73476041aa42e64671885bfc1af9628
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813862"
---
# <a name="bitsadmin-sethelpertokenflags"></a>sethelpertokenflags Bitsadmin

Imposta i flag di utilizzo per un [token helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) associato a un processo di trasferimento BITS.

**BITS 3.0 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo.|
|Flag|I valori possibili sono i seguenti. 0x0001&mdash;il token di supporto viene usato per aprire il file locale di un processo di caricamento, per creare o rinominare il file temporaneo di un processo di download, o per creare o rinominare il file reply di un processo di caricamento-risposta. 0x0002&mdash;viene utilizzato per aprire il file remoto di caricamento di un Server Message Block (SMB) o processo di download, il token di supporto o in risposta a una richiesta di server o un proxy HTTP per implicita NTLM o Kerberos per le credenziali necessarie. È necessario chiamare `/SetCredentialsJob TargetScheme NULL NULL` per consentire le credenziali devono essere inviati tramite HTTP.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
