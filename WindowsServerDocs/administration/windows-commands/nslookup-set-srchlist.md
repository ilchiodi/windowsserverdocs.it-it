---
title: nslookup set srchlist
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8936daa3505b02295ae2f09c2910dead8d4c0ff8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723558"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio e l'elenco di ricerca predefiniti Domain Name System (DNS).

## <a name="syntax"></a>Sintassi
```
Set srchlist=<DomainName>[/...]
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                        Descrizione                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica i nuovi nomi per il dominio DNS predefinito e l'elenco di ricerca. Il valore del nome di dominio predefinito è basato sul nome host. È possibile specificare un massimo di sei nomi separati da barre (/). |
| {Help &#124;?} |                                                                   Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                   |

## <a name="remarks"></a>Osservazioni
- Il comando **set srchlist**sostituisce il nome di dominio DNS predefinito e l'elenco di ricerca del comando **imposta dominio** . Usare il comando **imposta tutto** per visualizzare l'elenco.
  ## <a name="examples"></a>Esempi
  Per impostare il dominio DNS su mfg.widgets.com e l'elenco di ricerca sui tre nomi:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup set Domain](nslookup-set-domain.md)
  [nslookup set all](nslookup-set-all.md)
