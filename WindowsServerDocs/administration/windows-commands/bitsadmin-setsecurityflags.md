---
title: bitsadmin setsecurityflags
description: Argomento di riferimento per il comando Bitsadmin setsecurityflags, che imposta i flag di sicurezza per HTTP per determinare se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63430f9814037aeb948cc8b91e8590f13a4df00e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720471"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

Imposta i flag di sicurezza per HTTP per determinare se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP. Il valore è un Unsigned Integer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| value | Può includere uno o più dei flag di notifica seguenti, tra cui:<ul><li>Impostare il bit meno significativo per abilitare il controllo CRL.</li><li>Impostare il secondo bit da destra per ignorare i nomi comuni non corretti nel certificato del server.</li><li>Impostare il terzo bit da destra per ignorare le date non corrette nel certificato del server.</li><li>Impostare il quarto bit da destra per ignorare le autorità di certificazione non corrette nel certificato del server.</li><li>Impostare il quinto bit da destra per ignorare l'utilizzo non corretto del certificato del server.</li><li>Impostare il nono per l'undicesimo bit da destra per implementare i criteri di reindirizzamento specificati, tra cui:<ul><li>**0, 0, 0.** I reindirizzamenti sono consentiti automaticamente.</li><li>**0, 0, 1.** Il nome remoto nell'interfaccia **IBackgroundCopyFile** viene aggiornato se si verifica un reindirizzamento.</li><li>**0, 1, 0.** BITS genera un errore nel processo se si verifica un reindirizzamento.</li></ul></li><li>Impostare il dodicesimo bit da destra per consentire il Reindirizzamento da HTTPS a HTTP.</li></ul> |

## <a name="examples"></a>Esempi

Per impostare i flag di sicurezza per abilitare un controllo CRL per il processo denominato *myDownloadJob*:

```
bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
