---
title: elenco Auditpol
description: Windows Commands Topic for **auditpol list**, che elenca le categorie e le sottocategorie dei criteri di controllo, oppure elenca gli utenti per i quali è definito un criterio di controllo per utente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e0aff46e62ea4e4259360b78aae223dfcd66ef7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851184"
---
# <a name="auditpol-list"></a>elenco Auditpol

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca le categorie e/o le sottocategorie dei criteri di controllo oppure elenca gli utenti per i quali è definito un criterio di controllo per utente.

## <a name="syntax"></a>Sintassi

```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /User | Recupera tutti gli utenti per i quali è stato definito il criterio di controllo per utente. Se usato con il parametro/v, viene visualizzato anche l'ID di sicurezza (SID) dell'utente. |
| /category | Visualizza i nomi delle categorie riconosciute dal sistema. Se usato con il parametro/v, viene visualizzato anche l'identificatore univoco globale (GUID) Category. |
| /Subcategory | Visualizza i nomi delle sottocategorie e il GUID associato. |
| /v | Visualizza il GUID con la categoria o la sottocategoria oppure, se usato con/User, Visualizza il SID di ogni utente. |
| /r | Visualizza l'output come report in formato CSV (delimitato da virgole). |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note

Per tutte le operazioni di elenco per i criteri per utente, è necessario disporre dell'autorizzazione di lettura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di elenco con il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di elenco.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per elencare tutti gli utenti che dispongono di un criterio di controllo definito, digitare:

```
auditpol /list /user
```

Per elencare tutti gli utenti che dispongono di un criterio di controllo definito e il SID associato, digitare:

```
auditpol /list /user /v
```

Per elencare tutte le categorie e le sottocategorie in formato report, digitare:

```
auditpol /list /subcategory:* /r
```

Per elencare le sottocategorie delle categorie di rilevamento dettagliate e di accesso DS, digitare:

```
auditpol /list /subcategory:detailed Tracking,DS Access
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
