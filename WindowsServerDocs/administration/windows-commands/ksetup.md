---
title: ksetup
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f3fde0ada4ab8bcbe52eccf22b959f99f91319f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724543"
---
# <a name="ksetup"></a>ksetup



Esegue attività correlate alla configurazione e alla gestione del protocollo Kerberos e del Centro distribuzione chiavi (KDC) per supportare le aree di autenticazione Kerberos, che non sono anche domini di Windows. Per esempi di come è possibile usare questo comando, vedere la sezione Esempi in ognuno degli argomenti correlati.

## <a name="syntax"></a>Sintassi

```
ksetup 
[/setrealm <DNSDomainName>] 
[/mapuser <Principal> <Account>] 
[/addkdc <RealmName> <KDCName>] 
[/delkdc <RealmName> <KDCName>]
[/addkpasswd <RealmName> <KDCPasswordName>] 
[/delkpasswd <RealmName> <KDCPasswordName>]
[/server <ServerName>] 
[/setcomputerpassword <Password>]
[/removerealm <RealmName>]  
[/domain <DomainName>] 
[/changepassword <OldPassword> <NewPassword>] 
[/listrealmflags] 
[/setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/dumpstate]
[/addhosttorealmmap] <HostName> <RealmName>]  
[/delhosttorealmmap] <HostName> <RealmName>]  
[/setenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <DomainName>
[/addenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <DomainName>

```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Rende questo computer membro di un'area di autenticazione Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Esegue il mapping di un'entità Kerberos a un account.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Definisce una voce KDC per l'area di autenticazione specificata.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Elimina una voce KDC per l'area di autenticazione.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Aggiunge un indirizzo del server kpasswd per un'area di autenticazione.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Elimina un indirizzo del server kpasswd per un'area di autenticazione.|
|[Ksetup:server](ksetup-server.md)|Consente di specificare il nome di un computer Windows in cui applicare le modifiche.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Imposta la password per l'account di dominio del computer (o l'entità host).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Elimina tutte le informazioni per l'area di autenticazione specificato dal Registro di sistema.|
|[Ksetup:domain](ksetup-domain.md)|Consente di specificare un dominio (se \<domainname> non è stato impostato tramite **/Domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Consente di usare kpasswd per modificare la password dell'utente che ha eseguito l'accesso.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Elenca i flag dell'area di autenticazione disponibili che **che Ksetup** è in grado di rilevare.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Imposta i flag dell'area di autenticazione per un'area di autenticazione specifica.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Aggiunge ulteriori flag di area di autenticazione a un'area di autenticazione.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Elimina i flag area di autenticazione da un'area di autenticazione.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analizza la configurazione Kerberos nel computer specificato. Aggiunge un host al mapping dell'area di autenticazione al registro di sistema.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Aggiunge un valore del registro di sistema per eseguire il mapping dell'host all'area di autenticazione Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Elimina il valore del registro di sistema che ha eseguito il mapping del computer host all'area di autenticazione Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Imposta uno o più tipi di crittografia attributi di trust per il dominio.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Ottiene l'attributo trust dei tipi di crittografia per il dominio.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Aggiunge tipi di crittografia all'attributo trust dei tipi di crittografia per il dominio.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Elimina l'attributo trust dei tipi di crittografia per il dominio.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

**Che Ksetup** viene utilizzato per modificare le impostazioni del computer per l'individuazione delle aree di autenticazione Kerberos. Nelle implementazioni basate su Kerberos non Microsoft queste informazioni vengono in genere mantenute nel file krb5. conf. Nei sistemi operativi Windows Server, viene mantenuta nel registro di sistema. È possibile utilizzare questo strumento per modificare queste impostazioni. Queste impostazioni vengono usate dalle workstation per individuare le aree di autenticazione Kerberos e i controller di dominio per individuare le aree di autenticazione Kerberos per le relazioni di trust tra aree di autenticazione.

**Che Ksetup** Inizializza le chiavi del registro di sistema utilizzate da Kerberos Security Support Provider (SSP) per individuare un KDC per l'area di autenticazione Kerberos se il computer esegue windows Server 2003, windows Server 2008 o windows Server 2008 R2 e non è un membro di un dominio Windows. Dopo la configurazione, l'utente di un computer client che esegue il sistema operativo Windows può accedere agli account nell'area di autenticazione Kerberos.

Il protocollo Kerberos versione 5 è il valore predefinito per l'autenticazione di rete nei computer che eseguono Windows XP Professional, Windows Vista e Windows 7. SSP Kerberos Cerca nel registro di sistema il nome di dominio dell'area di autenticazione dell'utente e quindi risolve il nome in un indirizzo IP eseguendo una query su un server DNS. Il protocollo Kerberos può usare DNS per individuare KDC usando solo il nome dell'area di autenticazione, ma deve essere appositamente configurato per eseguire questa operazione.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)