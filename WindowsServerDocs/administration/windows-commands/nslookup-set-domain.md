---
title: nslookup set domain
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6ee52cd5ad35dcfc6da1cd3885f66124338d62f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723612"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio predefinito del Domain Name System (DNS) con il nome specificato.
## <a name="syntax"></a>Sintassi
```
set domain=<DomainName>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica un nuovo nome per il nome di dominio DNS predefinito. Il nome di dominio predefinito è il nome host. |
| {Help &#124;?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                      |

## <a name="remarks"></a>Osservazioni
- Il nome di dominio DNS predefinito viene aggiunto a una richiesta di ricerca a seconda dello stato delle opzioni di **ricerca** e di **defname** . L'elenco di ricerca del dominio DNS contiene gli elementi padre del dominio DNS predefinito se ha almeno due componenti nel nome. Ad esempio, se il dominio DNS predefinito è mfg.widgets.com, l'elenco di ricerca viene denominato sia mfg.widgets.com che widgets.com. Usare il comando **set srchlist** per specificare un elenco diverso e il comando **set all** per visualizzare l'elenco.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup set srchlist](nslookup-set-srchlist.md)
  [nslookup set all](nslookup-set-all.md)
