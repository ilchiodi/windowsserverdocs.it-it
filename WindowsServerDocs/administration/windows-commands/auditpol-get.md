---
title: get auditpol
description: Argomento i comandi di Windows per **auditpol ottenere** -recupera il criterio di sistema, dei criteri per ogni utente, il controllo delle opzioni e oggetto descrittore di sicurezza di controllo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba59b19af42ab2d3fdd1dd52d976d381779640
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435139"
---
# <a name="auditpol-get"></a>get auditpol

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera i criteri di sistema, dei criteri per ogni utente, il controllo delle opzioni e oggetto descrittore di sicurezza di controllo.

## <a name="syntax"></a>Sintassi
```
auditpol /get 
[/user[:<username>|<{sid}>]]
[/category:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/subcategory:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/option:<option name>]
[/sd]
[/r]
```
## <a name="parameters"></a>Parametri

|  Parametro   |                                                                                                                                         Descrizione                                                                                                                                          |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /User     | Consente di visualizzare l'entità di sicurezza per il quale viene eseguita una query il criterio di controllo per utente. Parametro /category o /subcategory deve essere specificato. L'utente può essere specificato come un ID di sicurezza (SID) o un nome. Se non viene specificato alcun account utente, viene eseguita una query il criterio di controllo di sistema. |
|  /category   |                                                          Uno o più categorie di controllo specificate dall'identificatore univoco globale (GUID) o nome. Un asterisco (\*) può essere utilizzato per indicare che tutte le categorie di controllo devono sottoporre a query.                                                          |
| /subcategory |                                                                                                                  Uno o più le sottocategorie di controllo specificate dal GUID o nome.                                                                                                                  |
|     /SD      |                                                                                                        Recupera il descrittore di sicurezza usato per delegare l'accesso per i criteri di controllo.                                                                                                        |
|   /opzione    |                                                                              Recupera i criteri esistenti per le opzioni CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories.                                                                               |
|      /r      |                                                                                                              Visualizza l'output in formato di report, valori delimitati da virgole (CSV).                                                                                                              |
|      /?      |                                                                                                                             Visualizza la guida al prompt dei comandi.                                                                                                                             |

## <a name="remarks"></a>Note
Tutte le categorie e sottocategorie possono essere specificate dal GUID o nome racchiuso tra virgolette. Gli utenti possono essere specificati dal nome o SID.
per tutte le operazioni get per il criterio per ogni utente e criteri di sistema, è necessario disporre di autorizzazioni per tale oggetto impostato nel descrittore di sicurezza lettura. È anche possibile eseguire operazioni get che presenta il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione get.
## <a name="BKMK_examples"></a>Esempi
### <a name="examples-for-the-per-user-audit-policy"></a>Esempi per i criteri di controllo per utente
Per recuperare i criteri di controllo per utente per l'account Guest e visualizzare l'output per il sistema di rilevamento dettagliate e le categorie di accesso agli oggetti, digitare:
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> Questo comando è utile in due scenari. Quando si monitora un account utente specifico per eventuali attività sospette, è possibile utilizzare il comando/get per recuperare i risultati in categorie specifiche usando criteri di inclusione per abilitare il controllo aggiuntivo. In alternativa, se le impostazioni di controllo in un account esegue l'accesso numerose ma gli eventi superflui, è possibile usare il comando/get per filtrare gli eventi estranei per l'account con i criteri di esclusione. Per un elenco di tutte le categorie, il comando auditpol /list /category.
> Per recuperare i criteri di controllo per utente per una categoria e una particolare sottocategoria, che indica le impostazioni inclusivi ed esclusive per tale sottocategoria sotto la categoria di sistema per l'account Guest, digitare:
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Per visualizzare l'output in formato di report e includere il nome del computer, destinazione dei criteri, subcategory, subcategory, GUID, le impostazioni di inclusione e impostazioni di esclusione, digitare:
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>Esempi per i criteri di controllo di sistema
> Per recuperare i criteri per la categoria di sistema e le sottocategorie, che indica le impostazioni dei criteri di categoria e sottocategoria per i criteri di controllo di sistema, digitare:
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Per recuperare i criteri per la categoria di rilevamento dettagliata e le sottocategorie in formato di report e includere il nome del computer, destinazione dei criteri, subcategory, subcategory GUID, le impostazioni di inclusione e impostazioni di esclusione, il tipo:
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> Per recuperare i criteri per due categorie con le categorie specificato come GUID, che segnala tutte le impostazioni dei criteri di controllo di tutte le sottocategorie in due categorie, tipo:
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>Esempi per le opzioni di controllo
> Per recuperare lo stato, abilitata o disabilitata, dell'opzione AuditBaseObjects, digitare:
> ```
> auditpol /get /option:AuditBaseObjects
> ```
> [!NOTE]
> Le opzioni disponibili sono AuditBaseObjects AuditBaseOperations e FullprivilegeAuditing.
> Per recuperare lo stato abilitato, disabilitato o 2 dell'opzione CrashOnAuditFail, digitare:
> ```
> auditpol /get /option:CrashOnAuditFail /r
> ```
> #### <a name="additional-references"></a>Riferimenti aggiuntivi
> [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
