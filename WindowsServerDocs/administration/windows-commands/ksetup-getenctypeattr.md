---
title: 'che Ksetup: getenctypeattr'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8363113d4fbb310d98b40d852b36a00f20320e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724630"
---
# <a name="ksetupgetenctypeattr"></a>che Ksetup: getenctypeattr



Recupera l'attributo di tipo di crittografia per il dominio.

## <a name="syntax"></a>Sintassi

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NomeDominio>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso.|

## <a name="remarks"></a>Osservazioni

Per visualizzare il tipo di crittografia per Kerberos ticket di concessione ticket (TGT) e la chiave di sessione, eseguire il **klist** comando e visualizzare l'output.

Se il comando ha esito positivo o negativo, viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

Per impostare il dominio a cui si desidera connettersi e usare, eseguire il comando **che Ksetup/domain \<DomainName>** .

## <a name="examples"></a>Esempi

Verificare che l'attributo di tipo di crittografia per il dominio:
```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)