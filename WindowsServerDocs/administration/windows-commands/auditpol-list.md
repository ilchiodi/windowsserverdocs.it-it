---
title: elenco di auditpol
description: 'Argomento i comandi di Windows per **auditpol elenco** : gli elenchi di controllo categorie di criteri e/o sottocategorie o vengono elencati gli utenti per i quali un utente per ogni criterio di controllo è definito.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08f524ef0aacd731f709ce7a2e17b3d831da1e5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858582"
---
# <a name="auditpol-list"></a>elenco di auditpol

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

gli elenchi di controllo vengono elencati gli utenti per i quali è definito un criterio di controllo per utente e/o sottocategorie o categorie di criteri.

## <a name="syntax"></a>Sintassi
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/User|Recupera tutti gli utenti per i quali è stato definito il criterio di controllo per utente. Se usato con il parametro /v, viene visualizzato anche l'ID di sicurezza (SID) dell'utente.|
|/category|Visualizza i nomi delle categorie riconosciute dal sistema. Se usato con il parametro /v, viene visualizzato anche l'identificatore univoco globale (GUID) di categoria.|
|/subcategory|Consente di visualizzare i nomi delle sottocategorie e i rispettivi GUID associati.|
|/v|Consente di visualizzare il GUID con la categoria o sottocategoria, o se usato con /user, il SID di ogni utente.|
|/r|Visualizza l'output come un report in formato con valori delimitati da virgole (CSV).|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
per tutte le operazioni di elenco per i criteri per ogni utente, è necessario disporre di autorizzazioni per tale oggetto impostato nel descrittore di sicurezza lettura. È anche possibile eseguire operazioni di elenco da cui appartiene il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione di elenco.
## <a name="BKMK_examples"></a>Esempi
Per elencare tutti gli utenti che dispongono di un criterio di controllo definito, digitare:
```
auditpol /list /user
```
Per elencare tutti gli utenti che dispongono di un criterio di controllo definito e il SID associato, digitare:
```
auditpol /list /user /v
```
Per elencare tutte le categorie e sottocategorie in formato di report, digitare:
```
auditpol /list /subcategory:* /r
```
Per visualizzare un elenco di sottocategorie delle categorie di accesso di dominio Active Directory e il rilevamento dettagliate, digitare:
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
