---
title: ksetup
description: Argomento di riferimento per il comando che Ksetup, che consente di eseguire attività correlate alla configurazione e alla gestione del protocollo Kerberos e del Centro distribuzione chiavi (KDC) per supportare le aree di autenticazione Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82b1627a8ddbc9e51ac32825c5a42c3df9effbf7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817351"
---
# <a name="ksetup"></a>ksetup

Esegue attività correlate alla configurazione e alla gestione del protocollo Kerberos e del Centro distribuzione chiavi (KDC) per supportare le aree di autenticazione Kerberos. In particolare, questo comando viene usato per:

- Modificare le impostazioni del computer per l'individuazione delle aree di autenticazione Kerberos. Nelle implementazioni basate su Kerberos non Microsoft queste informazioni vengono in genere mantenute nel file krb5. conf. Nei sistemi operativi Windows Server, viene mantenuto nel registro di sistema. È possibile utilizzare questo strumento per modificare queste impostazioni. Queste impostazioni vengono usate dalle workstation per individuare le aree di autenticazione Kerberos e i controller di dominio per individuare le aree di autenticazione Kerberos per le relazioni di trust tra aree di autenticazione.

- Inizializzare le chiavi del registro di sistema utilizzate da Kerberos Security Support Provider (SSP) per individuare un KDC per l'area di autenticazione Kerberos, se il computer non è membro di un dominio Windows. Dopo la configurazione, l'utente di un computer client che esegue il sistema operativo Windows può accedere agli account nell'area di autenticazione Kerberos.

- Cercare nel registro di sistema il nome di dominio dell'area di autenticazione dell'utente e quindi risolvere il nome in un indirizzo IP eseguendo una query su un server DNS. Il protocollo Kerberos può usare DNS per individuare KDC usando solo il nome dell'area di autenticazione, ma deve essere appositamente configurato per eseguire questa operazione.

## <a name="syntax"></a>Sintassi

```
ksetup
[/setrealm <DNSdomainname>]
[/mapuser <principal> <account>]
[/addkdc <realmname> <KDCname>]
[/delkdc <realmname> <KDCname>]
[/addkpasswd <realmname> <KDCPasswordName>]
[/delkpasswd <realmname> <KDCPasswordName>]
[/server <servername>]
[/setcomputerpassword <password>]
[/removerealm <realmname>]
[/domain <domainname>]
[/changepassword <oldpassword> <newpassword>]
[/listrealmflags]
[/setrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/addrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/dumpstate]
[/addhosttorealmmap] <hostname> <realmname>]
[/delhosttorealmmap] <hostname> <realmname>]
[/setenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <domainname>
[/addenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <domainname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [che Ksetup](ksetup-setrealm.md) | Rende questo computer membro di un'area di autenticazione Kerberos. |
| [addkdc che Ksetup](ksetup-addkdc.md) | Definisce una voce KDC per l'area di autenticazione specificata. |
| [delkdc che Ksetup](ksetup-delkdc.md) | Elimina una voce KDC per l'area di autenticazione. |
| [addkpasswd che Ksetup](ksetup-addkpasswd.md) | Aggiunge un indirizzo del server kpasswd per un'area di autenticazione. |
| [delkpasswd che Ksetup](ksetup-delkpasswd.md) | Elimina un indirizzo del server kpasswd per un'area di autenticazione. |
| [Server che Ksetup](ksetup-server.md) | Consente di specificare il nome di un computer Windows in cui applicare le modifiche. |
| [setcomputerpassword che Ksetup](ksetup-setcomputerpassword.md) | Imposta la password per l'account di dominio del computer (o l'entità host). |
| [removerealm che Ksetup](ksetup-removerealm.md) | Elimina tutte le informazioni per l'area di autenticazione specificato dal Registro di sistema. |
| [dominio che Ksetup](ksetup-domain.md) | Consente di specificare un dominio (se `<domainname>` non è già stato impostato dal parametro **/Domain** ). |
| [ChangePassword che Ksetup](ksetup-changepassword.md) | Consente di usare kpasswd per modificare la password dell'utente che ha eseguito l'accesso. |
| [listrealmflags che Ksetup](ksetup-listrealmflags.md) | Elenca i flag dell'area di autenticazione disponibili che **che Ksetup** è in grado di rilevare. |
| [setrealmflags che Ksetup](ksetup-setrealmflags.md) | Imposta i flag dell'area di autenticazione per un'area di autenticazione specifica. |
| [addrealmflags che Ksetup](ksetup-addrealmflags.md) | Aggiunge ulteriori flag di area di autenticazione a un'area di autenticazione. |
| [delrealmflags che Ksetup](ksetup-delrealmflags.md) | Elimina i flag area di autenticazione da un'area di autenticazione. |
| [dumpstate che Ksetup](ksetup-dumpstate.md) | Analizza la configurazione Kerberos nel computer specificato. Aggiunge un host al mapping dell'area di autenticazione al registro di sistema. |
| [addhosttorealmmap che Ksetup](ksetup-addhosttorealmmap.md) | Aggiunge un valore del registro di sistema per eseguire il mapping dell'host all'area di autenticazione Kerberos. |
| [delhosttorealmmap che Ksetup](ksetup-delhosttorealmmap.md) | Elimina il valore del registro di sistema che ha eseguito il mapping del computer host all'area di autenticazione Kerberos. |
| [setenctypeattr che Ksetup](ksetup-setenctypeattr.md) | Imposta uno o più tipi di crittografia attributi di trust per il dominio. |
| [getenctypeattr che Ksetup](ksetup-getenctypeattr.md) | Ottiene l'attributo trust dei tipi di crittografia per il dominio. |
| [addenctypeattr che Ksetup](ksetup-addenctypeattr.md) | Aggiunge tipi di crittografia all'attributo trust dei tipi di crittografia per il dominio. |
| [delenctypeattr che Ksetup](ksetup-delenctypeattr.md) | Elimina l'attributo trust dei tipi di crittografia per il dominio. |
| /? | Visualizza la Guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)