---
title: tsprof
description: Windows Commands argomento per tsprof, che copia le informazioni di configurazione dell'utente Servizi Desktop remoto da un utente a un altro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b1037b6daff467a71517917d423e4cbe87a97f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832474"
---
# <a name="tsprof"></a>tsprof

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia le informazioni di configurazione utente di Servizi Desktop remoto da un utente a un altro.
Le informazioni di configurazione dell'utente Servizi Desktop remoto vengono visualizzate nelle estensioni Servizi Desktop remoto a utenti e gruppi locali e utenti e computer di Active Directory.

**tsprof** può anche impostare il percorso del profilo per un utente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/update|Aggiorna le informazioni sul percorso del profilo per <*nome utente*> nel dominio <*DomainName*> per <*filePath*>.|
|/Domain:\<NomeDominio >|Specifica il nome del dominio in cui viene applicata l'operazione.|
|/ locale|Applica l'operazione solo agli account utente locali.|
|/profile: > percorso\<|Specifica il percorso del profilo come visualizzato nelle estensioni Servizi Desktop remoto in utenti e gruppi locali e in utenti e computer di Active Directory.|
|Nome utente \<>|Specifica il nome dell'utente per il quale si desidera aggiornare o il percorso del profilo del server di query.|
|/Copy|Copia le informazioni di configurazione utente da \<*SourceUser*> a \<*UtenteDestinazione*> e aggiorna le informazioni sul percorso del profilo per \<*UtenteDestinazione*> *per \<* propercorso >. Entrambe \<*SourceUser*> e \<*UtenteDestinazione*> devono essere locali o devono essere presenti nel dominio \<*DomainName*>.|
|\<Src_usr >|Specifica il nome dell'utente da cui si desidera copiare le informazioni di configurazione utente.|
|\<Dest_usr >|Specifica il nome dell'utente a cui si desidera copiare le informazioni di configurazione utente.|
|/q|Visualizza il percorso del profilo corrente dell'utente per il quale si desidera eseguire una query il percorso del profilo del server.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il comando **tsprof** è disponibile solo se è stato installato il servizio ruolo Terminal Server in un computer che esegue il servizio ruolo Host sessione Desktop remoto o windows Server 2008 in un computer che esegue windows Server 2008 R2.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
-   Per copiare le informazioni di configurazione utente da Utentelocale1 Utentelocale2, digitare:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Per impostare il percorso del profilo Servizi Desktop remoto per Utentelocale1 su una directory denominata c:\Profiles, digitare:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

## <a name="additional-references"></a>Altre informazioni di riferimento
- Guida di riferimento alla [chiave della sintassi della riga di comando](command-line-syntax-key.md)
[Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
