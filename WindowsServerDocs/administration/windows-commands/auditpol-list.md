---
title: elenco Auditpol
description: Argomento di riferimento per il comando auditpol list, che elenca le categorie e le sottocategorie dei criteri di controllo, oppure elenca gli utenti per i quali è definito un criterio di controllo per utente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96ee4388c716c066a2e9b55b57dd2e70b4b4f69c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719092"
---
# <a name="auditpol-list"></a>elenco Auditpol

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca le categorie e le sottocategorie dei criteri di controllo oppure elenca gli utenti per i quali è definito un criterio di controllo per utente.

Per eseguire operazioni di *elenco* sul criterio *per utente* , è necessario disporre dell'autorizzazione di **lettura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di *elenco* se si dispone del diritto utente **Gestisci registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire le operazioni di *elenco* globale.

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

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comandi auditpol](auditpol.md)
