---
title: bitsadmin setcredentials
description: Argomento dei comandi di Windows per Bitsadmin le credenziali, che aggiungono le credenziali a un processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 918bda93407e029cedaaf5eab937d1bb23dc3c4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849644"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Aggiunge le credenziali di un processo.

**BITS 1,2 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Destinazione|SERVER o PROXY|
|Schema|Uno dei seguenti:</br>-BASE, ovvero lo schema di autenticazione in cui il nome utente e password vengono inviati in testo non crittografato al server o proxy.</br>-DIGEST, uno schema di autenticazione challenge / response che utilizza una stringa di dati specificata dal server per la richiesta di verifica.</br>-NTLM, ovvero uno schema di autenticazione challenge / response che utilizza le credenziali dell'utente per l'autenticazione in un ambiente di rete di Windows.</br>-NEGOTIATE, noto anche come il protocollo di negoziazione protetta e semplice (Snego) è uno schema di autenticazione in attesa / risposta che negozia con il server o proxy per determinare lo schema da utilizzare per l'autenticazione. Alcuni esempi sono il protocollo Kerberos e NTLM.</br>-PASSPORT, ovvero un servizio di autenticazione centralizzato fornito da Microsoft che offre un unico punto di accesso per i siti membri.|
|Nome utente|Il nome delle credenziali fornite|
|Password|La password associata all'oggetto fornito *nome utente*|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Le seguenti credenziali Adds di esempio per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)