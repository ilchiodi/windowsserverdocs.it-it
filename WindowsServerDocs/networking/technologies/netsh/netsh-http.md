---
title: Comandi netsh per HTTP (Hypertext Transfer Protocol)
description: Usa netsh http per eseguire query e configurare parametri e impostazioni di HTTP.sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
manager: dougkim
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 81053e71040d2a0cd125af9fb7f3802dfd535781
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80853754"
---
# <a name="netsh-http-commands"></a>Comandi netsh per HTTP


Usa **netsh http** per eseguire query e configurare parametri e impostazioni di HTTP.sys.  

>[!TIP]
>Se usi Windows PowerShell in un computer che esegue Windows Server 2016 o Windows 10, digita **netsh** e premi INVIO. Al prompt netsh digita **http** e premi INVIO per ottenere il prompt netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

I comandi netsh http disponibili sono:

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [add timeout](#add-timeout)
- [add urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [delete timeout](#delete-timeout)
- [delete urlacl](#delete-urlacl)
- [flush logbuffer](#flush-logbuffer)
- [show cachestate](#show-cachestate)
- [show iplisten](#show-iplisten)
- [show servicestate](#show-servicestate)
- [show sslcert](#show-sslcert)
- [show timeout](#show-timeout)
- [show urlacl](#show-urlacl)

## <a name="add-iplisten"></a>add iplisten

Aggiunge un nuovo indirizzo IP all'elenco di ascolto IP, escluso il numero di porta.

**Sintassi**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parametri**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Indirizzo IPv4 o IPv6 da aggiungere all'elenco di ascolto IP. Questo elenco viene usato per definire l'ambito dell'elenco di indirizzi a cui viene eseguito il binding del servizio HTTP. "0.0.0.0" indica qualsiasi indirizzo IPv4 e "::" indica qualsiasi indirizzo IPv6. | Obbligatorio |

---

**Esempi**

Di seguito sono riportati quattro esempi del comando **add iplisten**.

-   add iplisten ipaddress=fe80::1
-   add iplisten ipaddress=1.1.1.1
-   add iplisten ipaddress=0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>add sslcert

Aggiunge un nuovo binding tra il certificato del server SSL e i criteri dei certificati client corrispondenti per un indirizzo IP e una porta.

**Sintassi**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parametri**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Specifica l'indirizzo IP e la porta per il binding. Vengono usati i due punti (:) come carattere delimitatore tra l'indirizzo IP e il numero di porta.                        | Obbligatorio |
|                 **certhash**                 |                                     Specifica l'hash SHA del certificato. Questo hash ha una lunghezza di 20 byte e viene specificato come stringa esadecimale.                                      | Obbligatorio |
|                  **appid**                   |                                                                  Specifica il GUID per identificare l'applicazione proprietaria.                                                                  | Obbligatorio |
|              **certstorename**               |                                  Specifica il nome dell'archivio per il certificato. L'impostazione predefinita è MY. Il certificato deve essere archiviato nel contesto del computer locale.                                  | Facoltativo |
|        **verifyclientcertrevocation**        |                                                      Specifica l'attivazione/disattivazione della verifica della revoca dei certificati client.                                                       | Facoltativo |
| **verifyrevocationwithcachedclientcertonly** |                                      Specifica se l'uso del solo certificato client memorizzato nella cache per la verifica della revoca è abilitato o disabilitato.                                       | Facoltativo |
|                **usagecheck**                |                                                      Specifica se la verifica dell'uso è abilitata o disabilitata. L'impostazione predefinita corrisponde all'abilitazione.                                                       | Facoltativo |
|         **revocationfreshnesstime**          | Specifica l'intervallo di tempo, in secondi, per la verifica della presenza di un elenco di revoche di certificati (CRL) aggiornato. Se questo valore è zero, il nuovo CRL viene aggiornato solo se quello precedente scade. | Facoltativo |
|           **urlretrievaltimeout**            |                            Specifica l'intervallo di timeout (in millisecondi) dopo il tentativo di recuperare l'elenco di revoche di certificati per l'URL remoto.                            | Facoltativo |
|             **sslctlidentifier**             |                Specifica l'elenco delle autorità di certificazione che possono essere considerate attendibili. Questo elenco può essere un sottoinsieme delle autorità di certificazione considerate attendibili dal computer.                 | Facoltativo |
|             **sslctlstorename**              |                                                Specifica il nome dell'archivio certificati in LOCAL_MACHINE dove è archiviato SslCtlIdentifier.                                                | Facoltativo |
|              **dsmapperusage**               |                                                        Specifica se le utilità di mapping servizio directory sono abilitate o disabilitate. L'impostazione predefinita corrisponde alla disabilitazione.                                                         | Facoltativo |
|          **clientcertnegotiation**           |                                              Specifica se la negoziazione del certificato è abilitata o disabilitata. L'impostazione predefinita corrisponde alla disabilitazione.                                               | Facoltativo |

---

**Esempi**

Di seguito è riportato un esempio del comando **add sslcert**.

add sslcert ipport=1.1.1.1:443 certhash=0102030405060708090A0B0C0D0E0F1011121314 appid={00112233-4455-6677-8899- AABBCCDDEEFF}

---

## <a name="add-timeout"></a>add timeout

Aggiunge un timeout globale al servizio.

**Sintassi** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parametri**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo di timeout per l'impostazione.                                     |
|    **value**    | Valore del timeout in secondi. Se il valore è in notazione esadecimale, aggiungi il prefisso 0x. |

---

**Esempi**

Di seguito sono riportati due esempi del comando **add timeout**.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>add urlacl

Aggiunge una voce di prenotazione URL (Uniform Resource Locator). Questo comando prenota l'URL per gli utenti e gli account non amministratori. È possibile specificare il DACL usando un nome di account NT con i parametri listen e delegate oppure usando una stringa SDDL.

**Sintassi**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parametri**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Specifica l'URL (Uniform Resource Locator) completo.                                           | Obbligatorio |
|   **user**   |                                                      Specifica il nome dell'utente o del gruppo di utenti.                                                       | Obbligatorio |
|  **listen**  | Specifica uno dei valori seguenti. yes: consente all'utente di registrare gli URL. Questo è il valore predefinito. no: impedisce all'utente di registrare gli URL. | Facoltativo |
| **delegate** |  Specifica uno dei valori seguenti. yes: consente all'utente di delegare gli URL. no: impedisce all'utente di delegare gli URL. Questo è il valore predefinito.  | Facoltativo |
|   **sddl**   |                                                Specifica una stringa SDDL che descrive il DACL.                                                 | Facoltativo |

---

**Esempi**

Di seguito sono riportati quattro esempi del comando **add urlacl**.

- add urlacl url=https://+:80/MyUri user=DOMAIN\\ user
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user listen=yes
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user delegate=no
- add urlacl url=https://+:80/MyUri sddl=...

---

## <a name="delete-cache"></a>delete cache

Elimina tutte le voci, o una voce specificata, dalla cache URI del kernel del servizio HTTP.

**Sintassi**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parametri**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Specifica l'URL (Uniform Resource Locator) completo da eliminare.                     | Facoltativo |
| **recursive** | Specifica se vengono rimosse tutte le voci nella cache URL. **yes**: vengono rimosse tutte le voci. **no**: non vengono rimosse tutte le voci. | Facoltativo |

---

**Esempi**

Di seguito sono riportati due esempi del comando **delete cache**.

- delete cache url=<https://www.contoso.com:80/myresource/> recursive=yes
- delete cache

---

## <a name="delete-iplisten"></a>delete iplisten

Elimina un indirizzo IP dall'elenco di ascolto IP. Questo elenco viene usato per definire l'ambito dell'elenco di indirizzi a cui viene eseguito il binding del servizio HTTP.

**Sintassi**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parametri**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Indirizzo IPv4 o IPv6 da eliminare dall'elenco di ascolto IP. Questo elenco viene usato per definire l'ambito dell'elenco di indirizzi a cui viene eseguito il binding del servizio HTTP. "0.0.0.0" indica qualsiasi indirizzo IPv4 e "::" indica qualsiasi indirizzo IPv6. Il numero di porta non è incluso. | Obbligatorio |

---


**Esempi**

Di seguito sono riportati quattro esempi del comando **delete iplisten**.

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>delete sslcert


Elimina i binding tra il certificato del server SSL e i criteri dei certificati client corrispondenti per un indirizzo IP e una porta.

**Sintassi**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parametri**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Specifica l'indirizzo IPv4 o IPv6 e la porta per cui vengono eliminati i binding dei certificati SSL. Vengono usati i due punti (:) come carattere delimitatore tra l'indirizzo IP e il numero di porta. | Obbligatorio |

---


**Esempi**

Di seguito sono riportati tre esempi del comando **delete sslcert**.

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>delete timeout

Elimina un timeout globale e fa in modo che il servizio ripristini i valori predefiniti.

**Sintassi**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parametri**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Specifica il tipo di impostazione di timeout. | Obbligatorio |

---


**Esempi**

Di seguito sono riportati due esempi del comando **delete timeout**.

-   delete timeout timeouttype=idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>delete urlacl

Elimina le prenotazioni URL.

**Sintassi**

```powershell
delete urlacl [ url= ] URL
```

**Parametri**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL (Uniform Resource Locator) completo da eliminare. | Obbligatorio |

---


**Esempi**

Di seguito sono riportati due esempi del comando **delete urlacl**.

- delete urlacl url=https://+:80/MyUri
- delete urlacl url=<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>flush logbuffer

Scarica i buffer interni per i file di log.

**Sintassi**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>show cachestate

Elenca le risorse URI memorizzate nella cache e le relative proprietà associate. Questo comando elenca tutte le risorse e le relative proprietà associate memorizzate nella cache risposte HTTP oppure visualizza una singola risorsa e le proprietà associate.

**Sintassi**

```powershell
show cachestate [ [url= ] URL]
```

**Parametri**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL completo da visualizzare. Se non specificato, vengono visualizzati tutti gli URL. L'URL può anche essere un prefisso degli URL registrati. | Facoltativo |

---


**Esempi**

Di seguito sono riportati due esempi del comando **show cachestate**:

- show cachestate url=<https://www.contoso.com:80/myresource>
- show cachestate

---

## <a name="show-iplisten"></a>show iplisten

Visualizza tutti gli indirizzi IP presenti nell'elenco di ascolto IP. Questo elenco viene usato per definire l'ambito dell'elenco di indirizzi a cui viene eseguito il binding del servizio HTTP. "0.0.0.0" indica qualsiasi indirizzo IPv4 e "::" indica qualsiasi indirizzo IPv6.

**Sintassi**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>show servicestate

Visualizza uno snapshot del servizio HTTP.

**Sintassi**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parametri**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Visualizza**   | Specifica se visualizzare uno snapshot dello stato del servizio HTTP in base alla sessione server o alle code delle richieste. | Facoltativo |
| **Verbose** |                Specifica se visualizzare informazioni dettagliate che mostrano anche le informazioni relative alle proprietà.                | Facoltativo |

---

**Esempi**

Di seguito sono riportati due esempi del comando **show servicestate**.

-   show servicestate view="session"
-   show servicestate view="requestq"

---

## <a name="show-sslcert"></a>show sslcert

Visualizza i binding tra il certificato del server SSL (Secure Sockets Layer) e i criteri dei certificati client corrispondenti per un indirizzo IP e una porta.

**Sintassi**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parametri**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Specifica l'indirizzo IPv4 o IPv6 e la porta per cui vengono visualizzati i binding dei certificati SSL. Vengono usati i due punti (:) come carattere delimitatore tra l'indirizzo IP e il numero di porta. Se non specifichi ipport, vengono visualizzati tutti i binding. | Obbligatorio |

---


**Esempi**

Di seguito sono riportati cinque esempi del comando **show sslcert**.

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   show sslcert ipport=0.0.0.0:443
-   show sslcert ipport=[::]:443
-   show sslcert

---

## <a name="show-timeout"></a>show timeout

Visualizza, in secondi, i valori di timeout del servizio HTTP.

**Sintassi**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>show urlacl

Visualizza gli elenchi di controllo di accesso discrezionale (DACL) per l'URL prenotato specificato o per tutti gli URL prenotati.

**Sintassi**

```powershell
show urlacl [ [url= ] URL]
```

**Parametri**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL completo da visualizzare. Se non specificato, vengono visualizzati tutti gli URL. | Facoltativo |

---


**Esempi**

Di seguito sono riportati tre esempi del comando **show urlacl**.

- show urlacl url=https://+:80/MyUri
- show urlacl url=<https://www.contoso.com:80/MyUri>
- show urlacl

---