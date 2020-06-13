---
title: nslookup set domain
description: Argomento di riferimento per il comando nslookup set Domain, che modifica il nome di dominio predefinito Domain Name System (DNS) con il nome specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e672bf53e655ef12cadb2a30aaa377b24e49afec
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721634"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome di dominio predefinito del Domain Name System (DNS) con il nome specificato.

## <a name="syntax"></a>Sintassi

```
set domain=<domainname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<domainname>` | Specifica un nuovo nome per il nome di dominio DNS predefinito. Il valore predefinito è il nome dell'host. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Il nome di dominio DNS predefinito viene aggiunto a una richiesta di ricerca a seconda dello stato delle opzioni di **ricerca** e di **defname** .

- L'elenco di ricerca del dominio DNS contiene gli elementi padre del dominio DNS predefinito se ha almeno due componenti nel nome. Ad esempio, se il dominio DNS predefinito è mfg.widgets.com, l'elenco di ricerca viene denominato sia mfg.widgets.com che widgets.com.

- Usare il comando [nslookup set srchlist](nslookup-set-srchlist.md) per specificare un elenco diverso e il comando [nslookup set all](nslookup-set-all.md) per visualizzare l'elenco.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set srchlist](nslookup-set-srchlist.md)

- [nslookup set all](nslookup-set-all.md)
