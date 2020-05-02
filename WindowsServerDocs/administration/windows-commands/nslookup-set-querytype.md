---
title: nslookup set querytype
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc992d83de8537c285b6d2d97e5f44545e2f930f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723600"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il tipo di record di risorse per la query.
## <a name="syntax"></a>Sintassi
```
set querytype=<ResourceRecordtype>
```
### <a name="parameters"></a>Parametri
<ResourceRecordtype>Specifica un tipo di record di risorse DNS. Il tipo di record di risorse predefinito è. Nella tabella seguente sono elencati i valori validi per questo comando.

| valore |                                                   Descrizione                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   Una   |                                      Specifica un indirizzo IP&#39;s del computer                                      |
|  ANY  |                                     Specifica un indirizzo IP&#39;s del computer.                                      |
| CNAME |                                    Specifica un nome canonico per un alias.                                     |
|  GID  |                                  Specifica un identificatore di gruppo di un nome di gruppo.                                  |
| HINFO |                          Specifica un computer&#39;s CPU e il tipo di sistema operativo.                           |
|  MB   |                                        Specifica un nome di dominio della cassetta postale.                                         |
|  MG   |                                         Specifica un membro del gruppo di posta elettronica.                                          |
| MINFO |                                   Specifica le informazioni sull'elenco di cassette postali o di posta                                   |
|  MR   |                                     Specifica il nome di dominio per la ridenominazione della posta.                                      |
|  MX   |                                          Specifica lo scambiatore di posta elettronica.                                          |
|  NS   |                                 Specifica un server dei nomi DNS per la zona denominata.                                 |
|  PTR  | Specifica un nome di computer se la query è un indirizzo IP; in caso contrario, specifica il puntatore ad altre informazioni. |
|  SOA  |                                Specifica l'inizio dell'autorità per una zona DNS.                                 |
|  TXT  |                                         Specifica le informazioni di testo.                                         |
|  UID  |                                         Specifica l'identificatore utente.                                          |
| UINFO |                                         Specifica le informazioni utente.                                         |
|  WKS  |                                         Descrive un servizio noto.                                         |
| {Guida |                                                       ?}                                                        |

Visualizza un breve riepilogo dei sottocomandi <strong>nslookup</strong>
## <a name="remarks"></a>Osservazioni
- Il comando <strong>set Type</strong> esegue la stessa funzione del comando <strong>set querytype</strong> .
- Per ulteriori informazioni sui tipi di record di risorse, vedere Request for Comment (RFC) 1035.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  <a href = Command-line-syntax-key.md data-RAW-Source =- [chiave sintassi della riga](command-line-syntax-key.md) di comando>chiave</a> sintassi della riga di comando <a href = nslookup-set-Type.MD data-RAW-Source =[nslookup tipo](nslookup-set-type.md) Set>nslookup set Type</a>
