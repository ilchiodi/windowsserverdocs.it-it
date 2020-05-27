---
title: delenctypeattr che Ksetup
description: Argomento di riferimento per il delenctypeattr che Ksetup, che rimuove l'attributo del tipo di crittografia per il dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3076a25b619615402a599bd8aaa6ce9d10d4fe0
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817941"
---
# <a name="ksetup-delenctypeattr"></a>delenctypeattr che Ksetup

Rimuove l'attributo di tipo di crittografia per il dominio. Viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

È possibile visualizzare il tipo di crittografia per il ticket di concessione ticket (TGT) Kerberos e la chiave della sessione, eseguendo il comando **klist** e visualizzando l'output. È possibile impostare il dominio per connettersi e usare, eseguendo il `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintassi

```
ksetup /delenctypeattr <domainname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ----------| ----------- |
| `<domainname>` | Nome del dominio a cui si desidera stabilire una connessione. È possibile utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso. |

### <a name="examples"></a>Esempi

Per determinare i tipi di crittografia correnti impostati nel computer, digitare:

```
klist
```

Per impostare il dominio su mit.contoso.com, digitare:

```
ksetup /domain mit.contoso.com
```

Per verificare l'attributo di tipo di crittografia per il dominio, digitare:

```
ksetup /getenctypeattr mit.contoso.com
```

Per rimuovere l'attributo set Encryption Type per il dominio mit.contoso.com, digitare:

```
ksetup /delenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando klist](klist.md)

- [comando che Ksetup](ksetup.md)

- [comando di dominio che Ksetup](ksetup-domain.md)

- [comando che Ksetup addenctypeattr](ksetup-addenctypeattr.md)

- [comando che Ksetup setenctypeattr](ksetup-setenctypeattr.md)
