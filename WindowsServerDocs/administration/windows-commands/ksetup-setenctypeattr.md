---
title: ksetup:setenctypeattr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a91539ec7a9e0ce4c75d5165da1b88ae36d3fe6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879202"
---
# <a name="ksetupsetenctypeattr"></a>ksetup:setenctypeattr



Imposta l'attributo di tipo di crittografia per il dominio. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DomainName>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso.|
|Tipo di crittografia|Deve essere uno dei tipi di crittografia supportati seguenti:</br>-   DES-CBC-CRC</br>-   DES-CBC-MD5</br>-   RC4-HMAC-MD5</br>-   AES128-CTS-HMAC-SHA1-96</br>-   AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Note

Per visualizzare il tipo di crittografia per Kerberos ticket di concessione ticket (TGT) e la chiave di sessione, eseguire il **klist** comando e visualizzare l'output.

È possibile impostare o aggiungere più tipi di crittografia, separare i tipi di crittografia nel comando con uno spazio. Tuttavia, è possibile solo farlo per un dominio alla volta.

Se il comando ha esito positivo o negativo, viene visualizzato un messaggio di stato.

Per impostare il dominio che si desidera connettersi e usare, eseguire la **che ksetup /domain \<NomeDominio >** comando.

## <a name="BKMK_Examples"></a>Esempi

Determinare i tipi di crittografia corrente che sono impostati su questo computer:
```
klist
```
Impostare il dominio corp.contoso.com:
```
ksetup /domain corp.contoso.com
```
Impostare l'attributo di tipo di crittografia su AES-256-CTS-HMAC-SHA1-96 per il dominio corp.contoso.com:
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Verificare che l'attributo di tipo di crittografia è stata impostata nel modo previsto per il dominio:
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)