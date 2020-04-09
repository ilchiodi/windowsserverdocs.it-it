---
title: nslookup set srchlist
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbeb09501474ade670147a6021abd2bb25291d71
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838284"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio e l'elenco di ricerca predefiniti Domain Name System (DNS).

## <a name="syntax"></a>Sintassi
```
Set srchlist=<DomainName>[/...]
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                        Descrizione                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica i nuovi nomi per il dominio DNS predefinito e l'elenco di ricerca. Il valore del nome di dominio predefinito è basato sul nome host. È possibile specificare un massimo di sei nomi separati da barre (/). |
| {Help &#124; ?} |                                                                   Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                   |

## <a name="remarks"></a>Note
- Il comando **set srchlist**sostituisce il nome di dominio DNS predefinito e l'elenco di ricerca del comando **imposta dominio** . Usare il comando **imposta tutto** per visualizzare l'elenco.
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Nell'esempio seguente il dominio DNS viene impostato su mfg.widgets.com e l'elenco di ricerca viene impostato sui tre nomi:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set Domain](nslookup-set-domain.md)
  [nslookup set all](nslookup-set-all.md)
