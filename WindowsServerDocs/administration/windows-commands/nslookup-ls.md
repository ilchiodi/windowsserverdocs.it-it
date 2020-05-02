---
title: nslookup ls
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8357208406c0fca5d68da419baa2092d94fa9ec9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723700"
---
# <a name="nslookup-ls"></a>nslookup ls

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca le informazioni per un dominio Domain Name System (DNS).
## <a name="syntax"></a>Sintassi
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | Nella tabella seguente sono elencate le opzioni valide.<p>--t: elenca tutti i record del tipo specificato. Per una descrizione di <querytype>, vedere **setquerytype** in riferimenti aggiuntivi.<br />--a: elenca gli alias dei computer nel dominio DNS. Questo parametro è un sinonimo di **-t CNAME**<br />--d: elenca tutti i record per il dominio DNS. Questo parametro è un sinonimo di **-t any**<br />--h: elenca le informazioni sulla CPU e sul sistema operativo per il dominio DNS. Questo parametro è un sinonimo di **-t HINFO**<br />--s: elenca i servizi noti dei computer nel dominio DNS. Questo parametro è un sinonimo di **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Specifica il dominio DNS per cui si desidera ottenere informazioni.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Specifica un nome file in cui salvare l'output. Per reindirizzare l'output nel modo consueto, è possibile utilizzare i caratteri maggiore di (>) e doppio maggiore di (>>).                                                                                                                                                                                                                                  |
| {Help &#124;?} |                                                                                                                                                                                                                                                                                          Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Osservazioni
- L'output predefinito contiene i nomi dei computer e i relativi indirizzi IP. Quando l'output viene indirizzato a un file, vengono stampati i contrassegni hash per ogni 50 record ricevuti dal server
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set querytype](nslookup-set-querytype.md)
