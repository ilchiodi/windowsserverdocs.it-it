---
title: setenctypeattr che Ksetup
description: Argomento di riferimento per il comando che Ksetup setenctypeattr, che imposta l'attributo del tipo di crittografia per il dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e76ad3d08505208346ff2a3e100194239187953d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817361"
---
# <a name="ksetup-setenctypeattr"></a>setenctypeattr che Ksetup

Imposta l'attributo del tipo di crittografia per il dominio. Viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

È possibile visualizzare il tipo di crittografia per il ticket di concessione ticket (TGT) Kerberos e la chiave della sessione, eseguendo il comando **klist** e visualizzando l'output. È possibile impostare il dominio per connettersi e usare, eseguendo il `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintassi

```
ksetup /setenctypeattr <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<domainname>` | Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso. |
| tipo di crittografia | Deve essere uno dei tipi di crittografia supportati seguenti:<ul><li>DES-CBC-CRC</li><li>DES-CBC-MD5</li><li>RC4-HMAC-MD5</li><li>AES128-CTS-HMAC-SHA1-96</li><li>AES256-CTS-HMAC-SHA1-96</li></ul> |

#### <a name="remarks"></a>Osservazioni

- È possibile impostare o aggiungere più tipi di crittografia separando i tipi di crittografia nel comando con uno spazio. Tuttavia, è possibile eseguire questa operazione solo per un dominio alla volta.

### <a name="examples"></a>Esempi

Per visualizzare il tipo di crittografia per il ticket di concessione ticket (TGT) Kerberos e la chiave della sessione, digitare:

```
klist
```

Per impostare il dominio su corp.contoso.com, digitare:

```
ksetup /domain corp.contoso.com
```

Per impostare l'attributo del tipo di crittografia su AES-256-CTS-HMAC-SHA1-96 per il dominio corp.contoso.com, digitare:

```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```

Per verificare che l'attributo del tipo di crittografia sia stato impostato come previsto per il dominio, digitare:

```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando klist](klist.md)

- [comando che Ksetup](ksetup.md)

- [comando di dominio che Ksetup](ksetup-domain.md)

- [comando che Ksetup addenctypeattr](ksetup-addenctypeattr.md)

- [comando che Ksetup getenctypeattr](ksetup-getenctypeattr.md)

- [comando che Ksetup delenctypeattr](ksetup-delenctypeattr.md)
