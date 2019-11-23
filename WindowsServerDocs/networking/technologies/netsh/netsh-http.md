---
title: Comandi Netsh per Hypertext Transfer Protocol (HTTP)
description: Utilizzare netsh http per eseguire query e configurare parametri e impostazioni HTTP. sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401881"
---
# <a name="netsh-http-commands"></a>Comandi netsh per HTTP


Utilizzare **netsh http** per eseguire query e configurare parametri e impostazioni http. sys.  

>[!TIP]
>Se si usa Windows PowerShell in un computer che esegue Windows Server 2016 o Windows 10, digitare **netsh** e premere INVIO. Al prompt dei comandi netsh digitare **http** e premere INVIO per ottenere il prompt http netsh.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

I comandi netsh http disponibili sono:

- [Aggiungi iplisten](#add-iplisten)
- [Aggiungi sslcert](#add-sslcert)
- [Aggiungi timeout](#add-timeout)
- [Aggiungi urlacl](#add-urlacl)
- [Elimina cache](#delete-cache)
- [Elimina iplisten](#delete-iplisten)
- [Elimina sslcert](#delete-sslcert)
- [timeout eliminazione](#delete-timeout)
- [Elimina urlacl](#delete-urlacl)
- [Scarica logbuffer](#flush-logbuffer)
- [Mostra capetto](#show-cachestate)
- [Mostra iplisten](#show-iplisten)
- [Mostra ServiceState](#show-servicestate)
- [Mostra sslcert](#show-sslcert)
- [Mostra timeout](#show-timeout)
- [Mostra urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Aggiungi iplisten

Aggiunge un nuovo indirizzo IP all'elenco di ascolto IP, escluso il numero di porta.

**Sintassi**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parametri**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **IPAddress** | Indirizzo IPv4 o IPv6 da aggiungere all'elenco di ascolto IP. L'elenco di ascolto IP viene usato per definire l'ambito dell'elenco di indirizzi a cui viene associato il servizio HTTP. "0.0.0.0" indica qualsiasi indirizzo IPv4 e "::" indica qualsiasi indirizzo IPv6. | Obbligatorio |

---

**esempi**

Di seguito sono riportati quattro esempi del comando **Add iplisten** .

-   aggiungere iplisten IPAddress = FE80:: 1
-   aggiungere iplisten IPAddress = 1.1.1.1
-   aggiungere iplisten IPAddress = 0.0.0.0
-   aggiungere iplisten IPAddress =::

---

## <a name="add-sslcert"></a>Aggiungi sslcert

Aggiunge una nuova associazione di certificati del server SSL e i criteri dei certificati client corrispondenti per un indirizzo IP e una porta.

**Sintassi**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parametri**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Specifica l'indirizzo IP e la porta per l'associazione. Un carattere due punti (:) viene utilizzato come delimitatore tra l'indirizzo IP e il numero di porta.                        | Obbligatorio |
|                 **certhash**                 |                                     Specifica l'hash SHA del certificato. Questo hash ha una lunghezza di 20 byte e viene specificato come stringa esadecimale.                                      | Obbligatorio |
|                  **appid**                   |                                                                  Specifica il GUID per identificare l'applicazione proprietaria.                                                                  | Obbligatorio |
|              **certstorename**               |                                  Specifica il nome dell'archivio per il certificato. IL valore predefinito è MY. Il certificato deve essere archiviato nel contesto del computer locale.                                  | Facoltativo |
|        **verifyclientcertrevocation**        |                                                      Specifica la verifica della revoca dei certificati client attiva/disattiva.                                                       | Facoltativo |
| **verifyrevocationwithcachedclientcertonly** |                                      Specifica se l'utilizzo del solo certificato client memorizzato nella cache per il controllo delle revoche è abilitato o disabilitato.                                       | Facoltativo |
|                **usagecheck**                |                                                      Specifica se il controllo di utilizzo è abilitato o disabilitato. Il valore predefinito è Enabled.                                                       | Facoltativo |
|         **RevocationFreshnessTime**          | Specifica l'intervallo di tempo, in secondi, per verificare la presenza di un elenco di revoche di certificati (CRL) aggiornato. Se questo valore è zero, il nuovo CRL viene aggiornato solo se quello precedente scade. | Facoltativo |
|           **urlretrievaltimeout**            |                            Specifica l'intervallo di timeout (in millisecondi) dopo il tentativo di recuperare l'elenco di revoche di certificati per l'URL remoto.                            | Facoltativo |
|             **sslctlidentifier**             |                Specifica l'elenco delle autorità emittenti di certificati che possono essere attendibili. Questo elenco può essere un subset delle autorità emittenti di certificati considerate attendibili dal computer.                 | Facoltativo |
|             **sslctlstorename**              |                                                Specifica il nome dell'archivio certificati in LOCAL_MACHINE dove è archiviato SslCtlIdentifier.                                                | Facoltativo |
|              **dsmapperusage**               |                                                        Specifica se i Mapper DS sono abilitati o disabilitati. Il valore predefinito è Disabled.                                                         | Facoltativo |
|          **clientcertnegotiation**           |                                              Specifica se la negoziazione del certificato è abilitata o disabilitata. Il valore predefinito è Disabled.                                               | Facoltativo |

---

**esempi**

Di seguito è riportato un esempio del comando **add sslcert** .

add sslcert ipport = 1.1.1.1:443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>Aggiungi timeout

Aggiunge un timeout globale al servizio.

**Sintassi** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parametri**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo di timeout per l'impostazione.                                     |
|    **value**    | Valore del timeout (in secondi). Se il valore è in notazione esadecimale, aggiungere il prefisso 0x. |

---

**esempi**

Di seguito sono riportati due esempi del comando **Add timeout** .

-   Aggiungi timeout timeouttype = idleconnectiontimeout valore = 120
-   Aggiungi timeout timeouttype = HeaderWaitTimeout valore = 0x40

---

## <a name="add-urlacl"></a>Aggiungi urlacl

Aggiunge una voce di prenotazione di Uniform Resource Locator (URL). Questo comando riserva l'URL per gli utenti e gli account non amministratori. È possibile specificare l'elenco DACL usando un nome di account NT con i parametri Listen e Delegate oppure usando una stringa SDDL.

**Sintassi**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parametri**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Specifica il Uniform Resource Locator completo (URL).                                           | Obbligatorio |
|   **user**   |                                                      Specifica il nome dell'utente o del gruppo di utenti                                                       | Obbligatorio |
|  **ascoltare**  | Specifica uno dei valori seguenti: Sì: consente all'utente di registrare gli URL. Rappresenta il valore predefinito. No: nega all'utente la registrazione degli URL. | Facoltativo |
| **Delegato** |  Specifica uno dei valori seguenti: Sì: consente all'utente di delegare gli URL No: nega all'utente la delega degli URL. Rappresenta il valore predefinito.  | Facoltativo |
|   **SDDL**   |                                                Specifica una stringa SDDL che descrive l'elenco DACL.                                                 | Facoltativo |

---

**esempi**

Di seguito sono riportati quattro esempi del comando **add urlacl** .

- Aggiungi URL urlacl =https://+:80/MyUri utente = dominio\\utente
- Aggiungi URL urlacl =<https://www.contoso.com:80/MyUri> utente = dominio\\utente ascolto = Sì
- Aggiungi URL urlacl =<https://www.contoso.com:80/MyUri> utente = dominio\\utente delegato = No
- Aggiungi URL urlacl =https://+:80/MyUri SDDL =...

---

## <a name="delete-cache"></a>Elimina cache

Elimina tutte le voci o una voce specificata dalla cache URI del kernel del servizio HTTP.

**Sintassi**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parametri**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Specifica il Uniform Resource Locator completo (URL) che si desidera eliminare.                     | Facoltativo |
| **ricorsiva** | Specifica se vengono rimosse tutte le voci della cache URL. **Sì**: Rimuovi tutte le voci **No**: non rimuovere tutte le voci | Facoltativo |

---

**esempi**

Di seguito sono riportati due esempi del comando **delete cache** .

- Elimina URL cache =<https://www.contoso.com:80/myresource/> ricorsivo = Sì
- Elimina cache

---

## <a name="delete-iplisten"></a>Elimina iplisten

Elimina un indirizzo IP dall'elenco di ascolto IP. L'elenco di ascolto IP viene usato per definire l'ambito dell'elenco di indirizzi a cui viene associato il servizio HTTP.

**Sintassi**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parametri**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **IPAddress** | Indirizzo IPv4 o IPv6 da eliminare dall'elenco di ascolto IP. L'elenco di ascolto IP viene usato per definire l'ambito dell'elenco di indirizzi a cui viene associato il servizio HTTP. "0.0.0.0" indica qualsiasi indirizzo IPv4 e "::" indica qualsiasi indirizzo IPv6. Il numero di porta non è incluso. | Obbligatorio |

---


**esempi**

Di seguito sono riportati quattro esempi del comando **delete iplisten** .

-   Elimina iplisten IPAddress = FE80:: 1
-   Elimina iplisten IPAddress = 1.1.1.1
-   Elimina iplisten IPAddress = 0.0.0.0
-   Elimina iplisten IPAddress =::

---

## <a name="delete-sslcert"></a>Elimina sslcert


Elimina i binding del certificato server SSL e i criteri dei certificati client corrispondenti per un indirizzo IP e una porta.

**Sintassi**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parametri**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Specifica l'indirizzo IPv4 o IPv6 e la porta per cui vengono eliminate le associazioni di certificati SSL. Un carattere due punti (:) viene utilizzato come delimitatore tra l'indirizzo IP e il numero di porta. | Obbligatorio |

---


**esempi**

Di seguito sono riportati tre esempi del comando **Delete sslcert** .

- Delete sslcert ipport = 1.1.1.1:443
- Delete sslcert ipport = 0.0.0.0:443
- Delete sslcert ipport = [::]: 443

---

## <a name="delete-timeout"></a>timeout eliminazione

Elimina un timeout globale e fa in modo che il servizio ripristini i valori predefiniti.

**Sintassi**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parametri**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Specifica il tipo di impostazione del timeout. | Obbligatorio |

---


**esempi**

Di seguito sono riportati due esempi del comando **Delete timeout** .

-   Delete timeout timeouttype = idleconnectiontimeout
-   Delete timeout timeouttype = HeaderWaitTimeout

---

## <a name="delete-urlacl"></a>Elimina urlacl

Elimina le prenotazioni URL.

**Sintassi**

```powershell
delete urlacl [ url= ] URL
```

**Parametri**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Specifica il Uniform Resource Locator completo (URL) che si desidera eliminare. | Obbligatorio |

---


**esempi**

Di seguito sono riportati due esempi del comando **delete urlacl** .

- Elimina URL urlacl =https://+:80/MyUri
- Elimina URL urlacl =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>Scarica logbuffer

Scarica i buffer interni per i file di log.

**Sintassi**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Mostra capetto

Elenca le risorse URI memorizzate nella cache e le relative proprietà associate. Questo comando elenca tutte le risorse e le relative proprietà associate memorizzate nella cache di risposta HTTP o Visualizza una singola risorsa e le proprietà associate.

**Sintassi**

```powershell
show cachestate [ [url= ] URL]
```

**Parametri**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL completo che si desidera visualizzare. Se non è specificato, visualizzare tutti gli URL. L'URL può anche essere un prefisso per gli URL registrati. | Facoltativo |

---


**esempi**

Di seguito sono riportati due esempi del comando **Mostra capetto** :

- Mostra URL di capetto =<https://www.contoso.com:80/myresource>
- Mostra capetto

---

## <a name="show-iplisten"></a>Mostra iplisten

Visualizza tutti gli indirizzi IP nell'elenco di ascolto IP. L'elenco di ascolto IP viene usato per definire l'ambito dell'elenco di indirizzi a cui viene associato il servizio HTTP. "0.0.0.0" indica qualsiasi indirizzo IPv4 e "::" indica qualsiasi indirizzo IPv6.

**Sintassi**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Mostra ServiceState

Consente di visualizzare uno snapshot del servizio HTTP.

**Sintassi**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parametri**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Visualizza**   | Specifica se visualizzare uno snapshot dello stato del servizio HTTP in base alla sessione del server o alle code di richiesta. | Facoltativo |
| **Dettagliato** |                Specifica se visualizzare informazioni dettagliate che mostrano anche le informazioni sulle proprietà.                | Facoltativo |

---

**esempi**

Di seguito sono riportati due esempi del comando **show servicestate** .

-   Mostra ServiceState View = "Session"
-   Mostra ServiceState View = "requestq"

---

## <a name="show-sslcert"></a>Mostra sslcert

Visualizza le associazioni di certificati del server Secure Sockets Layer (SSL) e i criteri dei certificati client corrispondenti per un indirizzo IP e una porta.

**Sintassi**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parametri**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Specifica l'indirizzo IPv4 o IPv6 e la porta per cui vengono visualizzati i binding del certificato SSL. Un carattere due punti (:) viene utilizzato come delimitatore tra l'indirizzo IP e il numero di porta. Se non si specifica ipport, verranno visualizzate tutte le associazioni. | Obbligatorio |

---


**esempi**

Di seguito sono riportati cinque esempi del comando **show sslcert** .

-   Mostra sslcert ipport = [fe80:: 1]: 443
-   Mostra sslcert ipport = 1.1.1.1:443
-   Mostra sslcert ipport = 0.0.0.0:443
-   Mostra sslcert ipport = [::]: 443
-   Mostra sslcert

---

## <a name="show-timeout"></a>Mostra timeout

Visualizza, in secondi, i valori di timeout del servizio HTTP.

**Sintassi**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>Mostra urlacl

Visualizza gli elenchi di controllo di accesso discrezionale (DACL) per l'URL riservato specificato o per tutti gli URL riservati.

**Sintassi**

```powershell
show urlacl [ [url= ] URL]
```

**Parametri**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL completo che si desidera visualizzare. Se non specificato, visualizzare tutti gli URL. | Facoltativo |

---


**esempi**

Di seguito sono riportati tre esempi del comando **show urlacl** .

- Mostra URL urlacl =https://+:80/MyUri
- Mostra URL urlacl =<https://www.contoso.com:80/MyUri>
- Mostra urlacl

---