---
title: auditpol get
description: Argomento di riferimento per il comando auditpol get, che recupera i criteri di sistema, i criteri per utente, le opzioni di controllo e l'oggetto del descrittore di sicurezza del controllo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 859ea9e2e42af0fe7f34f4e378166685f8316b9e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719140"
---
# <a name="auditpol-get"></a>auditpol get

> Si applica a: Windows Server (canale semestrale), Windows Server, 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera i criteri di sistema, i criteri per utente, le opzioni di controllo e l'oggetto descrittore di sicurezza del controllo.

Per eseguire operazioni *Get* sui criteri *per utente* e di *sistema* , è necessario disporre dell'autorizzazione di **lettura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire le operazioni *Get* se si dispone del diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire le operazioni *Get* complessive.

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

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /User | Consente di visualizzare l'entità di sicurezza per la quale vengono eseguite query sui criteri di controllo per utente. È necessario specificare il parametro/category o/Subcategory. L'utente può essere specificato come ID di sicurezza (SID) o nome. Se non viene specificato alcun account utente, viene eseguita una query sui criteri di controllo del sistema. |
| /category | Una o più categorie di controllo specificate da un identificatore univoco globale (GUID) o da un nome. È possibile utilizzare un asterisco (*) per indicare che è necessario eseguire una query su tutte le categorie di controllo. |
| /Subcategory | Una o più sottocategorie di controllo specificate in base al nome o al GUID. |
| /SD | Recupera il descrittore di sicurezza utilizzato per delegare l'accesso ai criteri di controllo. |
| /opzione | Recupera i criteri esistenti per le opzioni CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories. |
| /r | Visualizza l'output in formato report, con valori delimitati da virgole (CSV). |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="remarks"></a>Osservazioni

Tutte le categorie e le sottocategorie possono essere specificate in base al nome o al GUID racchiuso tra virgolette ("). Gli utenti possono essere specificati in base al SID o al nome.

## <a name="examples"></a>Esempi

Per recuperare i criteri di controllo per utente per l'account Guest e visualizzare l'output per le categorie sistema, rilevamento dettagliato e accesso agli oggetti, digitare:

```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:System,detailed Tracking,Object Access
```

> [!NOTE]
> Questo comando è utile in due scenari. 1) quando si esegue il monitoraggio di un account utente specifico per attività sospette `/get` , è possibile utilizzare il comando per recuperare i risultati in categorie specifiche utilizzando un criterio di inclusione per abilitare il controllo aggiuntivo. 2) se le impostazioni di controllo di un account registrano numerosi eventi ma superflui, è `/get` possibile usare il comando per filtrare gli eventi estranei per tale account con criteri di esclusione. Per un elenco di tutte le categorie, utilizzare `auditpol /list /category` il comando.

Per recuperare i criteri di controllo per utente per una categoria e una sottocategoria specifica, che riporta le impostazioni incluse ed esclusive per la sottocategoria nella categoria sistema per l'account Guest, digitare:

```
auditpol /get /user:guest /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Per visualizzare l'output in formato report e includere il nome del computer, la destinazione dei criteri, la sottocategoria, il GUID della sottocategoria, le impostazioni di inclusione e le impostazioni di esclusione, digitare:

```
auditpol /get /user:guest /category:detailed Tracking /r
```

Per recuperare i criteri per la categoria e le sottocategorie di sistema, che segnalano le impostazioni dei criteri di categoria e sottocategoria per i criteri di controllo del sistema, digitare:

```
auditpol /get /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Per recuperare i criteri per la categoria di rilevamento dettagliata e le sottocategorie in formato report e includere il nome del computer, la destinazione dei criteri, la sottocategoria, il GUID della sottocategoria, le impostazioni di inclusione e le impostazioni di esclusione, digitare:

```
auditpol /get /category:detailed Tracking /r
```

Per recuperare i criteri per due categorie con le categorie specificate come GUID, che segnala tutte le impostazioni dei criteri di controllo di tutte le sottocategorie in due categorie, digitare:

```
auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Per recuperare lo stato abilitato o disabilitato dell'opzione AuditBaseObjects, digitare:

```
auditpol /get /option:AuditBaseObjects
```

Dove le opzioni disponibili sono AuditBaseObjects, AuditBaseOperations e FullprivilegeAuditing. Per recuperare lo stato abilitato, disabled o 2 dell'opzione CrashOnAuditFail, digitare:

```
auditpol /get /option:CrashOnAuditFail /r
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comandi auditpol](auditpol.md)
