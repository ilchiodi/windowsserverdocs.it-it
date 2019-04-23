---
title: tsprof
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e17d60126125fcd4b10373133dd61ca0db030290
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836072"
---
# <a name="tsprof"></a>tsprof

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia le informazioni di configurazione utente di Servizi Desktop remoto da un utente a un altro.
Le informazioni di configurazione utente di Servizi Desktop remoto viene visualizzate in estensioni di Servizi Desktop remoto per gli utenti locali e i gruppi e active directory gli utenti e computer.

**tsprof** possono anche impostare il percorso del profilo per un utente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/Update|Informazioni sul percorso per il profilo aggiornamenti <*nomeutente*> dominio <*DomainName*> per <*PercorsoProfilo*>.|
|/domain:\<DomainName>|Specifica il nome del dominio in cui viene applicata l'operazione.|
|/ locale|Applica l'operazione solo agli account utente locali.|
|/profile:\<path>|Specifica il percorso del profilo così come visualizzati nelle estensioni di Servizi Desktop remoto in locale di utenti e gruppi e active directory gli utenti e computer.|
|\<Nome utente >|Specifica il nome dell'utente per il quale si desidera aggiornare o il percorso del profilo del server di query.|
|/Copy|Copia le informazioni di configurazione utente da \< *SourceUser*> per \< *DestinationUser*> e aggiorna le informazioni di percorso profilo per \<  *DestinationUser*> a \< *PercorsoProfilo*>. Entrambe \< *SourceUser*> e \< *DestinationUser*> deve essere locale o deve essere nel dominio \< *DomainName*> .|
|\<Src_usr>|Specifica il nome dell'utente da cui si desidera copiare le informazioni di configurazione utente.|
|\<Dest_usr>|Specifica il nome dell'utente a cui si desidera copiare le informazioni di configurazione utente.|
|/q|Visualizza il percorso del profilo corrente dell'utente per il quale si desidera eseguire una query il percorso del profilo del server.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il **tsprof** comando è disponibile solo dopo aver installato il servizio ruolo Terminal Server in un computer che esegue servizio ruolo Host sessione Desktop remoto o Windows Server 2008 in un computer che esegue Windows Server 2008 R2.

## <a name="BKMK_examples"></a>Esempi
-   Per copiare le informazioni di configurazione utente da Utentelocale1 Utentelocale2, digitare:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Per impostare il percorso profilo Servizi Desktop remoto per Utentelocale1 su una directory denominata "c:\profiles", digitare:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
