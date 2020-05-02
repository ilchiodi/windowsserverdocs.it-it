---
title: 'che Ksetup: delkpasswd'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2de1546b112041f7035a711852140e9bb34babe3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724664"
---
# <a name="ksetupdelkpasswd"></a>che Ksetup: delkpasswd

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un server di password Kerberos (kpasswd) per un'area di autenticazione.
## <a name="syntax"></a>Sintassi
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
#### <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                   Descrizione                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                Il nome dell'area di autenticazione è indicato come nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM ed è elencato come area di autenticazione predefinita o REALM = quando viene eseguito **che Ksetup** .                                |
| <KpasswdName> | Il nome da utilizzare come server delle password Kerberos KDC è indicato come nome di dominio completo, tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC. |

## <a name="remarks"></a>Osservazioni
Eseguire il comando **che ksetup** per verificare il nome KDC. Se **kpasswd =** non viene visualizzato nell'output, quindi il mapping non è stato configurato. Verranno elencati i mapping di più, se impostata.
## <a name="examples"></a>Esempi
verificare la CORP dell'area di autenticazione. CONTOSO.COM utilizza il server KDC non Windows mitkdc.contoso.com come server delle password:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Per verificare il comando ha lavorato come previsto, eseguire **che ksetup** nel computer Windows per verificare l'area di autenticazione CORP. CONTOSO.COM non viene mappato a un server di password (il nome KDC) Kerberos.
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [ksetup](ksetup.md)
-   [che Ksetup: delkpasswd](ksetup-delkpasswd.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
