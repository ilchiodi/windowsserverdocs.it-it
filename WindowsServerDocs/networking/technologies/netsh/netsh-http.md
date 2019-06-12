---
title: Netsh Commands for Hypertext Transfer Protocol (HTTP)
description: Usare netsh http per eseguire query e configurare le impostazioni HTTP. sys e parametri.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c5f3927abf1a2394c2dd5b8ea664c7de8d5f614
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446189"
---
# <a name="netsh-http-commands"></a>Comandi netsh per HTTP


Uso **netsh http** per eseguire query e configurare le impostazioni HTTP. sys e parametri.  

>[!TIP]
>Se si usa Windows PowerShell in un computer che esegue Windows Server 2016 o Windows 10, digitare **netsh** e premere INVIO. Al prompt netsh, digitare **http** e premere INVIO per ottenere il prompt dei comandi di netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

I comandi http netsh disponibili sono:

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [add timeout](#add-timeout)
- [aggiungere urlacl](#add-urlacl)
- [eliminare cache](#delete-cache)
- [Elimina iplisten](#delete-iplisten)
- [Elimina sslcert](#delete-sslcert)
- [eliminare i timeout](#delete-timeout)
- [Delete urlacl](#delete-urlacl)
- [scaricamento logbuffer](#flush-logbuffer)
- [Mostra cachestate](#show-cachestate)
- [show iplisten](#show-iplisten)
- [Mostra servicestate](#show-servicestate)
- [show sslcert](#show-sslcert)
- [Mostra timeout](#show-timeout)
- [Mostra urlacl](#show-urlacl)

## <a name="add-iplisten"></a>aggiungere iplisten

Aggiunge un nuovo indirizzo IP all'elenco di ascolto IP, escluso il numero di porta.

**Sintassi**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parametri**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Elenco di ascolto l'indirizzo IPv4 o IPv6 da aggiungere all'indirizzo IP. L'elenco di ascolto IP consente di definire l'ambito l'elenco di indirizzi a cui viene associato al servizio HTTP. "0.0.0.0" significa che tutti gli indirizzi IPv4 e "::" si intende qualsiasi indirizzo IPv6. | Obbligatorio |

---

**Esempi**

Quattro esempi seguenti vengono illustrate le **aggiungere iplisten** comando.

-   add iplisten ipaddress=fe80::1
-   add iplisten ipaddress=1.1.1.1
-   add iplisten ipaddress=0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>aggiungere sslcert

Aggiunge un nuovo certificato del server SSL binding e i corrispondenti criteri di certificato client per un indirizzo IP e porta.

**Sintassi**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parametri**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Specifica l'indirizzo IP e la porta per l'associazione. Un carattere due punti (:) viene usato come delimitatore tra l'indirizzo IP e il numero di porta.                        | Obbligatorio |
|                 **certhash**                 |                                     Specifica l'hash SHA del certificato. Questo hash è lunga 20 byte e viene specificato come stringa esadecimale.                                      | Obbligatorio |
|                  **appid**                   |                                                                  Specifica il GUID per identificare l'applicazione proprietaria.                                                                  | Obbligatorio |
|              **certstorename**               |                                  Specifica il nome dell'archivio del certificato. Il valore predefinito è MY. Certificato deve essere archiviato nel contesto del computer locale.                                  | Facoltativo |
|        **verifyclientcertrevocation**        |                                                      Specifica la disattiva in/verifica delle revoche dei certificati client.                                                       | Facoltativo |
| **verifyrevocationwithcachedclientcertonly** |                                      Specifica se l'utilizzo del solo i certificati client nella cache per la verifica delle revoche di certificati è abilitato o disabilitato.                                       | Facoltativo |
|                **usagecheck**                |                                                      Specifica se il controllo dell'utilizzo viene abilitato o disabilitato. Impostazione predefinita è abilitato.                                                       | Facoltativo |
|         **revocationfreshnesstime**          | Specifica l'intervallo di tempo in secondi, per verificare la presenza di elenco di revoche di certificati (CRL) un certificato aggiornato. Se questo valore è zero, il nuovo CRL viene aggiornato solo se alla scadenza di quello precedente. | Facoltativo |
|           **urlretrievaltimeout**            |                            Specifica l'intervallo di timeout, in millisecondi, dopo il tentativo di recuperare l'elenco di revoche di certificati per l'URL remoto.                            | Facoltativo |
|             **sslctlidentifier**             |                Specifica l'elenco di autorità emittenti di certificati che può essere considerato attendibile. Questo elenco può essere un subset di autorità emittenti di certificati considerati attendibili dal computer.                 | Facoltativo |
|             **sslctlstorename**              |                                                Specifica il nome dell'archivio certificati in computer_locale archiviazione SslCtlIdentifier.                                                | Facoltativo |
|              **dsmapperusage**               |                                                        Specifica se il mapper di dominio Active Directory è abilitata o disabilitata. Valore predefinito è disabled.                                                         | Facoltativo |
|          **clientcertnegotiation**           |                                              Specifica se la negoziazione del certificato è abilitata o disabilitata. Valore predefinito è disabled.                                               | Facoltativo |

---

**Esempi**

Seguito è riportato un esempio del **aggiungere sslcert** comando.

Add sslcert ipport=0.0.0.0:443 certhash 1.1.1.1:443 = = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>aggiungere timeout

Aggiunge un timeout globale al servizio.

**Sintassi** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parametri**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo di timeout per l'impostazione.                                     |
|    **value**    | Valore di timeout (in secondi). Se il valore è in notazione esadecimale, aggiungere il prefisso 0 x. |

---

**Esempi**

Di seguito sono riportati due esempi del **aggiungere timeout** comando.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   aggiungere timeouttype timeout = valore headerwaittimeout 0x40 =

---

## <a name="add-urlacl"></a>aggiungere urlacl

Aggiunge una voce di prenotazione Uniform Resource Locator (URL). Questo comando di prenotare l'URL per gli account e utenti non amministratori. L'elenco DACL può essere specificato usando un nome di account NT con i parametri di ascolto e un delegato o utilizzando una stringa SDDL.

**Sintassi**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parametri**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Specifica il nome completo localizzatore URL (Uniform Resource).                                           | Obbligatorio |
|   **user**   |                                                      Specifica il nome utente o gruppo di utenti                                                       | Obbligatorio |
|  **listen**  | Specifica uno dei seguenti valori: Sì: Consentire all'utente di registrare gli URL. Rappresenta il valore predefinito. no: Negare all'utente tramite la registrazione degli URL. | Facoltativo |
| **delegate** |  Specifica uno dei seguenti valori: Sì: Consenti all'utente di delegare gli URL non: Negare la delega dell'URL all'utente. Rappresenta il valore predefinito.  | Facoltativo |
|   **sddl**   |                                                Specifica una stringa SDDL che descrive l'elenco DACL.                                                 | Facoltativo |

---

**Esempi**

Quattro esempi seguenti vengono illustrate le **aggiungere urlacl** comando.

- aggiungere url urlacl =https://+:80/MyUri utente = DOMAIN\\utente
- aggiungere url urlacl =<https://www.contoso.com:80/MyUri> utente = DOMAIN\\listen utente = Sì
- aggiungere url urlacl =<https://www.contoso.com:80/MyUri> utente = DOMAIN\\delegato dell'utente = no
- aggiungere url urlacl =https://+:80/MyUri sddl =...

---

## <a name="delete-cache"></a>eliminare cache

Elimina tutte le voci, o una voce, specificata dall'URI nella cache del kernel del servizio HTTP.

**Sintassi**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parametri**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Specifica il nome completo URL Uniform Resource Locator () che si desidera eliminare.                     | Facoltativo |
| **recursive** | Specifica se ottengano vengono rimosse tutte le voci nella cache url. **Sì**: rimuovere tutte le voci **alcun**: non rimuovere tutte le voci | Facoltativo |

---

**Esempi**

Di seguito sono riportati due esempi del **eliminare cache** comando.

- Elimina cache url =<https://www.contoso.com:80/myresource/> ricorsiva = Sì
- eliminare cache

---

## <a name="delete-iplisten"></a>Elimina iplisten

Elimina un indirizzo IP dall'elenco di ascolto IP. L'elenco di ascolto IP consente di definire l'ambito l'elenco di indirizzi a cui viene associato al servizio HTTP.

**Sintassi**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parametri**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Elenco di ascolto l'indirizzo IPv4 o IPv6 da eliminare dall'IP. L'elenco di ascolto IP consente di definire l'ambito l'elenco di indirizzi a cui viene associato al servizio HTTP. "0.0.0.0" significa che tutti gli indirizzi IPv4 e "::" si intende qualsiasi indirizzo IPv6. Questo non include il numero di porta. | Obbligatorio |

---


**Esempi**

Quattro esempi seguenti vengono illustrate le **eliminare iplisten** comando.

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>Elimina sslcert


Elimina le associazioni certificato server SSL e criteri di certificato client corrispondente per un indirizzo IP e porta.

**Sintassi**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parametri**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Specifica l'indirizzo IPv4 o IPv6 e la porta per il quale vengono eliminate le associazioni certificato SSL. Un carattere due punti (:) viene usato come delimitatore tra l'indirizzo IP e il numero di porta. | Obbligatorio |

---


**Esempi**

Tre esempi seguenti vengono illustrate le **eliminare sslcert** comando.

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- Elimina sslcert ipport = [::]: 443

---

## <a name="delete-timeout"></a>eliminare i timeout

Elimina un timeout globale e rende il servizio di ripristinare i valori predefiniti.

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

Di seguito sono riportati due esempi del **eliminare timeout** comando.

-   Elimina timeouttype timeout = idleconnectiontimeout
-   Elimina timeouttype timeout = headerwaittimeout

---

## <a name="delete-urlacl"></a>Delete urlacl

Elimina le prenotazioni URL.

**Sintassi**

```powershell
delete urlacl [ url= ] URL
```

**Parametri**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Specifica il nome completo URL Uniform Resource Locator () che si desidera eliminare. | Obbligatorio |

---


**Esempi**

Di seguito sono riportati due esempi del **delete urlacl** comando.

- Delete urlacl url =https://+:80/MyUri
- Delete urlacl url =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>scaricamento logbuffer

Scarica il buffer interno per i file di log.

**Sintassi**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Mostra cachestate

Gli elenchi memorizzati nella cache le risorse URI e le relative proprietà. Questo comando Elenca tutte le risorse e le proprietà associate che vengono memorizzati nella cache nella cache delle risposte HTTP oppure visualizza una singola risorsa e le relative proprietà associate.

**Sintassi**

```powershell
show cachestate [ [url= ] URL]
```

**Parametri**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL completo che si desidera visualizzare. Se non specificato, visualizzare tutti gli URL. L'URL può anche essere un prefisso di URL registrati. | Facoltativo |

---


**Esempi**

Di seguito sono riportati due esempi del **mostrare cachestate** comando:

- Mostra url cachestate =<https://www.contoso.com:80/myresource>
- Mostra cachestate

---

## <a name="show-iplisten"></a>Mostra iplisten

Visualizza tutti gli indirizzi IP nell'elenco di ascolto IP. L'elenco di ascolto IP consente di definire l'ambito l'elenco di indirizzi a cui viene associato al servizio HTTP. "0.0.0.0" significa che tutti gli indirizzi IPv4 e "::" si intende qualsiasi indirizzo IPv6.

**Sintassi**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Mostra servicestate

Consente di visualizzare uno snapshot del servizio HTTP.

**Sintassi**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parametri**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Visualizza**   | Specifica se visualizzare uno snapshot dello stato del servizio HTTP in base la sessione del server o le code di richiesta. | Facoltativo |
| **Verbose** |                Specifica se visualizzare informazioni dettagliate che illustra anche le informazioni sulle proprietà.                | Facoltativo |

---

**Esempi**

Di seguito sono riportati due esempi del **mostrare servicestate** comando.

-   show servicestate view="session"
-   show servicestate view="requestq"

---

## <a name="show-sslcert"></a>Mostra sslcert

Consente di visualizzare le associazioni di certificati server Secure Sockets Layer (SSL) e criteri di certificato client corrispondente per un indirizzo IP e porta.

**Sintassi**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parametri**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Specifica l'indirizzo IPv4 o IPv6 e la porta per cui il protocollo SSL certificato visualizzato Binding. Un carattere due punti (:) viene usato come delimitatore tra l'indirizzo IP e il numero di porta. Se non si specifica ipport, vengono visualizzate tutte le associazioni. | Obbligatorio |

---


**Esempi**

Cinque esempi seguenti vengono illustrate le **mostrare sslcert** comando.

-   Mostra sslcert ipport = [FE80:: 1]: 443
-   Mostra sslcert ipport = 1.1.1.1:443
-   show sslcert ipport=0.0.0.0:443
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

Controllo di accesso discrezionale consente di visualizzare elenchi (DACL) per l'URL riservato specificato o tutti gli URL riservati.

**Sintassi**

```powershell
show urlacl [ [url= ] URL]
```

**Parametri**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Specifica l'URL completo che si desidera visualizzare. Se non specificato, visualizzare tutti gli URL. | Facoltativo |

---


**Esempi**

Tre esempi seguenti vengono illustrate le **mostrare urlacl** comando.

- Mostra url urlacl =https://+:80/MyUri
- Mostra url urlacl =<https://www.contoso.com:80/MyUri>
- Mostra urlacl

---