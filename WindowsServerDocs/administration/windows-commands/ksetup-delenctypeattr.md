---
title: 'che Ksetup: delenctypeattr'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2908cc0a095a6985c11f7885766926b7f0354ab0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724708"
---
# <a name="ksetupdelenctypeattr"></a>che Ksetup: delenctypeattr



Rimuove l'attributo di tipo di crittografia per il dominio.

## <a name="syntax"></a>Sintassi

```
ksetup /delenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NomeDominio>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso.|

## <a name="remarks"></a>Osservazioni

Per visualizzare il tipo di crittografia per Kerberos ticket di concessione ticket (TGT) e la chiave di sessione, eseguire il **klist** comando e visualizzare l'output.

Viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

Per impostare il dominio a cui si desidera connettersi e usare, eseguire il comando **che Ksetup/domain \<DomainName>** .

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)