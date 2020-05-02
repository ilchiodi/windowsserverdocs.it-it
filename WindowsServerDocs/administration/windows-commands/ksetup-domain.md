---
title: 'che Ksetup: dominio'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f127eaf33e9ef6d597851c31a4167ceaa3516abb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724680"
---
# <a name="ksetupdomain"></a>che Ksetup: dominio



Imposta il nome di dominio per tutte le operazioni di Kerberos.

## <a name="syntax"></a>Sintassi

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NomeDominio>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio contoso.com o contoso.|

## <a name="remarks"></a>Osservazioni

No.

## <a name="examples"></a>Esempi

Stabilire una connessione a un dominio valido, ad esempio Microsoft utilizzando il sottocomando /mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando la connessione ha esito positivo, verrà visualizzato un nuovo TGT o un TGT esistenti verranno aggiornati.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)