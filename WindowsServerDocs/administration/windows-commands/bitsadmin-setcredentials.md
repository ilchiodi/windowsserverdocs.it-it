---
title: bitsadmin setcredentials
description: Argomento di riferimento per il comando Bitsadmin secredentials, che aggiunge le credenziali a un processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fbedcc65931e7d3cfb1719786f423b0d071b411
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719320"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Aggiunge le credenziali di un processo.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setcredentials <job> <target> <scheme> <username> <password>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| target | Utilizzare il **Server** o il **proxy**. |
| scheme | Usare uno dei seguenti:<ul><li>**Basic.** Schema di autenticazione in cui il nome utente e la password vengono inviati in testo non crittografato al server o al proxy.</li><li>**DIGEST.** Uno schema di autenticazione di richiesta-risposta che usa una stringa di dati specificata dal server per la richiesta di verifica.</li><li>**NTLM.** Uno schema di autenticazione di richiesta-risposta che usa le credenziali dell'utente per l'autenticazione in un ambiente di rete Windows.</li><li>**NEGOTIATe (noto anche come protocollo di negoziazione semplice e protetto).** Uno schema di autenticazione di richiesta-risposta che negozia con il server o il proxy per determinare lo schema da usare per l'autenticazione. Alcuni esempi sono il protocollo Kerberos e NTLM.</li><li>**Passport.** Servizio di autenticazione centralizzato fornito da Microsoft che offre un unico accesso per i siti membri.</li></ul> |
| nome_utente | Il nome dell'utente. |
| password | Password associata al *nome utente*specificato. |

## <a name="examples"></a>Esempi

Per aggiungere le credenziali al processo denominato *myDownloadJob*:

```
bitsadmin /setcredentials myDownloadJob SERVER BASIC Edward password20
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
