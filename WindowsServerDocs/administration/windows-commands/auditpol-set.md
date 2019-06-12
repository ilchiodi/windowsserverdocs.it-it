---
title: auditpol set
description: 'Argomento i comandi di Windows per **auditpol set** : imposta il criterio di controllo per utente, criteri di controllo di sistema o le opzioni di controllo.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8778401efb272a167aaa3d9abb4ecafc67e5f50d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435109"
---
# <a name="auditpol-set"></a>auditpol set

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criteri di controllo set singolo utente, criteri di controllo di sistema o le opzioni di controllo.

## <a name="syntax"></a>Sintassi
```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```
## <a name="parameters"></a>Parametri

|  Parametro   |                                                                                                                                          Descrizione                                                                                                                                           |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /User     |                                        L'entità di sicurezza per il quale criterio specificato per la categoria o sottocategoria di controllo per singolo utente è impostato. La categoria o sottocategoria opzione deve essere specificata, come un ID di sicurezza (SID) o un nome.                                         |
|   /include   | Specificato con /user; indica che i criteri per ogni utente dell'utente genererà un controllo deve essere generato anche se non è specificato dal criterio di controllo del sistema. Questa impostazione è il valore predefinito e viene applicata automaticamente se non si specifica il / include né /exclude parametri vengono specificati in modo esplicito. |
|   /exclude   |                                Specificato con /user; indica che i criteri per ogni utente dell'utente genererà un controllo deve essere omesso indipendentemente dai criteri di controllo del sistema. Questa impostazione viene ignorata per gli utenti che sono membri del gruppo Administrators locale.                                |
|  /category   |                                                                            Uno o più categorie di controllo specificate dall'identificatore univoco globale (GUID) o nome. Se viene specificato alcun utente, viene impostato il criterio di sistema.                                                                             |
| /subcategory |                                                                                         Uno o più le sottocategorie di controllo specificate dal GUID o nome. Se viene specificato alcun utente, viene impostato il criterio di sistema.                                                                                          |
|   /success   |                 Specifica il controllo della riuscita. Questa impostazione è l'impostazione predefinita e viene applicata automaticamente se i parametri di /success né /failure vengono specificati in modo esplicito. Questa impostazione deve essere usata con un parametro che indica se abilitare o disabilitare l'impostazione.                 |
|   /failure   |                                                                                  Specifica il controllo di un errore. Questa impostazione deve essere usata con un parametro che indica se abilitare o disabilitare l'impostazione.                                                                                   |
|   /opzione    |                                                                                   Imposta i criteri di controllo per le opzioni CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories.                                                                                    |
|     /SD      |                 Imposta il descrittore di sicurezza usato per delegare l'accesso per i criteri di controllo. Il descrittore di sicurezza deve essere specificato utilizzando il SDDL Security Descriptor Definition Language (). Il descrittore di sicurezza deve avere un elenco di controllo di accesso discrezionale (DACL).                 |
|      /?      |                                                                                                                              Visualizza la guida al prompt dei comandi.                                                                                                                              |

## <a name="remarks"></a>Note
per tutte le operazioni sui set per il criterio per ogni utente e criteri di sistema, è necessario avere scrivere o dell'autorizzazione controllo completo su tale oggetto impostato nel descrittore di sicurezza. È anche possibile eseguire operazioni sui set che presenta il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione di impostazione.
## <a name="BKMK_examples"></a>Esempi
### <a name="examples-for-the-per-user-audit-policy"></a>Esempi per i criteri di controllo per utente
Per impostare l'utente per ogni criterio di controllo per tutte le sottocategorie sotto la categoria di rilevamento dettagliata per l'utente bianchi in modo che tutti i tentativi riusciti dell'utente verranno controllati, digitare:
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
Per impostare i criteri di controllo per utente per le categorie specificato dal nome e GUID e le sottocategorie specificate dal GUID da escludere il controllo per tutti i tentativi riusciti o non, digitare:
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
Per impostare i criteri di controllo per utente per l'utente specificato per tutte le categorie per l'eliminazione del controllo di tutto tranne i tentativi con esito positivo, digitare:
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>Esempi per i criteri di controllo di sistema
Per impostare i criteri di controllo di sistema per tutte le sottocategorie sotto la categoria di rilevamento dettagliata per includere il controllo per i tentativi solo esito positivo, digitare:
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> Le impostazioni di errore non viene modificata.
> Per impostare i criteri di controllo di sistema per le categorie di accesso agli oggetti e di sistema (che è implicita poiché sono elencate le sottocategorie) e le sottocategorie specificate dal GUID per l'eliminazione di tentativi non riusciti e il controllo dei tentativi con esito positivo, digitare:
> ```
> auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
> ```
> ### <a name="example-for-auditing-options"></a>Esempio per le opzioni di controllo
> Per impostare le opzioni di controllo per lo stato abilitato per l'opzione CrashOnAuditFail, digitare:
> ```
> auditpol /set /option:CrashOnAuditFail /value:enable
> ```
> #### <a name="additional-references"></a>Riferimenti aggiuntivi
> [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
