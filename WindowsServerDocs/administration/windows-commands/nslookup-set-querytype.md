---
title: nslookup set querytype
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c066277a22325e5db8383f58cda31038aa656e0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838434"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il tipo di record di risorse per la query.
## <a name="syntax"></a>Sintassi
```
set querytype=<ResourceRecordtype>
```
### <a name="parameters"></a>Parametri
<ResourceRecordtype> specifica un tipo di record di risorse DNS. Il tipo di record di risorse predefinito è. Nella tabella seguente sono elencati i valori validi per questo comando.

| Valore |                                                   Descrizione                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Specifica un indirizzo&#39;IP del computer                                      |
|  QUALSIASI  |                                     Specifica un indirizzo&#39;IP del computer.                                      |
| CNAME |                                    Specifica un nome canonico per un alias.                                     |
|  GID  |                                  Specifica un identificatore di gruppo di un nome di gruppo.                                  |
| HINFO |                          Specifica la CPU&#39;e il tipo di sistema operativo di un computer.                           |
|  MB   |                                        Specifica un nome di dominio della cassetta postale.                                         |
|  MG   |                                         Specifica un membro del gruppo di posta elettronica.                                          |
| MINFO |                                   Specifica le informazioni sull'elenco di cassette postali o di posta                                   |
|  Lettura memoria   |                                     Specifica il nome di dominio per la ridenominazione della posta.                                      |
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
## <a name="remarks"></a>Note
- Il comando <strong>set Type</strong> esegue la stessa funzione del comando <strong>set querytype</strong> .
- Per ulteriori informazioni sui tipi di record di risorse, vedere Request for Comment (RFC) 1035.
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  < a href = Command-line-syntax-key.md data-RAW-source =-sintassi della [riga di comando chiave](command-line-syntax-key.md)> sintassi della riga di comando</a> < a href = nslookup-set-Type.MD data-RAW-Source =[nslookup set digitare](nslookup-set-type.md)> nslookup set Type</a>
