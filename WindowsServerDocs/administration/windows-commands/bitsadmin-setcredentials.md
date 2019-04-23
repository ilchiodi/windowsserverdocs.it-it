---
title: bitsadmin setcredentials
description: Argomento i comandi di Windows per **bitsadmin setcredentials** -aggiunge le credenziali di un processo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 923dcff7d268d40b72db3254e2a97c808c7c7253
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877392"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Aggiunge le credenziali di un processo.

**BITS 1.2 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Destinazione|SERVER o PROXY|
|Schema|Uno dei valori seguenti:</br>-BASE, ovvero lo schema di autenticazione in cui il nome utente e password vengono inviati in testo non crittografato al server o proxy.</br>-DIGEST, uno schema di autenticazione challenge / response che utilizza una stringa di dati specificata dal server per la richiesta di verifica.</br>-NTLM, ovvero uno schema di autenticazione challenge / response che utilizza le credenziali dell'utente per l'autenticazione in un ambiente di rete di Windows.</br>-NEGOTIATE, noto anche come il protocollo di negoziazione protetta e semplice (Snego) è uno schema di autenticazione in attesa / risposta che negozia con il server o proxy per determinare lo schema da utilizzare per l'autenticazione. Alcuni esempi sono il protocollo Kerberos e NTLM.</br>-PASSPORT, ovvero un servizio di autenticazione centralizzato fornito da Microsoft che offre un unico punto di accesso per i siti membri.|
|Nome utente|Il nome delle credenziali fornite|
|Password|La password associata all'oggetto fornito *nome utente*|

## <a name="BKMK_examples"></a>Esempi

Le seguenti credenziali Adds di esempio per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)