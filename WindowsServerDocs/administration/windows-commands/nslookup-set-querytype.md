---
title: nslookup set querytype
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bc0eb19fd66e738b4bfc110a2bbc172153a12d98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372896"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il tipo di record di risorse per la query.
## <a name="syntax"></a>Sintassi
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Parametri
<ResourceRecordtype>Specifica un tipo di record di risorse DNS. Il tipo di record di risorse predefinito è. Nella tabella seguente sono elencati i valori validi per questo comando.

| Value |                                                   Descrizione                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Specifica un indirizzo&#39;IP del computer                                      |
|  QUALSIASI  |                                     Specifica un indirizzo&#39;IP del computer.                                      |
| CNAME |                                    Specifica un nome canonico per un alias.                                     |
|  GID  |                                  Specifica un identificatore di gruppo di un nome di gruppo.                                  |
| HINFO |                          Specifica la CPU&#39;e il tipo di sistema operativo di un computer.                           |
|  MB   |                                        Specifica un nome di dominio della cassetta postale.                                         |
|  GRUPPO   |                                         Specifica un membro del gruppo di posta elettronica.                                          |
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
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">Chiave della sintassi della riga di comando</a>@no__t<a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">tipo di set nslookup</a>
