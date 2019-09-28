---
title: 'che Ksetup: dominio'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d9f09def32c7518046c25887f4154020c5d7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375123"
---
# <a name="ksetupdomain"></a>che Ksetup: dominio



Imposta il nome di dominio per tutte le operazioni di Kerberos. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DomainName >|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio contoso.com o contoso.|

## <a name="remarks"></a>Note

No.

## <a name="BKMK_Examples"></a>Esempi

Stabilire una connessione a un dominio valido, ad esempio Microsoft utilizzando il sottocomando /mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando la connessione ha esito positivo, verrà visualizzato un nuovo TGT o un TGT esistenti verranno aggiornati.

#### <a name="additional-references"></a>Altri riferimenti

-   [Che Ksetup](ksetup.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)