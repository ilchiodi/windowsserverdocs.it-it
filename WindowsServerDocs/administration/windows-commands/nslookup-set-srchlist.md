---
title: nslookup set srchlist
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb93a9f7cf969161536e88bec929b7e6ba0f0e5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372775"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio e l'elenco di ricerca predefiniti Domain Name System (DNS).

## <a name="syntax"></a>Sintassi
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                                                                        Descrizione                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica i nuovi nomi per il dominio DNS predefinito e l'elenco di ricerca. Il valore del nome di dominio predefinito è basato sul nome host. È possibile specificare un massimo di sei nomi separati da barre (/). |
| {Help &#124; ?} |                                                                   Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                   |

## <a name="remarks"></a>Note
- Il comando **set srchlist**sostituisce il nome di dominio DNS predefinito e l'elenco di ricerca del comando **imposta dominio** . Usare il comando **imposta tutto** per visualizzare l'elenco.
  ## <a name="BKMK_examples"></a>Esempi
  Nell'esempio seguente il dominio DNS viene impostato su mfg.widgets.com e l'elenco di ricerca viene impostato sui tre nomi:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set Domain](nslookup-set-domain.md)
  [nslookup set all](nslookup-set-all.md)
