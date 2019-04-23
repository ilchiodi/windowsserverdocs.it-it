---
title: ksetup:delkpasswd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e487389e0f27f58aacaea2b81d573dc9a965e42f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889082"
---
# <a name="ksetupdelkpasswd"></a>ksetup:delkpasswd

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un server di password Kerberos (Kpasswd) per un'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).
## <a name="syntax"></a>Sintassi
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<RealmName>|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e viene indicato il valore predefinito dell'area di autenticazione o dell'area di AUTENTICAZIONE = quando **che ksetup** viene eseguito.|
|<KpasswdName>|Il nome da utilizzare come server delle password Kerberos KDC è indicato come nome di dominio completo, tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC.|
## <a name="remarks"></a>Note
Eseguire il comando **che ksetup** per verificare il nome KDC. Se **kpasswd =** non viene visualizzato nell'output, quindi il mapping non è stato configurato. Verranno elencati i mapping di più, se impostata.
## <a name="BKMK_Examples"></a>Esempi
Verificare l'area di autenticazione CORP. CONTOSO.COM utilizza il mitkdc.contoso.com server KDC non Windows come server di password:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Per verificare il comando ha lavorato come previsto, eseguire **che ksetup** nel computer Windows per verificare l'area di autenticazione CORP. CONTOSO.COM non viene mappato a un server di password (il nome KDC) Kerberos.
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [ksetup](ksetup.md)
-   [ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
