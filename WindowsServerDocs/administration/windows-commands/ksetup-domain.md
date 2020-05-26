---
title: dominio che Ksetup
description: Argomento di riferimento per il comando del dominio che Ksetup, che imposta il nome di dominio per tutte le operazioni Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d497f2bc76bae8a95b077658c661e0fdc1e93f3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817801"
---
# <a name="ksetup-domain"></a>dominio che Ksetup

Imposta il nome di dominio per tutte le operazioni di Kerberos.

## <a name="syntax"></a>Sintassi

```
ksetup /domain <domainname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<domainname>` | Nome del dominio a cui si desidera stabilire una connessione. Usare il nome di dominio completo o un modulo semplice del nome, ad esempio contoso.com o contoso.|

### <a name="examples"></a>Esempi

Per stabilire una connessione a un dominio valido, ad esempio Microsoft, usando il `ksetup /mapuser` sottocomando, digitare:

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

Al termine della connessione, si riceverà un nuovo TGT o verrà aggiornato un TGT esistente.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup mapuser](ksetup-mapuser.md)