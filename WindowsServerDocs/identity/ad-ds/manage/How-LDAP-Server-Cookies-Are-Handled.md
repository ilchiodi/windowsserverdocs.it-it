---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: Come vengono gestiti i cookie del Server LDAP
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 89369cb1e52a315520062ca5ecc96b66ac3e2bfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="how-ldap-server-cookies-are-handled"></a>Come vengono gestiti i cookie del Server LDAP

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In LDAP alcune query generano un risultato di grandi dimensione impostato. Tali query comportano delle difficoltà in Windows Server.  
  
Raccogliere e compilare questi set di risultati di grandi dimensioni rappresenta un impegno notevole. Molti degli attributi devono essere convertiti da una rappresentazione interna per la rappresentazione di collegamento LDAP. Per molti attributi, una conversione da un formato interno, spesso binario, deve essere eseguita in un formato UTF-8 basato su testo nel frame di risposta LDAP.  
  
Un altro problema è che i set di risultati con decine di migliaia di oggetti diventano enormi, facilmente diverse centinaia di megabyte. Questi quindi richiedono numerose virtuale dello spazio di indirizzi e anche il trasferimento tramite rete presenta problemi come tutto il lavoro viene perso se durante la sessione TCP si interrompe.  
  
Queste capacità e problemi logistiche hanno portato gli sviluppatori LDAP di Microsoft per la creazione di un'estensione LDAP nota come "Query di paging". Tale estensione implementa un controllo LDAP per separare una query di grandi dimensioni in blocchi di set di risultati più piccoli. È diventato uno standard RFC come [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Gestione dei cookie nel Client  
Il metodo Query di paging Usa le dimensioni di pagina impostate dal client o tramite un [criteri LDAP](https://support.microsoft.com/kb/315071/en-us) ("MaxPageSize"). Il client deve sempre abilitare il paging inviando un controllo LDAP.  

  
Quando si utilizza una query con molti risultati, a un certo punto viene raggiunto il numero massimo di oggetti consentiti. Il server LDAP il messaggio di risposta di pacchetti e aggiunge un cookie che contiene le informazioni necessarie per continuare la ricerca.  
  
L'applicazione client deve considerare il cookie come un blob opaco. È possibile recuperare il conteggio degli oggetti nella risposta e può continuare la ricerca in base alla presenza del cookie. Il client continua la ricerca inviando la query al server LDAP con gli stessi parametri come oggetto di base e il filtro e include il valore del cookie restituito nella risposta precedente.  
  
Se il numero di oggetti non riempie una pagina, la query LDAP è completa e la risposta non contiene alcun cookie di pagina. Se viene restituito alcun cookie dal server, il client deve considerare la ricerca di paging completata.  
  
Se viene restituito un errore dal server, il client deve considerare la ricerca di paging non riuscita. Nuovo tentativo in corso la ricerca si verificheranno riavviata dalla prima pagina.  
  
## <a name="server-side-cookie-handling"></a>Gestione dei Cookie sul lato server  
Windows Server restituisce il cookie al client e talvolta archivia le informazioni relative al cookie sul server. Queste informazioni vengono archiviate nel server in una cache e sono soggette a determinati limiti.  
  
In questo caso, il cookie inviato al client dal Server viene anche utilizzato dal server per cercare le informazioni dalla cache sul Server. Quando il client continua la ricerca di paging, Windows Server utilizzerà il cookie di client, nonché eventuali informazioni correlate nella cache cookie del server per continuare la ricerca. Se il server è possibile trovare informazioni correlate sui cookie nella cache del server a causa di un qualsiasi motivo, la ricerca viene interrotta e viene restituito l'errore al client.  
  
## <a name="how-the-cookie-pool-is-managed"></a>Come viene gestito il pool di cookie  
Ovviamente, il server LDAP gestisce più di un client alla volta, e anche più di un client in un momento possibile avviare una query che richiede l'uso di cache cookie del server. In questo modo l'implementazione di Windows Server prevede di tenere traccia dell'utilizzo del pool di cookie e limiti vengono allocati in modo che il pool di cookie non richieda una quantità eccessiva di risorse. I limiti possono essere impostati dall'amministratore usando le impostazioni seguenti nei criteri LDAP. Le impostazioni predefinite e le spiegazioni sono:  
  
**MinResultSets: 4**  
  
Il server LDAP non verrà esaminati la dimensione massima del pool descritte di seguito, se sono presenti inferiore a MinResultSets voci nella cache cookie del server.  
  
**MaxResultSetSize: 262.144 byte**  
  
Le dimensioni della cache cookie totale sul server non devono superare il numero massimo di MaxResultSetSize in byte. In caso affermativo, vengono eliminati i cookie a partire dal meno recenti fino a quando il pool di dimensioni inferiore a MaxResultSetSize byte o inferiore a MinResultSets cookie nel pool. Ciò significa che usando le impostazioni predefinite, il server LDAP considera un pool di 450KB se sono presenti solo 3 cookie archiviati.  
  
**MaxResultSetsPerConn: 10**  
  
Il server LDAP consente non cookie maggiore di MaxResultSetsPerConn per ogni connessione LDAP nel pool.  
  
## <a name="handling-deleted-cookies"></a>Gestione dei cookie eliminati  
La rimozione delle informazioni dei cookie dalla cache del Server LDAP non generano un errore immediato per le applicazioni in tutti i casi. Le applicazioni possono riavviare la ricerca di paging dall'inizio e completarla in un altro tentativo. Alcune applicazioni presentano questo tipo di un meccanismo di tentativi per ottenere una maggiore affidabilità.  
  
Alcune applicazioni possono passare attraverso una ricerca di pagina e senza mai completarla. Ciò potrebbe lasciare le voci del server LDAP cache dei cookie, che viene gestita mediante il meccanismo della sezione 4. Ciò è essenziale per liberare memoria nel server per le ricerche LDAP attive.  
  
Cosa accade quando un cookie viene eliminato nel server e il client continua la ricerca con questo handle del cookie? Il Server LDAP non troverà il cookie nel server cache dei cookie e restituire un errore per la query, la risposta di errore sarà simile a:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> Il valore esadecimale dietro "DSID" varia a seconda della versione di build dei file binari server LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Creazione di report nel pool di cookie  
Il Server LDAP è in grado di registrare eventi fino alla categoria "16 interfaccia Ldap" [chiave diagnostica NTDS](https://support.microsoft.com/kb/314980/en-us). Se si imposta questa categoria su "2", è possibile ottenere gli eventi seguenti:  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2898  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has reached the limit of the number of Result Sets it will maintain for a single connection.  A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.  
Maximum number of Result Sets allowed per LDAP connection:  
10  
Current number of Result Sets for this LDAP connection:  
11  
  
User Action  
The client should consider a more efficient search filter.  The limit for Maximum Result Sets per Connection may also be increased.  
  
```  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2899  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has exceeded the limit of the LDAP Maximum Result Set Size. A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.   
  
Number of result sets currently stored:   
4   
Current Result Set Size:   
263504   
Maximum Result Set Size:   
262144   
Size of single Result Set being discarded:   
40876   
User Action   
The client should consider a more efficient search filter.  The limit for Maximum Result Set Size may also be increased.  
  
```  
  
Gli eventi segnalano che è stato rimosso un cookie archiviato. Non significa che un client ha rilevato l'errore LDAP, ma solo che il Server LDAP ha raggiunto i limiti di amministrazione per la cache.  In alcuni casi, un client LDAP può abbia abbandonato la ricerca di paging e potrebbe non visualizzare l'errore.  
  
## <a name="monitoring-the-cookie-pool"></a>Monitoraggio il pool di cookie  
Se si sono mai verificano errori di ricerca LDAP nel dominio, non è necessario monitorare il pool di cookie ricerca di pagina LDAP server. Nel caso in cui viene visualizzato di pagina LDAP errori correlati alla ricerca nel proprio ambiente, potrebbe essere un problema con i limiti di amministrazione di pool di cookie.  
  
Gli eventi 2898 e 2899 rappresentano l'unico modo di sapere che il server LDAP ha raggiunto i limiti di amministrazione. Quando si verificano errori che le query LDAP a causa del controllo sopra errore di elaborazione, è necessario esaminare aumentare i limiti in uno o più delle impostazioni dei criteri LDAP menzionate nella sezione 4, a seconda di quale evento si desidera ottenere.  
  
Se nel Server LDAP al controller di dominio viene visualizzato l'evento 2898, è consigliabile che impostare MaxResultSetsPerConn su 25. Più di 25 ricerche di paging parallele in una singola connessione LDAP non è normale. Se si continua l'evento 2898 persiste, prendere in considerazione l'applicazione client LDAP che rileva l'errore di analisi. Il potrebbe essere possibile che in qualche modo Ottiene bloccata durante il recupero altri risultati di paging, lasciando il cookie in sospeso e riavvia una nuova query. Pertanto, vedere se l'applicazione a un certo punto avrebbe cookie sufficienti per lo scopo è anche possibile aumentare il valore di MaxResultSetsPerConn 25. quando viene visualizzato l'evento 2899 nei controller di dominio, la soluzione sarà diversa. Se il server LDAP al controller di dominio viene eseguita in un computer con memoria sufficiente (diversi GB di memoria disponibile), è consigliabile impostare MaxResultsetSize al server LDAP > = 250MB. Questo limite è sufficiente per soddisfare volumi elevati di ricerche di pagina LDAP anche in directory di dimensioni molto grandi.  
  
Se si è comunque visualizzare l'evento 2899 con un pool di 250MB o più, è probabile che molti client con un numero elevato di oggetti restituiti, eseguire una query in modo molto frequente. I dati è possibile raccogliere con il [insieme agenti di raccolta dati di Active Directory](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) possibile disponibilità consentono di trovare le query di paging ripetitive che mantengono i server LDAP. Tali query verranno visualizzate con un numero di "Voci restituite" che corrisponde alle dimensioni della pagina usata.  
  
Se possibile, si deve rivedere la progettazione dell'applicazione e implementare un approccio diverso con una frequenza inferiore, volume di dati e/o una query di questi dati meno istanze del client. In caso di applicazioni per cui si dispone di accesso al codice sorgente, questa guida per [la creazione di applicazioni AD-Enabled efficiente](https://msdn.microsoft.com/en-us/library/ms808539.aspx) può aiutarti a comprendere il modo ottimale per le applicazioni per l'accesso ad Active Directory.  
  
Se non è possibile modificare il comportamento della query, un approccio che consiste nell'aggiungere più istanze replicate dei contesti dei nomi necessari e per ridistribuire i client e alla fine di ridurre il carico sui singoli server LDAP.  
  


