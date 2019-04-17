---
title: Novità di Server DNS in Windows Server
description: In questo argomento viene fornita una panoramica delle nuove funzionalità in Server DNS in Windows Server 2016 e versioni successive
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ddb8920c045f231dcf5286283d9895ef6ffff47
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Novità di Server DNS in Windows Server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento descrive le funzionalità server di sistema DNS (Domain Name) nuove o modificate in Windows Server 2016.  
  
In Windows Server 2016, Server DNS offre supporto avanzato nelle aree seguenti.  
  
|Funzionalità|Nuove o migliorate|Descrizione|  
|-----------------|-------------------|---------------|  
|Criteri DNS|Nuovo|È possibile configurare criteri DNS per specificare come un server DNS risponde alle query DNS. Le risposte DNS possono essere basate sull'indirizzo IP client (posizione), ora del giorno e molti altri parametri. Criteri di DNS consentono DNS con riconoscimento della posizione, la gestione del traffico, il bilanciamento del carico, DNS "split Brain" e altri scenari.|  
|Velocità di risposta limitante (RRL)|Nuovo|Per abilitare la limitazione della velocità di risposta dei server DNS. In questo modo si evita la possibilità di sistemi dannoso tramite i server DNS per avviare un attacco denial of service in un client DNS.|  
|Autenticazione basata su DNS di entità denominate (DANE)|Nuovo|È possibile utilizzare i record TLSA (autenticazione di sicurezza Layer di trasporto) per fornire informazioni al client DNS che indicano quali CA deve prevedono un certificato da per il nome di dominio. Questo impedisce gli attacchi man-in-the-middle in cui un utente potrebbe danneggiare la cache DNS in modo che punti al proprio sito Web e fornire un certificato da un'autorità di certificazione diversi.|  
|Supporto di record sconosciuto|Nuovo|È possibile aggiungere i record che non sono supportati in modo esplicito dal server DNS di Windows utilizzando la funzionalità di record sconosciuto.|  
|Parametri radice IPv6|Nuovo|È possibile utilizzare la connettività IPV6 nativa supportano i parametri radice per eseguire la risoluzione dei nomi internet utilizzando il server principali di IPV6.|  
|Supporto di Windows PowerShell|Migliorato|Nuovi cmdlet di Windows PowerShell sono disponibili per il Server DNS.|  
  
## <a name="dns-policies"></a>Criteri DNS

È possibile utilizzare criteri DNS per la gestione del traffico basato su posizione geografica, risposte DNS basate sull'ora del giorno, per gestire un singolo server DNS configurato per la distribuzione "split Brain", l'applicazione di filtri query DNS e altro ancora. Gli elementi seguenti forniscono altri dettagli su queste funzionalità.

-   **Bilanciamento carico di applicazioni.** Quando si distribuiscono più istanze di un'applicazione in posizioni diverse, è possibile utilizzare criteri DNS per bilanciare il carico del traffico tra le istanze di applicazioni diverso, allocazione dinamica il carico del traffico per l'applicazione.

-   **Gestione del traffico basata sulla Geo\-Location.** È possibile utilizzare criteri DNS per consentire ai server DNS primari e secondari rispondere alle query client DNS in base alla posizione geografica del client e la risorsa a cui il client sta tentando di connettersi, fornendo il client con l'indirizzo IP della risorsa più vicina. 

-   **Dividere cervello DNS.** Con DNS "split Brain", i record DNS sono suddivisi in diversi ambiti di zona nello stesso server DNS e client DNS ricevano una risposta basata sul fatto che i client si trovano i client interni o esterni. È possibile configurare DNS "split Brain" per le zone integrate con Active Directory o per le zone in server DNS autonomi.

-   **Il filtro.** È possibile configurare criteri DNS per creare i filtri di query basate su criteri specificati. Filtri di query in Criteri di DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e client DNS che invia la query DNS. 
-   **Analisi forense.** È possibile utilizzare criteri DNS per reindirizzare i client DNS non autorizzati a un indirizzo IP esistente anziché al computer che stanno tentando di raggiungere.

-   **Ora del giorno in base reindirizzamento.** È possibile utilizzare criteri DNS per distribuire il traffico dell'applicazione tra diverse istanze distribuite geograficamente di un'applicazione utilizzando i criteri DNS basate sull'ora del giorno. 
  
È inoltre possibile utilizzare criteri DNS per DNS integrate in Active Directory zone.

Per ulteriori informazioni, vedere il [Guida agli scenari dei criteri DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitazione della velocità di risposta

È possibile configurare le impostazioni di RRL per stabilire come rispondere alle richieste di un client DNS quando il server riceve molte richieste per lo stesso client. In questo modo è possibile impedire l'invio di un attacco Denial of Service (Dos) tramite i server DNS. Ad esempio, un componente di rete possono inviare richieste al server DNS utilizzando l'indirizzo IP di un terzo computer come il richiedente. Senza RRL, i server DNS potrebbero rispondere a tutte le richieste, un numero eccessivo di terzo computer. Quando si utilizza RRL, è possibile configurare le impostazioni seguenti:  
  
-   **Le risposte al secondo**. Questo è il numero massimo di volte in cui che la stessa risposta verrà assegnata a un client all'interno di un secondo.  
  
-   **Errori al secondo**. Questo è il numero massimo di volte in cui che verrà inviata una risposta di errore per lo stesso client all'interno di un secondo.  
  
-   **Finestra**. Questo è il numero di secondi per cui le risposte a un client verranno sospeso se vengono apportate numero eccessivo di richieste.  
  
-   **Tasso di perdita**. Questa è la frequenza con cui il server DNS risponderà a una query durante la fase di risposte vengono sospesi. Ad esempio, se il server sospende le risposte a un client per 10 secondi e il tasso di perdita di memoria è 5, il server ancora risponderà a una query per ogni 5 query inviati. In questo modo i client autorizzati ottenere le risposte anche quando il server DNS è l'applicazione di velocità di risposta limitazione nella loro subnet o un nome di dominio completo.  
  
-   **Frequenza TC**. Viene utilizzato per indicare al client di provare a connettersi con TCP quando le risposte al client vengono sospesi. Ad esempio, se la velocità di TC è 3 e il server sospende le risposte a un determinato client, il server genererà una richiesta di connessione TCP per ogni 3 query ricevuto. Assicurarsi che il valore di frequenza TC è inferiore al tasso di perdita, per consentire il client di connettersi tramite TCP prima una perdita di risposte.  
  
-   **Risposte massime**. Questo è il numero massimo di risposte che server rilascerà a un client mentre le risposte vengono sospesi.  
  
-   **Elenco approvato domini**. Questo è un elenco di domini da escludere dalle impostazioni RRL.  
  
-   **Elenco approvato subnet**. Questo è un elenco di subnet da escludere dalle impostazioni RRL.  
  
-   **Interfacce server elenco approvato**. Questo è un elenco delle interfacce di server DNS da escludere dalle impostazioni RRL.  
  
## <a name="dane-support"></a>Supporto DANE

È possibile utilizzare il supporto di DANE \ (RFC 6394 e 6698\) per specificare per i client DNS cosa deve prevedono l'emissione da per i nomi di domini di certificati CA ospitato nel server DNS. Questo impedisce un tipo di attacco man-in-the-middle in cui un utente è in grado di danneggiare una cache DNS e scegliere un nome DNS per il proprio indirizzo IP.  
  
Si supponga ad esempio si ospita un sito Web protetto che utilizza SSL in www.contoso.com mediante un certificato da un'autorità noto denominato CA1. Qualcuno potrebbe comunque essere in grado di ottenere un certificato per www.contoso.com da un diverso, non in modo-noti, certificato autorità denominata CA2. Quindi, l'entità che ospita il sito Web www.contoso.com fittizio potrebbe essere in grado di danneggiare la cache DNS di un client o server in modo da puntare www.contoto.com al rispettivo sito fittizio. L'utente finale verrà visualizzato un certificato da CA2, può semplicemente confermare e connettersi al sito fittizio. Con DANE, il client potrebbe effettuare una richiesta al server DNS per contoso.com che richiede il record TLSA e informazioni che il certificato per www.contoso.com è problemi da CA1. Se viene visualizzata con un certificato da un'altra CA, la connessione viene interrotta.  
  
## <a name="unknown-record-support"></a>Supporto di record sconosciuto

Un Record"sconosciuto" è un record il cui formato RDATA non è noto al server DNS. Il supporto aggiunto per i tipi di record sconosciuto (RFC 3597) significa che è possibile aggiungere i tipi di record non supportati nelle zone DNS di Windows server nel formato binario in transito. Il windows resolver di memorizzazione nella cache è in grado di elaborare i tipi di record sconosciuto. Server DNS di Windows operazione verrà eseguita alcuna elaborazione record specifici per i record sconosciuti, ma verrà inviarlo nuovamente nelle risposte se vengono ricevute le query per tale.  
  
## <a name="ipv6-root-hints"></a>Parametri radice IPv6

I parametri principali IPV6, pubblicati da IANA, sono stati aggiunti al server DNS di windows. Le query di nome internet server principali di IPv6 ora è possono utilizzare per l'esecuzione di risoluzioni dei nomi.

## <a name="windows-powershell-support"></a>Supporto di Windows PowerShell

I parametri e i nuovi cmdlet di Windows PowerShell seguenti vengono introdotti in Windows Server 2016.
  
-   **Aggiungere DnsServerRecursionScope**. Questo cmdlet crea un nuovo ambito di ricorsione nel server DNS. Gli ambiti di ricorsione vengono utilizzati dai criteri DNS per specificare un elenco di server d'inoltro da utilizzare in una query DNS.  
  
-   **Rimuovi DnsServerRecursionScope**. Questo cmdlet rimuove ambiti ricorsione esistenti.  
  
-   **Set DnsServerRecursionScope**. Questo cmdlet modifica le impostazioni di un ambito di ricorsione esistente.  
  
-   **DnsServerRecursionScope Get**. Questo cmdlet recupera le informazioni sugli ambiti di ricorsione esistente.  
  
-   **Aggiungere DnsServerClientSubnet**. Questo cmdlet crea una nuova subnet client DNS. Subnet vengono utilizzate dai criteri DNS per identificare un client DNS in cui si trova.  
  
-   **Rimuovi DnsServerClientSubnet**. Questo cmdlet rimuove subnet del client DNS esistente.  
  
-   **Set DnsServerClientSubnet**. Questo cmdlet modifica le impostazioni di una subnet client DNS esistente.  
  
-   **DnsServerClientSubnet Get**. Questo cmdlet recupera le informazioni sulle subnet del client DNS esistente.  
  
-   **Aggiungere DnsServerQueryResolutionPolicy**. Questo cmdlet crea un nuovo criterio di risoluzione di query DNS. Criteri di risoluzione di query DNS vengono utilizzati per specificare come, o se una query viene risposto, in base a criteri diversi.  
  
-   **Rimuovi DnsServerQueryResolutionPolicy**. Questo cmdlet rimuove i criteri DNS esistenti.  
  
-   **Set DnsServerQueryResolutionPolicy**. Questo cmdlet modifica le impostazioni di un criterio DNS esistente.  
  
-   **DnsServerQueryResolutionPolicy Get**. Questo cmdlet recupera le informazioni sui criteri DNS esistente.  
  
-   **Enable-DnsServerPolicy**. Questo cmdlet Abilita criteri DNS esistenti.  
  
-   **Disable-DnsServerPolicy**. Questo cmdlet Disabilita criteri DNS esistenti.  
  
-   **Aggiungere DnsServerZoneTransferPolicy**. Questo cmdlet crea un nuovo criterio di trasferimento di zona server DNS. Criteri di trasferimento di zona DNS specificano se negare o ignorare un trasferimento di zona in base a criteri diversi.  
  
-   **Rimuovi DnsServerZoneTransferPolicy**. Questo cmdlet rimuove i criteri di trasferimento zona DNS server esistenti.  
  
-   **Set DnsServerZoneTransferPolicy**. Questo cmdlet modifica le impostazioni di un criterio di trasferimento zona di server DNS esistente.  
  
-   **Get-DnsServerResponseRateLimiting**. Questo cmdlet recupera le impostazioni di RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Questo cmdlet modifica le impostazioni RRL.  
  
-   **Aggiungere DnsServerResponseRateLimitingExceptionlist**. Questo cmdlet crea un elenco di eccezioni RRL nel server DNS.  
  
-   **DnsServerResponseRateLimitingExceptionlist Get**. Questo cmdlet recupera gli elenchi di excception RRL.  
  
-   **Rimuovi DnsServerResponseRateLimitingExceptionlist**. Questo cmdlet rimuove un elenco di eccezioni RRL esistente.  
  
-   **Set DnsServerResponseRateLimitingExceptionlist**. Questo cmdlet modifica RRL elenchi di eccezioni.  
  
-   **Aggiungere DnsServerResourceRecord**. Questo cmdlet è stato aggiornato per supportare il tipo di record sconosciuto.  
  
-   **DnsServerResourceRecord Get**. Questo cmdlet è stato aggiornato per supportare il tipo di record sconosciuto.  
  
-   **Rimuovi DnsServerResourceRecord**. Questo cmdlet è stato aggiornato per supportare il tipo di record sconosciuto.  
  
-   **Set DnsServerResourceRecord**. Questo cmdlet è stato aggiornato per supportare il tipo di record sconosciuto

Per ulteriori informazioni, vedere gli argomenti di riferimento di comando Windows Server 2016, Windows PowerShell seguenti.

- [Modulo server DNS](https://technet.microsoft.com/itpro/powershell/windows/dns-server/index)
- [Modulo DnsClient](https://technet.microsoft.com/itpro/powershell/windows/dns-client/index)

## <a name="see-also"></a>Vedere anche  
  
-   [Novità di Client DNS](What-s-New-in-DNS-Client.md)  
  

  

