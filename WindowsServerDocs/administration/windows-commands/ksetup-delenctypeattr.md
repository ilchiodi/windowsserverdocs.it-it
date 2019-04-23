---
title: ksetup:delenctypeattr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c2cc96e8156cafd3846422596abe62513e275b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838132"
---
# <a name="ksetupdelenctypeattr"></a>ksetup:delenctypeattr



Rimuove l'attributo di tipo di crittografia per il dominio. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DomainName>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso.|

## <a name="remarks"></a>Note

Per visualizzare il tipo di crittografia per Kerberos ticket di concessione ticket (TGT) e la chiave di sessione, eseguire il **klist** comando e visualizzare l'output.

Viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

Per impostare il dominio che si desidera connettersi e usare, eseguire la **che ksetup /domain \<NomeDominio >** comando.

## <a name="BKMK_Examples"></a>Esempi

Determinare i tipi di crittografia corrente che sono impostati su questo computer:
```
klist
```
Impostare il dominio mit.contoso.com:
```
ksetup /domain mit.contoso.com
```
Verificare che cos'è l'attributo di tipo di crittografia per il dominio:
```
ksetup /getenctypeattr mit.contoso.com
```
Rimuovere l'attributo di tipo set di crittografia per il dominio mit.contoso.com:
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)