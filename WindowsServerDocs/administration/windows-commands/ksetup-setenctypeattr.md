---
title: 'che Ksetup: setenctypeattr'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cb7380a5fc65734902c6eed0b4b941eda6f6f5a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724565"
---
# <a name="ksetupsetenctypeattr"></a>che Ksetup: setenctypeattr



Imposta l'attributo del tipo di crittografia per il dominio.

## <a name="syntax"></a>Sintassi

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NomeDominio>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso.|
|Tipo di crittografia|Deve essere uno dei tipi di crittografia supportati seguenti:</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Osservazioni

Per visualizzare il tipo di crittografia per Kerberos ticket di concessione ticket (TGT) e la chiave di sessione, eseguire il **klist** comando e visualizzare l'output.

È possibile impostare o aggiungere più tipi di crittografia separando i tipi di crittografia nel comando con uno spazio. Tuttavia, è possibile eseguire questa operazione solo per un dominio alla volta.

Se il comando ha esito positivo o negativo, viene visualizzato un messaggio di stato.

Per impostare il dominio a cui si desidera connettersi e usare, eseguire il comando **che Ksetup/domain \<DomainName>** .

## <a name="examples"></a>Esempi

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
Verificare che l'attributo del tipo di crittografia sia impostato come previsto per il dominio:
```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)