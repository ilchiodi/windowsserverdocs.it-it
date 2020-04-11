---
title: setsecurityflags Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setsecurityflags**, che imposta i flag di sicurezza per http per determinare se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d73361bceda8c0eb24992bdee176b47bf82a878
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122720"
---
# <a name="bitsadmin-setsecurityflags"></a>setsecurityflags Bitsadmin

Imposta i flag di sicurezza per HTTP per determinare se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP. Il valore è un Unsigned Integer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| value | Può includere uno o più dei flag di notifica seguenti, tra cui:<ul><li>Impostare il bit meno significativo per abilitare il controllo CRL.</li><li>Impostare il secondo bit da destra per ignorare i nomi comuni non corretti nel certificato del server.</li><li>Impostare il terzo bit da destra per ignorare le date non corrette nel certificato del server.</li><li>Impostare il quarto bit da destra per ignorare le autorità di certificazione non corrette nel certificato del server.</li><li>Impostare il quinto bit da destra per ignorare l'utilizzo non corretto del certificato del server.</li><li>Impostare il nono per l'undicesimo bit da destra per implementare i criteri di reindirizzamento specificati, tra cui:<ul><li>**0, 0, 0.** I reindirizzamenti sono consentiti automaticamente.</li><li>**0, 0, 1.** Il nome remoto nell'interfaccia **IBackgroundCopyFile** viene aggiornato se si verifica un reindirizzamento.</li><li>**0, 1, 0.** BITS genera un errore nel processo se si verifica un reindirizzamento.</li></ul></li><li>Impostare il dodicesimo bit da destra per consentire il Reindirizzamento da HTTPS a HTTP.</li></ul> |

## <a name="examples"></a>Esempi

Nell'esempio seguente vengono impostati i flag di sicurezza per abilitare un controllo CRL per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)