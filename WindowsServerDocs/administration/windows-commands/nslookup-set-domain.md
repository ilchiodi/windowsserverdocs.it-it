---
title: nslookup set dominio
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d1af9f30dd2c44111adecb477a6469333f4f7685
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436773"
---
# <a name="nslookup-set-domain"></a>nslookup set dominio

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il nome di dominio Domain Name System (DNS) predefinito per il nome specificato.
## <a name="syntax"></a>Sintassi
```
set domain=<DomainName>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Specifica un nuovo nome per il nome di dominio DNS predefinito. Il nome di dominio predefinito è il nome host. |
| {help &#124; ?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                      |

## <a name="remarks"></a>Note
- Il nome di dominio DNS predefinito viene aggiunto a una richiesta di ricerca a seconda dello stato del **defname** e **ricerca** opzioni. L'elenco di ricerca di dominio DNS contiene gli elementi padre del dominio DNS predefinito se è costituito da almeno due componenti nel nome. Ad esempio, se il dominio DNS predefinito costr.carrega.com, l'elenco di ricerca è denominato costr.carrega.com sia carrega.com. Usare il **impostare srchlist** comando per specificare un elenco diverso e il **imposta tutti** comando per visualizzare l'elenco.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup impostare srchlist](nslookup-set-srchlist.md)
  [tutti i set nslookup](nslookup-set-all.md)
