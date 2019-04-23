---
title: nslookup set type
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: accf8ccbe147a831ee306dd670384b90844ff4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835952"
---
# <a name="nslookup-set-type"></a>nslookup set type

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il tipo di record di risorse per la query.
## <a name="syntax"></a>Sintassi
```
set type=<ResourceRecordtype>
```
## <a name="parameters"></a>Parametri
<ResourceRecordtype> Specifica un tipo di record risorsa DNS. Il tipo di record di risorse predefinito è r. La tabella seguente elenca i valori validi per questo comando.
|Value|Descrizione|
|-----|--------|
|A|Specifica l'indirizzo IP del computer|
|QUALSIASI|Specifica l'indirizzo IP del computer.|
|CNAME|Specifica un nome canonico di un alias.|
|GID|Specifica un identificatore del gruppo di un nome di gruppo.|
|HINFO|Specifica della CPU e il tipo del sistema operativo del computer.|
|MB|Specifica un nome di dominio della cassetta postale.|
|MG|Specifica un membro del gruppo di posta elettronica.|
|MINFO|Specifica le informazioni di elenco della cassetta postale o posta elettronica.|
|Lettura memoria|Specifica il nome di dominio di posta elettronica rename.|
|MX|Specifica il sistema di posta.|
|NS|Specifica un server dei nomi DNS per la zona denominata.|
|PTR|Specifica un computer con nome se la query è un indirizzo IP. in caso contrario, specifica il puntatore per altre informazioni.|
|SOA|Specifica l'inizio dell'autorità per una zona DNS.|
|TXT|Specifica le informazioni di testo.|
|UID|Specifica l'identificatore utente.|
|UINFO|Specifica le informazioni dell'utente.|
|WKS|Descrive un servizio noto.|
Guida { | ?}
Visualizza una breve descrizione della **nslookup** sottocomandi
## <a name="remarks"></a>Note
-   Il **impostare il tipo** comando esegue la stessa funzione di **set querytype** comando.
-   per altre informazioni sui tipi di record di risorse, vedere richiesta di commento (Rfc) 1035.
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[querytype set nslookup](nslookup-set-querytype.md)
