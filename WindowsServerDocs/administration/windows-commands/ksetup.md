---
title: ksetup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0194cf81d069d7a5c1223f0a514d593e4870d397
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868842"
---
# <a name="ksetup"></a>ksetup



Esegue attività correlate alla configurazione e gestione protocollo Kerberos e il centro distribuzione chiavi (KDC) per il supporto di aree di autenticazione Kerberos, che non sono presenti anche i domini di Windows. Per esempi di come è possibile utilizzare questo comando, vedere la sezione esempi in ognuno dei relativi sottoargomenti.

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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Il computer diventa un membro di un'area di autenticazione Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Un'entità Kerberos viene eseguito il mapping a un account.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Definisce una voce KDC per l'area di autenticazione specificato.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Elimina una voce KDC per l'area di autenticazione.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Aggiunge un indirizzo del server Kpasswd per un'area di autenticazione.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Elimina un indirizzo del server Kpasswd per un'area di autenticazione.|
|[Ksetup:server](ksetup-server.md)|Consente di specificare il nome di un computer Windows a cui applicare le modifiche.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Imposta la password per del computer dominio (entità o un account host).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Elimina tutte le informazioni per l'area di autenticazione specificato dal Registro di sistema.|
|[Ksetup:domain](ksetup-domain.md)|Consente di specificare un dominio (se \<NomeDominio > non è stato impostato tramite **/domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Consente di usare il Kpasswd per modificare la password dell'utente attualmente connesso al computer.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Elenca le disponibile che i flag dell'area di autenticazione **che ksetup** in grado di rilevare.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Imposta i flag dell'area di autenticazione per un'area di autenticazione specifico.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Aggiunge i flag dell'area di autenticazione aggiuntive a un'area di autenticazione.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Elimina i flag dell'area di autenticazione da un'area di autenticazione.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analizza la configurazione di Kerberos in un dato computer. Aggiunge un host per il mapping dell'area di autenticazione al Registro di sistema.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Aggiunge un valore del Registro di sistema per eseguire il mapping di host per l'area di autenticazione Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Elimina il valore del Registro di sistema che è mappata al computer host per l'area di autenticazione Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Imposta i tipi di crittografia di una o più attributi di relazione di trust per il dominio.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Ottiene l'attributo di relazione di trust i tipi di crittografia per il dominio.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Aggiunge i tipi di crittografia per l'attributo di relazione di trust i tipi di crittografia per il dominio.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Elimina l'attributo di relazione di trust i tipi di crittografia per il dominio.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

**Che Ksetup** viene utilizzato per modificare le impostazioni del computer per l'individuazione delle aree di autenticazione Kerberos. Nelle implementazioni non basati su Microsoft Kerberos, tali informazioni vengono in genere mantenute nel file Krb5.conf. Nei sistemi operativi Windows Server, viene mantenuta nel Registro di sistema. È possibile utilizzare questo strumento per modificare queste impostazioni. Queste impostazioni vengono usate per le workstation per individuare le aree di autenticazione Kerberos e per i controller di dominio per individuare le aree di autenticazione Kerberos per le relazioni di trust tra aree.

**Che Ksetup** Inizializza le chiavi del Registro di sistema che i Security Support Provider (SSP) di Kerberos viene usato per individuare un KDC per l'area di autenticazione Kerberos se il computer è in esecuzione Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 e non è un membro di un Windows dominio. Dopo la configurazione, gli account utente di un computer client che esegue il sistema operativo può accedere a Windows nell'area di autenticazione Kerberos.

Il protocollo Kerberos versione 5 è il valore predefinito per l'autenticazione di rete nei computer che eseguono Windows XP Professional, Windows Vista e Windows 7. il SSP Kerberos ricerca Registro di sistema per il nome di dominio dell'area di autenticazione dell'utente e quindi il nome viene risolto in un indirizzo IP eseguendo una query di un server DNS. Il protocollo Kerberos è possibile usare DNS per individuare KDC utilizzando solo il nome dell'area di autenticazione, ma deve essere configurato in modo specifico a tale scopo.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)