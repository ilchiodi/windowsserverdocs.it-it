---
title: nslookup set srchlist
description: Argomento di riferimento per il comando nslookup set srchlist, che modifica il nome di dominio e l'elenco di ricerca predefiniti Domain Name System (DNS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed9bbce1910324c4cae5da4228a6d3d1f269d050
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721401"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome di dominio e l'elenco di ricerca predefiniti Domain Name System (DNS). Questo comando sostituisce il nome di dominio DNS predefinito e l'elenco di ricerca del comando [nslookup set Domain](nslookup-set-domain.md) .

## <a name="syntax"></a>Sintassi

```
set srchlist=<domainname>[/...]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<domainname>` | Specifica i nuovi nomi per il dominio DNS predefinito e l'elenco di ricerca. Il valore del nome di dominio predefinito è basato sul nome host. È possibile specificare un massimo di sei nomi separati da barre (/). |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Utilizzare il comando [nslookup imposta tutto](nslookup-set-all.md) per visualizzare l'elenco.

### <a name="examples"></a>Esempio

Per impostare il dominio DNS su *MFG.widgets.com* e l'elenco di ricerca sui tre nomi:

```
set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set domain](nslookup-set-domain.md)

- [nslookup set all](nslookup-set-all.md)
