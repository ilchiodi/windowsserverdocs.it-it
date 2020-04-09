---
title: nslookup set dominio
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa433383e23fd19779960348e0af88e0a405ff83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838464"
---
# <a name="nslookup-set-domain"></a>nslookup set dominio

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio predefinito del Domain Name System (DNS) con il nome specificato.
## <a name="syntax"></a>Sintassi
```
set domain=<DomainName>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica un nuovo nome per il nome di dominio DNS predefinito. Il nome di dominio predefinito è il nome host. |
| {Help &#124; ?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                      |

## <a name="remarks"></a>Note
- Il nome di dominio DNS predefinito viene aggiunto a una richiesta di ricerca a seconda dello stato delle opzioni di **ricerca** e di **defname** . L'elenco di ricerca del dominio DNS contiene gli elementi padre del dominio DNS predefinito se ha almeno due componenti nel nome. Ad esempio, se il dominio DNS predefinito è mfg.widgets.com, l'elenco di ricerca viene denominato sia mfg.widgets.com che widgets.com. Usare il comando **set srchlist** per specificare un elenco diverso e il comando **set all** per visualizzare l'elenco.
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set srchlist](nslookup-set-srchlist.md)
  [nslookup set all](nslookup-set-all.md)
