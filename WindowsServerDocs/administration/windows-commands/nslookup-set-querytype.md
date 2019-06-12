---
title: nslookup set querytype
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0015db716bd8c74bc4366063009bda41d338d19
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436736"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il tipo di record di risorse per la query.
## <a name="syntax"></a>Sintassi
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Parametri
<ResourceRecordtype> Specifica un tipo di record risorsa DNS. Il tipo di record di risorse predefinito è r. La tabella seguente elenca i valori validi per questo comando.

| Value |                                                   Descrizione                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Specifica un computer con&#39;indirizzo IP                                      |
|  QUALSIASI  |                                     Specifica un computer con&#39;indirizzo IP.                                      |
| CNAME |                                    Specifica un nome canonico di un alias.                                     |
|  GID  |                                  Specifica un identificatore del gruppo di un nome di gruppo.                                  |
| HINFO |                          Specifica un computer con&#39;s della CPU e il tipo del sistema operativo.                           |
|  MB   |                                        Specifica un nome di dominio della cassetta postale.                                         |
|  MG   |                                         Specifica un membro del gruppo di posta elettronica.                                          |
| MINFO |                                   Specifica le informazioni di elenco della cassetta postale o posta elettronica.                                   |
|  Lettura memoria   |                                     Specifica il nome di dominio di posta elettronica rename.                                      |
|  MX   |                                          Specifica il sistema di posta.                                          |
|  NS   |                                 Specifica un server dei nomi DNS per la zona denominata.                                 |
|  PTR  | Specifica un computer con nome se la query è un indirizzo IP. in caso contrario, specifica il puntatore per altre informazioni. |
|  SOA  |                                Specifica l'inizio dell'autorità per una zona DNS.                                 |
|  TXT  |                                         Specifica le informazioni di testo.                                         |
|  UID  |                                         Specifica l'identificatore utente.                                          |
| UINFO |                                         Specifica le informazioni dell'utente.                                         |
|  WKS  |                                         Descrive un servizio noto.                                         |
| Guida { |                                                       ?}                                                        |

Visualizza una breve descrizione della <strong>nslookup</strong> sottocomandi
## <a name="remarks"></a>Note
- Il <strong>impostare il tipo</strong> comando esegue la stessa funzione di <strong>set querytype</strong> comando.
- per altre informazioni sui tipi di record di risorse, vedere richiesta di commento (Rfc) 1035.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">Chiave sintassi della riga di comando</a>
  <a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">tipo set nslookup</a>
