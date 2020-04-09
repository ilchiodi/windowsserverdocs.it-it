---
title: 'che Ksetup: delkpasswd'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b849265e6036f338413b75fe1da2067e4cdb4cd8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841654"
---
# <a name="ksetupdelkpasswd"></a>che Ksetup: delkpasswd

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un server di password Kerberos (kpasswd) per un'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).
## <a name="syntax"></a>Sintassi
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
#### <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                   Descrizione                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e viene indicato il valore predefinito dell'area di autenticazione o dell'area di AUTENTICAZIONE = quando **che ksetup** viene eseguito.                                |
| <KpasswdName> | Il nome da utilizzare come server delle password Kerberos KDC è indicato come nome di dominio completo, tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC. |

## <a name="remarks"></a>Note
Eseguire il comando **che ksetup** per verificare il nome KDC. Se **kpasswd =** non viene visualizzato nell'output, quindi il mapping non è stato configurato. Verranno elencati i mapping di più, se impostata.
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
verificare la CORP dell'area di autenticazione. CONTOSO.COM utilizza il server KDC non Windows mitkdc.contoso.com come server delle password:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Per verificare il comando ha lavorato come previsto, eseguire **che ksetup** nel computer Windows per verificare l'area di autenticazione CORP. CONTOSO.COM non viene mappato a un server di password (il nome KDC) Kerberos.
## <a name="additional-references"></a>Altre informazioni di riferimento
-   [ksetup](ksetup.md)
-   [che Ksetup: delkpasswd](ksetup-delkpasswd.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
