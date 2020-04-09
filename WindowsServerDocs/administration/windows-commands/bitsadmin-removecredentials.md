---
title: bitsadmin removecredentials
description: Windows Commands Topic for Bitsadmin **removecredentials**, che rimuove le credenziali da un processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ff7e2a813c7cc6b60e04d55ef63804a2aed796
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849844"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Rimuove le credenziali da un processo.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| target | Utilizzare il **Server** o il **proxy**. |
| scheme | Usare uno dei seguenti elementi:<ul><li>**Basic.** Schema di autenticazione in cui il nome utente e la password vengono inviati in testo non crittografato al server o al proxy.</li><li>**digest.** Uno schema di autenticazione di richiesta-risposta che usa una stringa di dati specificata dal server per la richiesta di verifica.</li><li>**NTLM.** Uno schema di autenticazione di richiesta-risposta che usa le credenziali dell'utente per l'autenticazione in un ambiente di rete Windows.</li><li>**Negotiate (noto anche come protocollo di negoziazione semplice e protetto).** Uno schema di autenticazione di richiesta-risposta che negozia con il server o il proxy per determinare lo schema da usare per l'autenticazione. Alcuni esempi sono il protocollo Kerberos e NTLM.</li><li>**Passport.** Servizio di autenticazione centralizzato fornito da Microsoft che offre un unico accesso per i siti membri.</li></ul> |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente rimuove le credenziali dal processo denominato *myDownloadJob*.

```
C:\>bitsadmin /removecredentials myDownloadJob server basic
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)