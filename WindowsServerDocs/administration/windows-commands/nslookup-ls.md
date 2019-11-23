---
title: nslookup ls
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ecc419a72599b661865af6283821129a7021938a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373095"
---
# <a name="nslookup-ls"></a>nslookup ls

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca le informazioni per un dominio Domain Name System (DNS).
## <a name="syntax"></a>Sintassi
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | Nella tabella seguente sono elencate le opzioni valide.<br /><br />--t: elenca tutti i record del tipo specificato. Per una descrizione di <querytype>, vedere **setquerytype** in riferimenti aggiuntivi.<br />--a: elenca gli alias dei computer nel dominio DNS. Questo parametro è un sinonimo di **-t CNAME**<br />--d: elenca tutti i record per il dominio DNS. Questo parametro è un sinonimo di **-t any**<br />--h: elenca le informazioni sulla CPU e sul sistema operativo per il dominio DNS. Questo parametro è un sinonimo di **-t HINFO**<br />--s: elenca i servizi noti dei computer nel dominio DNS. Questo parametro è un sinonimo di **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Specifica il dominio DNS per cui si desidera ottenere informazioni.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Specifica un nome file in cui salvare l'output. Per reindirizzare l'output nel modo consueto, è possibile utilizzare i caratteri maggiore di (>) e doppio maggiore di (> >).                                                                                                                                                                                                                                  |
| {Help &#124; ?} |                                                                                                                                                                                                                                                                                          Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Osservazioni
- L'output predefinito contiene i nomi dei computer e i relativi indirizzi IP. Quando l'output viene indirizzato a un file, vengono stampati i contrassegni hash per ogni 50 record ricevuti dal server
  ## <a name="additional-references"></a>riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set querytype](nslookup-set-querytype.md)
