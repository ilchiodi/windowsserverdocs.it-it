---
title: getenctypeattr che Ksetup
description: Argomento di riferimento per il comando che Ksetup getenctypeattr, che consente di recuperare l'attributo del tipo di crittografia per il dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2acead4ff1179002303c18d4feff262080203a28
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817701"
---
# <a name="ksetup-getenctypeattr"></a>getenctypeattr che Ksetup

Recupera l'attributo di tipo di crittografia per il dominio. Viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

È possibile visualizzare il tipo di crittografia per il ticket di concessione ticket (TGT) Kerberos e la chiave della sessione, eseguendo il comando **klist** e visualizzando l'output. È possibile impostare il dominio per connettersi e usare, eseguendo il `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintassi

```
ksetup /getenctypeattr <domainname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<domainname>` | Nome del dominio a cui si desidera stabilire una connessione. Usare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso. |

### <a name="examples"></a>Esempi

Per verificare l'attributo di tipo di crittografia per il dominio, digitare:

```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando klist](klist.md)

- [comando che Ksetup](ksetup.md)

- [comando di dominio che Ksetup](ksetup-domain.md)

- [comando che Ksetup addenctypeattr](ksetup-addenctypeattr.md)

- [comando che Ksetup setenctypeattr](ksetup-setenctypeattr.md)

- [comando che Ksetup delenctypeattr](ksetup-delenctypeattr.md)
