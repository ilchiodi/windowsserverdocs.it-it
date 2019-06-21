---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: Come vengono gestiti i cookie del server LDAP
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 873953155d22bafef5b042887b22e953ff580b5c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280577"
---
# <a name="how-ldap-server-cookies-are-handled"></a>Come vengono gestiti i cookie del server LDAP

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In LDAP alcune query generano un set di risultati di grandi dimensioni. Tali query comportano delle difficoltà in Windows Server.  
  
Raccogliere e compilare questi set di risultati di grandi dimensioni rappresenta un impegno notevole. Molti degli attributi devono essere convertiti da una rappresentazione interna nella rappresentazione di collegamento LDAP. Per molti attributi, la conversione da un formato interno, spesso binario, deve essere eseguita in un formato UTF-8 basato su testo nel frame di risposta LDAP.  
  
Un altro problema è costituito dal fatto che i set di risultati con decine di migliaia di oggetti diventano enormi, raggiungendo spesso diverse centinaia di megabyte richiedendo pertanto una quantità elevata di spazio degli indirizzi virtuali. Anche il trasferimento tramite rete presenta dei problemi in quanto tutto il lavoro viene perso se durante il trasferimento la sessione TCP si interrompe.  
  
Queste capacità e i problemi di logistici hanno portato gli sviluppatori LDAP di Microsoft per la creazione di un'estensione LDAP nota come "Query di paging". Tale estensione implementa un controllo LDAP per separare una query di grandi dimensioni in blocchi di set di risultati più piccoli. È diventato uno standard RFC come [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Gestione dei cookie nel client  
Il metodo di Query di paging Usa le dimensioni di pagina impostate dal client o tramite un [criteri LDAP](https://support.microsoft.com/kb/315071/en-us) ("MaxPageSize"). Il client deve sempre abilitare il paging inviando un controllo LDAP.  

  
Quando si usa una query con molti risultati, a un certo punto viene raggiunto il numero massimo di oggetti consentiti. Il server LDAP suddivide il messaggio di risposta in pacchetti e aggiunge un cookie che contiene le informazioni necessarie per continuare la ricerca.  
  
L'applicazione client deve considerare il cookie come un BLOB opaco. Può recuperare il conteggio degli oggetti nella risposta e può continuare la ricerca in base alla presenza del cookie. Il client continua la ricerca inviando di nuovo la query al server LDAP con gli stessi parametri come l'oggetto e il filtro di base e include il valore del cookie restituito nella risposta precedente.  
  
Se il numero di oggetti non riempie una pagina, la query LDAP risulterà completa e la risposta non contiene alcun cookie di pagina. Se dal server non viene restituito alcun cookie, il client deve considerare la ricerca di paging completata.  
  
Se dal server viene restituito un errore, il client deve considerare la ricerca di paging non riuscita. Se si riprova, la ricerca verrà riavviata dalla prima pagina.  
  
## <a name="server-side-cookie-handling"></a>Gestione dei cookie sul lato server  
Windows Server restituisce il cookie al client e talvolta archivia le informazioni relative al cookie sul server. Queste informazioni vengono archiviate nel server in una cache e sono soggette a determinati limiti.  
  
In questo caso, il cookie inviato al client dal server viene usato anche dal server per cercare le informazioni presenti nella cache del server. Quando il client continua la ricerca di paging, Windows Server userà il cookie del client, nonché tutte le informazioni correlate presenti nella cache dei cookie del server per continuare la ricerca. Se per un qualsiasi motivo il server non riesce a trovare le informazioni correlate sui cookie nella cache del server, la ricerca viene interrotta e viene restituito un errore al client.  
  
## <a name="how-the-cookie-pool-is-managed"></a>Come viene gestito il pool di cookie  
Ovviamente, il server LDAP gestisce più di un client alla volta ed è anche possibile che più client alla volta avviino una query che richiede l'uso della cache dei cookie del server. Pertanto l'implementazione di Windows Server prevede di tenere traccia dell'utilizzo del pool di cookie e vengono applicati alcuni limiti in modo tale che il pool di cookie non richieda una quantità eccessiva di risorse. I limiti possono essere impostati dall'amministratore usando le seguenti impostazioni nei criteri LDAP. Le impostazioni predefinite e le spiegazioni sono:  
  
**MinResultSets: 4**  
  
Il server LDAP non considera le dimensioni massime del pool descritte di seguito, se nella cache dei cookie del server è presente un numero inferiore di voci rispetto a MinResultSets.  
  
**MaxResultSetSize: 262.144 byte**  
  
Le dimensioni totali della cache dei cookie nel server non devono superare il numero massimo di MaxResultSetSize in byte. Altrimenti, vengono eliminati i cookie meno recenti fino a quando le dimensioni in byte del pool non sono inferiori a MaxResultSetSize o nel pool non sia presente un numero di cookie inferiore a MinResultSets. Ciò significa che usando le impostazioni predefinite, il server LDAP considera come valore ottimale un pool di 450 KB se sono archiviati solo 3 cookie.  
  
**MaxResultSetsPerConn: 10**  
  
Il server LDAP non consente un numero di cookie maggiore di MaxResultSetsPerConn per ogni connessione LDAP nel pool.  
  
## <a name="handling-deleted-cookies"></a>Gestione dei cookie eliminati  
La rimozione delle informazioni dei cookie dalla cache del server LDAP non comporta sempre un errore immediato per le applicazioni. Le applicazioni possono riavviare la ricerca di paging dall'inizio e completarla in un altro tentativo. Alcune applicazioni presentano questo tipo di meccanismo di gestione dei tentativi per ottenere una maggiore affidabilità.  
  
Alcune applicazioni possono eseguire una ricerca di pagina senza mai completarla. Ciò potrebbe lasciare le voci nella cache dei cookie del server LDAP, che verranno gestite in base al meccanismo della sezione 4. È importante liberare memoria nel server per le ricerche LDAP attive.  
  
Cosa accade quando un cookie viene eliminato nel server e il client continua la ricerca con questo handle del cookie? Il server LDAP non troverà il cookie nella cache dei cookie del server e restituirà un errore per la query. La risposta di errore sarà simile alla seguente:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> Il valore esadecimale dietro "DSID" variano a seconda della versione di build dei file binari del server LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Creazione di report nel pool di cookie  
Il Server LDAP ha la possibilità di registrare eventi fino alla categoria "16 interfaccia Ldap" [chiave diagnostica NTDS](https://support.microsoft.com/kb/314980/en-us). Se si imposta questa categoria su "2", è possibile ottenere gli eventi seguenti:  
  
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
  
Gli eventi segnalano che è stato rimosso un cookie archiviato. Questo non significa che un client ha rilevato l'errore LDAP, ma solo che il server LDAP ha raggiunto i limiti di amministrazione per la cache.  In alcuni casi, è possibile che un client LDAP abbia abbandonato la ricerca di paging e quindi potrebbe non visualizzare l'errore.  
  
## <a name="monitoring-the-cookie-pool"></a>Monitoraggio del pool di cookie  
Se non si sono mai verificati errori di ricerca LDAP nel dominio, potrebbe non essere necessario monitorare il pool di cookie di ricerca di pagina del server LDAP. Nel caso in cui nell'ambiente usato si verificano errori correlati alla ricerca di pagina di LDAP, si potrebbe avere un problema con i limiti di amministrazione del pool di cookie.  
  
Gli eventi 2898 e 2899 rappresentano l'unico modo per sapere se il server LDAP ha raggiunto i limiti di amministrazione. Quando si verifica che le query LDAP restituiscono un errore a causa dell'errore di elaborazione dei controlli indicato in precedenza, è consigliabile aumentare i limiti in una o più delle impostazioni dei criteri LDAP menzionate nella sezione 4, a seconda dell'evento ricevuto.  
  
Se nel controller di dominio o nel server LDAP viene visualizzato l'evento 2898, è consigliabile impostare MaxResultSetsPerConn su 25. In genere non si eseguono più di 25 ricerche di paging parallele in una singola connessione LDAP. Se l'evento 2898 persiste, provare a cercare la causa dell'errore nell'applicazione client LDAP. Potrebbe essere possibile che l'applicazione rimanga in qualche modo bloccata durante il recupero di altri risultati di paging lasciando il cookie in sospeso e riavviando quindi una nuova query. Per questo motivo è opportuno valutare se l'applicazione dispone di cookie sufficienti per lo scopo. È anche possibile aumentare il valore di MaxResultSetsPerConn impostando un valore maggiore di 25. Quando invece nei controller di dominio viene visualizzato l'evento 2899, la soluzione sarà diversa. Se il controller di dominio o il server LDAP viene eseguito in un computer con memoria sufficiente (diversi GB di memoria disponibile), è consigliabile impostare MaxResultsetSize su un valore maggiore di 250 MB nel server LDAP. Questo limite è sufficiente per soddisfare volumi elevati di ricerche di pagina LDAP anche in directory di dimensioni molto grandi.  
  
Se l'evento 2899 persiste con un pool di 250 MB o più, è probabile che molti client con un numero elevato di oggetti restituiti abbiamo eseguito una query con un'elevata frequenza. I dati è possibile raccogliere con il [insieme agenti di raccolta dati di Active Directory](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) può occupato consentono di trovare query di paging ripetitive che mantengono i server LDAP. Tali query verranno visualizzate con un numero di "Voci restituite" che corrisponde alle dimensioni della pagina usata.  
  
Se possibile, è necessario rivedere la progettazione dell'applicazione e implementare un approccio diverso con una frequenza inferiore, volume di dati e/o meno istanze del client relative a questi dati. In caso di applicazioni per cui si dispone di accesso al codice sorgente, questa guida per [creazione di applicazioni efficienti AD-Enabled](https://msdn.microsoft.com/library/ms808539.aspx) possono aiutare a comprendere il modo ottimale per l'accesso AD delle applicazioni.  
  
Se non è possibile modificare il comportamento di query, un approccio consiste nell'aggiungere più istanze replicate dei contesti dei nomi necessiti per ridistribuire i client e alla fine di ridurre il carico sui singoli server LDAP.  
  


