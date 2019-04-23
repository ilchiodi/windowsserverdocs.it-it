---
title: ksetup:domain
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1f53e807891b434709b1a8faed7aae8e8d444f6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857452"
---
# <a name="ksetupdomain"></a>ksetup:domain



Imposta il nome di dominio per tutte le operazioni di Kerberos. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DomainName>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio contoso.com o contoso.|

## <a name="remarks"></a>Note

Nessuno.

## <a name="BKMK_Examples"></a>Esempi

Stabilire una connessione a un dominio valido, ad esempio Microsoft utilizzando il sottocomando /mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando la connessione ha esito positivo, verrà visualizzato un nuovo TGT o un TGT esistenti verranno aggiornati.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup](ksetup.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)