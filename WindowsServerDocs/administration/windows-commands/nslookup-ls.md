---
title: nslookup ls
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a11867ff2ec69b1ef938149ac485ff8827b58de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436926"
---
# <a name="nslookup-ls"></a>nslookup ls

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni per un dominio di sistema DNS (Domain Name).
## <a name="syntax"></a>Sintassi
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | Nella tabella seguente sono elencati i valori validi.<br /><br />--t: Elenca tutti i record del tipo specificato. Per una descrizione delle <querytype>, vedere **setquerytype** in riferimenti aggiuntivi.<br />--r: Elenca gli alias dei computer nel dominio DNS. Questo parametro è un sinonimo **- t CNAME**<br />--unità d: sono elencati tutti i record per il dominio DNS. Questo parametro è un sinonimo **- t ANY**<br />--h: Elenca le informazioni di CPU e del sistema operativo per il dominio DNS. Questo parametro è un sinonimo **HINFO -t**<br />--s: Elenca i servizi noti di computer del dominio DNS. Questo parametro è un sinonimo **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Specifica il dominio DNS di cui si desiderano informazioni.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Specifica un nome di file in cui salvare l'output. È possibile usare il simbolo di maggiore (>) e double maggiore di (>>) per reindirizzare l'output nel modo usuale.                                                                                                                                                                                                                                  |
| {help &#124; ?} |                                                                                                                                                                                                                                                                                          Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Note
- L'output predefinito contiene nomi di computer e i relativi IP indirizzi. Quando l'output viene indirizzato a un file, hash contrassegni vengono stampate per ogni 50 record ricevuti dal server
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [querytype set nslookup](nslookup-set-querytype.md)
