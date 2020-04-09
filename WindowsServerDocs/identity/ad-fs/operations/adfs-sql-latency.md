---
title: Ottimizzazione di SQL e risoluzione dei problemi di latenza con AD FS
description: In questo documento viene illustrato come ottimizzare SQL con AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0881bff3455b471b0e51e960e1b0e522508a8b3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854864"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Ottimizzazione di SQL e risoluzione dei problemi di latenza con AD FS
In un aggiornamento per [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) sono stati introdotti i miglioramenti seguenti per ridurre la latenza tra database. Un aggiornamento imminente per AD FS 2019 includerà questi miglioramenti.

## <a name="in-memory-cache-update-in-background-thread"></a>Aggiornamento della cache in memoria in un thread in background 
Nelle distribuzioni precedenti di disponibilità always on (AoA), la latenza esisteva per qualsiasi operazione di "lettura" poiché il nodo master potrebbe trovarsi in un data center separato. La chiamata tra due diversi Data Center ha comportato una latenza.  

Nell'aggiornamento più recente per AD FS, una riduzione della latenza viene indirizzata tramite l'aggiunta di un thread in background per aggiornare la cache di configurazione AD FS e un'impostazione per impostare il periodo di tempo di aggiornamento. Il tempo impiegato per la ricerca di un database viene ridotto significativamente nel thread della richiesta, perché gli aggiornamenti della cache del database vengono spostati nel thread in background.  

Quando la `backgroundCacheRefreshEnabled` è impostata su true, AD FS consentirà al thread in background di eseguire gli aggiornamenti della cache. La frequenza di recupero dei dati dalla cache può essere personalizzata in base a un valore di ora impostando `cacheRefreshIntervalSecs`. Il valore predefinito è impostato su 300 secondi se `backgroundCacheRefreshEnabled` è impostato su true. Dopo la durata del valore impostato, AD FS inizia ad aggiornare la cache e mentre è in corso l'aggiornamento, i dati della cache obsoleti continueranno a essere usati.  

Quando AD FS riceve una richiesta per un'applicazione, AD FS recupera l'applicazione da SQL e la aggiunge alla cache. Al valore `cacheRefreshIntervalSecs`, l'applicazione nella cache viene aggiornata usando il thread in background. Mentre è presente una voce nella cache, le richieste in ingresso utilizzeranno la cache mentre è in corso l'aggiornamento in background. Se non si accede a una voce per 5 * `cacheRefreshIntervalSecs`, questa viene eliminata dalla cache. La voce meno recente può anche essere eliminata dalla cache quando viene raggiunto il valore di `maxRelyingPartyEntries` configurabile.

>[!NOTE]
> I dati della cache verranno aggiornati al di fuori del valore `cacheRefreshIntervalSecs` se ADFS riceve una notifica da SQL indicante che si è verificata una modifica nel database. Questa notifica attiverà la cache da aggiornare. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Suggerimenti per l'impostazione dell'aggiornamento della cache 
Il valore predefinito per l'aggiornamento della cache è **cinque minuti**. È consigliabile impostarla su **1 ora** per ridurre l'aggiornamento dei dati non necessario per ad FS perché i dati della cache verranno aggiornati in caso di modifiche SQL.  

AD FS registra un callback per le modifiche SQL e, in seguito a una modifica, ADFS riceve una notifica. Tramite questo metodo, ADFS riceve ogni nuova modifica da SQL non appena si verifica. 

In caso di problemi di rete che determinano AD FS manca la notifica SQL, AD FS aggiornerà in base all'intervallo specificato dal valore di aggiornamento della cache. Se si sospettano problemi di connettività tra AD FS e SQL, è consigliabile impostare il valore di aggiornamento della cache su un valore inferiore a 1 ora.  

### <a name="configuration-instructions"></a>Istruzioni di configurazione 
Il file di configurazione supporta più voci della cache. Gli elementi elencati di seguito possono essere configurati in base alle esigenze dell'organizzazione. 

L'esempio seguente abilita l'aggiornamento della cache in background e imposta il periodo di aggiornamento della cache su 1800 secondi o 30 minuti. Questa operazione deve essere eseguita in ogni nodo ADFS e il servizio ADFS deve essere riavviato in seguito. Le modifiche non influiscano sugli altri nodi e testano il primo nodo prima di apportare la modifica in tutti i nodi. 

  1. Passare al file di configurazione AD FS e, nella sezione "Microsoft. IdentityServer. Service", aggiungere la voce seguente:  
  
  - `backgroundCacheRefreshEnabled`: specifica se la funzionalità cache in background è abilitata. valori "true/false".
  - `cacheRefreshIntervalSecs`: valore in secondi in cui ADFS aggiornerà la cache. AD FS aggiornerà la cache in caso di modifiche in SQL. AD FS riceverà una notifica SQL e aggiornerà la cache.  
 
 >[!NOTE]
 > Tutte le voci nel file di configurazione fanno distinzione tra maiuscole e minuscole.  
 &lt;cache cacheRefreshIntervalSecs = "1800" backgroundCacheRefreshEnabled = "true"/&gt; 
 
Valori configurabili aggiuntivi supportati: 

   - **maxRelyingPartyEntries** : numero massimo di voci di relying party che ad FS manterranno in memoria. Questo valore viene usato anche dalla cache delle autorizzazioni dell'applicazione oAuth. Se sono presenti più autorizzazioni dell'applicazione rispetto a RPs e se tutte verranno archiviate in memoria, questo valore deve corrispondere al numero di autorizzazioni dell'applicazione. Il valore predefinito è 1000.
   - **maxIdentityProviderEntries** : numero massimo di voci del provider di attestazioni ad FS manterrà in memoria. Il valore predefinito è 200. 
   - **maxClientEntries** : numero massimo di voci client OAuth ad FS manterrà in memoria. Il valore predefinito è 500. 
   - **maxClaimDescriptorEntries** : numero massimo di voci del descrittore di attestazione ad FS manterrà in memoria. Il valore predefinito è 500. 
   - **maxNullEntries** : viene usato come cache negativa. Quando AD FS cerca una voce nel database e non viene trovata, AD FS aggiunge nella cache negativa. Si tratta della dimensione massima della cache. È presente una cache negativa per ogni tipo di oggetto, non è una singola cache per tutti gli oggetti. Il valore predefinito è 50, 0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Supporto per più database di artefatti tra i Data Center 
Per le configurazioni precedenti di più data center, AD FS ha supportato un solo database di elementi, causando latenza tra i Data Center durante le chiamate di recupero.  

Per ridurre la latenza tra Data Center, un amministratore AD FS ora può distribuire più istanze di database di artefatto e quindi modificare il file di configurazione di un nodo di AD FS in modo che punti a istanze di database di artefatto diverse. La stringa di connessione del database di elementi può essere specificata nel file di configurazione che consente un database di artefatto per nodo. Se la stringa di connessione non è presente nel file di configurazione, il nodo eseguirà il fallback alla progettazione precedente per usare il database degli artefatti presente nel database di configurazione.  
Con questa configurazione sono supportati anche gli ambienti ibridi.  

### <a name="requirements"></a>Requisiti: 
Prima di configurare più supporto per database di elementi, eseguire un aggiornamento su tutti i nodi e aggiornare i file binari poiché le chiamate a più nodi si verificano tramite questa funzionalità. 
  1. Generare lo script di distribuzione per creare il database di artefatto: per distribuire più istanze di database di artefatto, un amministratore dovrà generare lo script di distribuzione SQL per il database dell'artefatto. Nell'ambito di questo aggiornamento, il cmdlet `Export-AdfsDeploymentSQLScript`esistente è stato aggiornato per includere facoltativamente un parametro che specifica il database AD FS per cui generare uno script di distribuzione SQL. 
 
 Ad esempio, per generare lo script di distribuzione solo per il database di artefatto, specificare il parametro `-DatabaseType` e passare il valore "artefatto". Il parametro facoltativo `-DatabaseType` specifica il tipo di database AD FS e può essere impostato su: All (impostazione predefinita), artefatto o Configuration. Se non viene specificato alcun parametro di `-DatabaseType`, lo script configurerà sia l'artefatto che gli script di configurazione.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
Lo script generato deve essere eseguito nel computer SQL per creare i database necessari e assegnare all'account del servizio AD FS le autorizzazioni di SQL SA per tali database.

 2. Creare il database degli artefatti usando lo script di distribuzione. Copiare gli script di distribuzione CreateDB. SQL e setlocale. SQL appena generati nel computer SQL Server ed eseguirli per creare il database degli artefatti locali. 
 
 3. Modificare il file di configurazione per aggiungere la connessione al database dell'artefatto. 
 Passare al file di configurazione del nodo AD FS e, nella sezione "Microsoft. IdentityServer. Service", aggiungere un punto di ingresso al ArtifactDB appena configurato. 

 >[!NOTE] 
 > artifactStore e connectionString sono valori con distinzione tra maiuscole e minuscole. Verificare che siano configurati correttamente. &lt;artifactStore connectionString = "data source = .\SQLInstance; Integrated Security = true; Initial Catalog = AdfsArtifactStore"/&gt; 
>
>Utilizzare un valore dell'origine dati corrispondente alla connessione SQL.



 4. Riavviare il servizio AD FS per rendere effettive le modifiche. 
 
 >[!NOTE] 
 > Non è consigliabile usare la replica SQL o la sincronizzazione tra i database degli artefatti. Si consiglia di configurare un database di artefatto per ogni data center. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Failover tra data center e ripristino del database  
Si consiglia di creare database di artefatto di failover nello stesso data center del database dell'artefatto master. Se si verifica un failover, non si verificherà alcun aumento della latenza. Non è consigliabile usare i database di artefatto di failover nei data center. Di seguito vengono illustrati i dettagli relativi alle chiamate per la funzione di rilevamento OAuth, SAML, ESL e token replay con più database di artefatto. 
 - **OAuth e SAML** 

   Per le richieste di elementi OAuth e SAML, il nodo creerà l'artefatto nel database di elementi presente nel file di configurazione. Se il file di configurazione non contiene una connessione del database di artefatti, utilizzerà il DB dell'artefatto comune. Quando la richiesta successiva di recupero dell'artefatto passa a un altro nodo, l'altro nodo renderà l'API REST al primo nodo per recuperare l'artefatto dal database dell'artefatto. Questa operazione è necessaria perché i nodi diversi possono avere database di artefatto diversi e i nodi non lo conoscono. Se il primo nodo è inattivo, la risoluzione dell'artefatto avrà esito negativo. A causa di questa progettazione, non è necessario replicare il database di artefatti in diversi Data Center. Se un intero Data Center è inattivo, probabilmente anche il nodo che ha creato l'artefatto è inattivo, ovvero non è più possibile risolvere l'artefatto.  

 - **Blocco Extranet** 

    Il database di elementi a cui si fa riferimento nel file di configurazione verrà usato per i dati di blocco Extranet. Tuttavia, per la funzionalità ESL, AD FS sceglie un master che scrive i dati nel database dell'artefatto. Tutti i nodi effettuano una chiamata API REST al nodo master per ottenere e impostare le informazioni più recenti su ogni utente. Se sono in uso più database di artefatto, l'amministratore deve selezionare un nodo master per ogni elemento DB o Datacenter. 

    Per selezionare un nodo come master ESL, passare al file di configurazione del nodo ADFS e, nella sezione "Microsoft. IdentityServer. Service", aggiungere quanto segue:       
    
    Nel master aggiungere la voce seguente. Si noti che le tre chiavi fanno distinzione tra maiuscole e minuscole. 

    &lt;useractivityfarmrole masterFQDN = [FQDN del database primario selezionato] Master = "true"/&gt;
    
    Negli altri nodi aggiungere la voce seguente:

   &lt;useractivityfarmrole masterFQDN = [FQDN del database primario selezionato] Master = "false"/&gt;
 
    >[!NOTE] 
    >Poiché i database di più artefatti non sincronizzano i dati, i valori di ESL non verranno sincronizzati tra i database di artefatto.
    Un utente può potenzialmente raggiungere un data center diverso per una richiesta, rendendo quindi il ExtranetLockoutThreshold dipendente dal numero di database di artefatto, ExtranetLockoutThreshold * numero di database di artefatto. 
 
  - **Rilevamento riproduzione token** 
    
    I dati di rilevamento della riproduzione dei token vengono sempre chiamati dal database dell'artefatto centrale. AD FS Salva il token dall'attendibilità del provider di attestazioni, assicurando che non sia possibile riprodurre lo stesso token. Se un utente malintenzionato tenta di riprodurre lo stesso token, AD FS verifica se il token esiste nel database dell'artefatto. Se il token è presente, la richiesta verrà rifiutata. Il database dell'artefatto centrale viene usato per la sicurezza, poiché i dati del database di artefatto non vengono replicati, un utente malintenzionato potrebbe inviare la richiesta a un altro Data Center e riprodurre un token. La creazione di altre copie di sola lettura di ArtifactDB non impedirà la latenza tra data center in questo scenario, poiché viene usato solo il database degli artefatti centrali.    
 
 
