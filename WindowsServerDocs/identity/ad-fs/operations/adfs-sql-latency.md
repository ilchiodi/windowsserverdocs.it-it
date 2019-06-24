---
title: Ottimizzazione di SQL e risoluzione dei problemi di latenza con AD FS
description: Questo documento illustra come ottimizzare SQL con AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb699a1f92013f5657d2fbb48b203f96a5e5a5ba
ms.sourcegitcommit: 6b6c3601fb7493ab145ccff02db26d7123df9a3d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322859"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Ottimizzazione di SQL e risoluzione dei problemi di latenza con AD FS
In un aggiornamento per [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) abbiamo introdotto i miglioramenti seguenti per ridurre tra latenza del database. Un aggiornamento futuro per AD FS 2019 includerà questi miglioramenti.

## <a name="in-memory-cache-update-in-background-thread"></a>Aggiornamento della cache in memoria nel thread in background 
In previo sempre in distribuzioni di disponibilità (AoA), la latenza esistenti per una "Operazione di lettura" come è stato possibile individuare il nodo master in un Data Center separato. La chiamata tra due diversi Data Center ha comportato la latenza.  

Nell'ultimo aggiornamento ad AD FS, una riduzione della latenza di destinazione mediante l'aggiunta di un thread in background per aggiornare la cache di configurazione di AD FS e un'impostazione per impostare il periodo di tempo l'aggiornamento. Il tempo impiegato per una ricerca nel database viene notevolmente ridotto nel thread di richiesta, come gli aggiornamenti della cache di database vengono spostati nel thread in background.  

Quando il `backgroundCacheRefreshEnabled` è impostato su true, ADFS abiliterà il thread in background eseguire aggiornamenti della cache. La frequenza di recupero dei dati dalla cache può essere personalizzata con un valore di ora impostando `cacheRefreshIntervalSecs`. Il valore predefinito è impostato su 300 secondi quando `backgroundCacheRefreshEnabled` è impostato su true. Dopo che il set di valore di durata, AD FS inizia l'aggiornamento della cache relativo e mentre è in corso l'aggiornamento, i dati della cache precedente continuerà a essere utilizzato.  

>[!NOTE]
> Dati della cache verranno aggiornati di fuori del `cacheRefreshIntervalSecs` valore se ADFS riceve una notifica da SQL a indicare che si è verificata una modifica nel database. Questa notifica verrà attivata la cache per essere aggiornati. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Raccomandazioni per l'impostazione di aggiornamento della cache 
Il valore predefinito per l'aggiornamento della cache viene **cinque minuti**. È consigliabile impostarlo **1 ora** per ridurre un aggiornamento di dati non necessari da AD FS, perché verranno aggiornati i dati della cache se si verificano le modifiche SQL.  

AD FS registra un callback per le modifiche SQL e dopo una modifica, ad FS riceve una notifica. Tramite questo metodo, ad FS riceve ogni nuova modifica da SQL, non appena si verifica. 

In caso di un problema di rete determinando in AD FS manca la notifica di SQL, AD FS verrà aggiornato nell'intervallo specificato dalla cache aggiorni il valore. Se eventuali problemi di connettività sono sospetti tra ADFS e SQL, è consigliabile impostare il valore di aggiornamento della cache a minore di 1 ora.  

### <a name="configuration-instructions"></a>Istruzioni di configurazione 
Il file di configurazione supporta più voci della cache. Di seguito elencate di seguito può essere configurate in base alle esigenze dell'organizzazione. 

Nell'esempio seguente Abilita l'aggiornamento della cache in background e imposta il periodo di aggiornamento della cache su 1800 secondi o 30 minuti. Questa operazione deve essere eseguita in ogni nodo di ad FS e il servizio ADFS deve essere riavviato in un secondo momento. Le modifiche non compromettere gli altri nodi e testare il primo nodo prima di apportare la modifica in tutti i nodi. 

  1. Passare al file di configurazione di AD FS e nella sezione "Microsoft.IdentityServer.Service", aggiungere la seguente voce:  
  
  - `backgroundCacheRefreshEnabled`  -Specifica se è abilitata la funzionalità di cache in background. valori "true/false".
  - `cacheRefreshIntervalSecs` -Valore in secondi in cui ADFS aggiornerà la cache. Se si verifica alcun cambiamento in SQL, AD FS aggiornerà la cache. AD FS riceverà una notifica di SQL e aggiornare la cache.  
 
 >[!NOTE]
 > Tutte le voci nel file di configurazione sono tra maiuscole e minuscole.  
 &lt;cache cacheRefreshIntervalSecs="1800" backgroundCacheRefreshEnabled="true" /&gt; 
 
Altri valori configurabili supportati: 

   - **maxRelyingPartyEntries** - massimo numero di voci di terze parti relying party che continui ad AD FS in memoria. Questo valore viene utilizzato anche per la cache di autorizzazione oAuth dell'applicazione. Se sono presenti altre autorizzazioni per l'applicazione più richieste al secondo e se sarà essere tutte archiviate in memoria, questo valore deve essere il numero di autorizzazioni per l'applicazione. Il valore predefinito è 1000.
   - **maxIdentityProviderEntries** - questo è il numero massimo di attestazioni AD FS verrà mantenuto nella memoria le voci di provider. Il valore predefinito è 200. 
   - **maxClientEntries** -questo è il numero massimo di voci di client OAuth ADFS verrà mantenuto nella memoria. Il valore predefinito è 500. 
   - **maxClaimDescriptorEntries** - massimo numero di voci di descrittore attestazioni AD FS verrà mantenuto nella memoria. Il valore predefinito è 500. 
   - **maxNullEntries** -viene utilizzato come cache negativa. Quando viene eseguita la ricerca per una voce nel database e non viene trovato, AD FS aggiunge nella cache negativa. Questa è la dimensione massima della cache. Di ogni tipo di oggetti di negativo cache, non è una singola cache per tutti gli oggetti. Il valore predefinito è 50,0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Elemento più DB supporta in diversi Data Center 
Per le configurazioni precedenti di più Data Center, ADFS è supportato solo un singolo database, dell'elemento causando center tra Data Center latenza durante le chiamate di recupero.  

Per ridurre la latenza tra Data Center, un amministratore di AD FS possa ora distribuire più istanze di database dell'elemento e quindi modificare il file di configurazione del nodo un ADFS in modo che punti a istanze diverse dell'artefatto DB. È possibile specificare la stringa di connessione del database dell'elemento nel file di configurazione che consente a un database di artefatto per ogni nodo. Se la stringa di connessione non è presente all'interno del file di configurazione, il nodo tornerà alla progettazione precedente per usare il database dell'elemento che è presente nella configurazione del database.  
Gli ambienti ibridi sono anche supportati con questa configurazione.  

### <a name="requirements"></a>Requisiti: 
Prima di configurare il supporto per database multipli di artefatto, eseguire un aggiornamento in tutti i nodi e aggiornare i file binari poiché le chiamate a più nodi si verificano tramite questa funzionalità. 
  1. Generare script di distribuzione per creare il database dell'artefatto: Per distribuire più istanze di database dell'elemento, un amministratore dovrà generare lo script di distribuzione di SQL per il database dell'elemento. Come parte di questo aggiornamento, l'oggetto esistente `Export-AdfsDeploymentSQLScript`cmdlet è stato aggiornato per eseguire facoltativamente in un parametro che specifica il database di ADFS da generare uno script di distribuzione SQL. 
 
 Ad esempio, per generare lo script di distribuzione per appena il database dell'elemento, specificare il `-DatabaseType` parametro e passare il valore "Elemento". L'opzione facoltativa `-DatabaseType` parametro specifica il tipo di database di AD FS e può essere impostato su: Tutte (predefinito), dell'elemento o alla configurazione. Se nessun `-DatabaseType` viene specificato, lo script configurerà gli script di configurazione sia l'artefatto.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
Lo script generato deve essere eseguito nella macchina virtuale di SQL per creare i database necessari e assegnare l'account del servizio AD FS, le autorizzazioni a SA di SQL per tali database.

 2. Creare il database dell'elemento usando lo script di distribuzione. Copiare gli script di distribuzione appena generati createdb. SQL e SetPermissions.sql al computer di SQL server ed eseguire in modo da creare il database dell'elemento locale. 
 
 3. Modificare il file di configurazione per aggiungere la connessione al database dell'elemento. 
 Passare al file di configurazione del nodo AD FS e, nella sezione "Microsoft.IdentityServer.Service", aggiungere un punto di ingresso per il ArtifactDB appena configurata. 

 >[!NOTE] 
 > artifactStore e connectionString sono valori distinzione maiuscole / minuscole. Assicurarsi che siano configurati correttamente. &lt;artifactStore connectionString = "Data Source =. \SQLInstance; Integrated Security = True; Initial Catalog = AdfsArtifactStore" /&gt; 
>
>Usare un valore di origine dati che corrisponde alla connessione sql.



 4. Riavviare il servizio AD FS rendere effettive le modifiche. 
 
 >[!NOTE] 
 > È consigliabile non usare replica di SQL o la sincronizzazione tra i database dell'elemento. Si consiglia di configurare un database di artefatto per ogni Data Center. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Tra Data Center failover e ripristino del database  
Si consiglia di creare database artefatto failover su stesso Data Center del database master dell'artefatto. Se si verifica un failover, non vi sarà alcun aumento della latenza. I database dell'artefatto di failover tra Data Center non è consigliata. Il seguente illustra in dettaglio come le chiamate per OAuth, SAML, ESL e token Riproduci funzione di rilevamento con più database dell'elemento. 
 - **OAuth e SAML** 

   Per le richieste di artefatto SAML e OAuth, il nodo verrà creato l'artefatto l'elemento DB presente nel file di configurazione. Se il file di configurazione non contiene una connessione al database dell'elemento, userà l'artefatto DB comune. Quando la richiesta successiva per recuperare l'elemento viene inserito in un altro nodo, l'altro nodo renderà l'API rest per il 1 ° nodo per recuperare l'elemento dall'elemento DB. Ciò è necessario in nodi diversi potrebbero avere artefatto diversi database e i nodi non avvertono che. Se il 1 ° nodo è inattivo, la risoluzione artefatto avrà esito negativo. A causa di questa struttura, la replica l'artefatto di DB in diversi Data Center non è necessaria. Se un intero Data Center è inattivo, molto probabilmente il nodo che ha creato l'elemento viene anche verso il basso, ciò significa che tale elemento non è più possibile risolvere.  

 - **Blocco Extranet** 

    Il database dell'elemento fa riferimento nel file di configurazione verrà utilizzato per i dati di blocco Extranet. Tuttavia, per la funzionalità di ESL ADFS sceglie un master che scrive i dati nel database l'artefatto. Tutti i nodi di eseguire un'API REST chiamata nel nodo master per ottenere e impostare le informazioni più recenti relative a ciascun utente. Se più database dell'elemento è in uso, l'amministratore deve selezionare un nodo master per ogni artefatto database o Data Center. 

    Per selezionare un nodo per essere il master ESL, passare al file di configurazione del nodo ad FS e nella sezione "Microsoft.IdentityServer.Service", aggiungere quanto segue:       
    
    Sul master aggiungere il seguente voce. Si noti che tutte e tre le chiavi sono tra maiuscole e minuscole. 

    &lt;useractivityfarmrole masterFQDN isMaster [nome di dominio completo del database primario selezionato] = = "true" /&gt;
    
    In altri nodi aggiungere la seguente voce:

   &lt;useractivityfarmrole masterFQDN isMaster [nome di dominio completo del database primario selezionato] = = "false" /&gt;
 
    >[!NOTE] 
    >Poiché più database di artefatto non sincronizzano dati, i valori ESL non verranno sincronizzati tra i database dell'elemento.
    Un utente può potenzialmente rilevare un Data Center diverso per una richiesta, rendendo pertanto il ExtranetLockoutThreshold dipende dal numero di database dell'elemento, ExtranetLockoutThreshold * numero di database dell'elemento. 
 
  - **Rilevamento riproduzione token** 
    
    I dati di rilevamento riproduzione token viene sempre chiamati dal database centrale dell'elemento. AD FS Salva il token di attendibilità Provider di attestazioni, garantendo che lo stesso token non possono essere riprodotti. Se un utente malintenzionato tenta di riprodurre lo stesso token, ADFS verifica se il token è presente nel database dell'elemento. Se il token è presente, la richiesta verrà rifiutata. Il database centrale dell'elemento viene usato per la sicurezza, poiché i dati dell'elemento DB non vengono replicati, un utente malintenzionato potrebbe inviare la richiesta a un altro Data Center e un token di riproduzione. Creazione di copie di sola lettura aggiuntive del ArtifactDB non impedirà la latenza tra Data Center in questo scenario, poiché viene usato solo il database centrale dell'elemento.    
 
 