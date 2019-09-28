---
title: nslookup set dominio
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f140a371a6374baa7921ca823df469156593423c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372930"
---
# <a name="nslookup-set-domain"></a>nslookup set dominio

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio predefinito del Domain Name System (DNS) con il nome specificato.
## <a name="syntax"></a>Sintassi
```
set domain=<DomainName>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica un nuovo nome per il nome di dominio DNS predefinito. Il nome di dominio predefinito è il nome host. |
| {Help &#124; ?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                      |

## <a name="remarks"></a>Note
- Il nome di dominio DNS predefinito viene aggiunto a una richiesta di ricerca a seconda dello stato delle opzioni di **ricerca** e di **defname** . L'elenco di ricerca del dominio DNS contiene gli elementi padre del dominio DNS predefinito se ha almeno due componenti nel nome. Ad esempio, se il dominio DNS predefinito è mfg.widgets.com, l'elenco di ricerca viene denominato sia mfg.widgets.com che widgets.com. Usare il comando **set srchlist** per specificare un elenco diverso e il comando **set all** per visualizzare l'elenco.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set srchlist](nslookup-set-srchlist.md)
  [nslookup set all](nslookup-set-all.md)
