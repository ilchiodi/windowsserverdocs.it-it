---
title: ksetup:getenctypeattr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 302f94616f98eb350332b08ad37a58305a0a0be1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841992"
---
# <a name="ksetupgetenctypeattr"></a>ksetup:getenctypeattr



Recupera l'attributo di tipo di crittografia per il dominio. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DomainName>|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio corp.contoso.com o contoso.|

## <a name="remarks"></a>Note

Per visualizzare il tipo di crittografia per Kerberos ticket di concessione ticket (TGT) e la chiave di sessione, eseguire il **klist** comando e visualizzare l'output.

Se il comando ha esito positivo o negativo, viene visualizzato un messaggio di stato al completamento riuscito o non riuscito.

Per impostare il dominio che si desidera connettersi e usare, eseguire la **che ksetup /domain \<NomeDominio >** comando.

## <a name="BKMK_Examples"></a>Esempi

Verificare che l'attributo di tipo di crittografia per il dominio:
```
ksetup /getenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)