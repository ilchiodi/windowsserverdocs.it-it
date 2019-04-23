---
title: ksetup:mapuser
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2828f92b20cafcb571c81c8ceae28c741fbe025a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872862"
---
# <a name="ksetupmapuser"></a>ksetup:mapuser



Il nome dell'entità Kerberos viene eseguito il mapping a un account. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Principal>|Il nome di dominio completo di qualsiasi entità; ad esempio, mike@corp.CONTOSO.COM.|
|\<Account>|Qualsiasi account o sicurezza gruppo nome presente in questo computer, ad esempio Guest, gli utenti del dominio o amministratore.|

## <a name="remarks"></a>Note

Un account può essere specificamente identificato, ad esempio Guest del dominio. Oppure è possibile utilizzare il carattere jolly (*) per includere tutti gli account.

Se il nome dell'account viene omesso, viene eliminato il mapping per l'entità specificata.

Il computer solo eseguire l'autenticazione di entità dell'area di autenticazione specifico che presentano i ticket Kerberos validi

Uso **che ksetup** senza parametri o argomenti corrente eseguito il mapping le impostazioni e l'area di autenticazione predefinito.

Quando vengono apportate modifiche per il centro di distribuzione chiavi (KDC) esterna e la configurazione dell'area di autenticazione, è necessario un riavvio del computer in cui è stata modificata l'impostazione.

## <a name="BKMK_Examples"></a>Esempi

Eseguire il mapping di account di Mike Danseglio all'interno di CONTOSO dell'area di autenticazione Kerberos per l'account guest su questo computer, senza dover eseguire l'autenticazione per questo computer concede tutti i privilegi di un membro dell'account Guest predefinito:
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Rimuovere il mapping dell'account di Mike Danseglio per l'account guest su questo computer per impedire l'autenticazione al computer con le credenziali da CONTOSO:
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Eseguire il mapping account di Mike Danseglio area di autenticazione Kerberos di CONTOSO a qualsiasi account esistente nel computer. (se l'utente standard e gli account guest sono attivi nel computer, solo privilegi di Mike verranno impostati su quelle):
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Eseguire il mapping di tutti gli account entro l'area di autenticazione Kerberos di CONTOSO a qualsiasi account esistente con lo stesso nome nel computer in uso:
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)