---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Aggiornamenti dei componenti di Servizi directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cde839feda47d55415b2b6cc1026a7a3e6515a44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823094"
---
# <a name="directory-services-component-updates"></a>Aggiornamenti dei componenti di Servizi directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  
  
In questa lezione vengono illustrati gli aggiornamenti dei componenti dei servizi directory in Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Contenuto dell'esercitazione  
Descrizione dei seguenti nuovi aggiornamenti dei componenti di servizi directory:  
  
-   Descrizione dei seguenti nuovi aggiornamenti dei componenti di servizi directory:  
  
    -   [Livelli di funzionalità di domini e foreste](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Elementi deprecati di NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Modifiche a Query Optimizer LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Miglioramenti dell'evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Miglioramento della velocità effettiva della replica di Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="domain-and-forest-functional-levels"></a><a name="BKMK_FL"></a>Livelli di funzionalità di domini e foreste  
  
### <a name="overview"></a>Panoramica  
La sezione fornisce una breve introduzione alle modifiche del livello di funzionalità del dominio e della foresta.  
  
### <a name="new-dfl-and-ffl"></a>Nuovi DFL e FFL  
Con la versione sono disponibili nuovi livelli di funzionalità di domini e foreste:  
  
-   Livello di funzionalità della foresta: Windows Server 2012 R2  
  
-   Livello di funzionalità del dominio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Il livello di funzionalità del dominio Windows Server 2012 R2 consente il supporto per gli elementi seguenti:  
  
1.  Protezioni lato controller di dominio per *utenti protetti*  
  
    *Gli utenti protetti* che eseguono l'autenticazione a un dominio di Windows Server 2012 R2 **non possono più**:  
  
    -   Usare l'autenticazione NTLM  
  
    -   Usare suite di crittografia DES o RC4 nella preautenticazione Kerberos  
  
    -   Usare la delega vincolata o non vincolata  
  
    -   Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore.  
  
2.  Criteri di autenticazione  
  
    Nuovi criteri di Active Directory basati sulle foreste che possono essere applicati agli account nei domini di Windows Server 2012 R2 per controllare quali host possono accedere da un account e applicare le condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account  
  
3.  Authentication Policy Silos  
  
    Nuovo oggetto Active Directory basato su foresta che può creare una relazione tra l'utente, il servizio gestito e gli account computer da usare per classificare gli account per i criteri di autenticazione o per l'isolamento dell'autenticazione.  
  
Per ulteriori informazioni, vedere come configurare gli account protetti.  
  
Oltre alle funzionalità sopra riportate, il livello di funzionalità del dominio Windows Server 2012 R2 garantisce che qualsiasi controller di dominio nel dominio esegua Windows Server 2012 R2.  
Il livello di funzionalità della foresta di Windows Server 2012 R2 non fornisce nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta funzioni automaticamente al livello di funzionalità del dominio Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>DFL minimo applicato alla creazione di un nuovo dominio  
Windows Server 2008 DFL è il livello di funzionalità minimo supportato per la creazione di un nuovo dominio.  
  
> [!NOTE]  
> La deprecazione di FRS viene eseguita rimuovendo la possibilità di installare un nuovo dominio con un livello di funzionalità del dominio inferiore a Windows Server 2008 con Server Manager o tramite Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Riduzione dei livelli di funzionalità del dominio e della foresta  
Per impostazione predefinita, i livelli di funzionalità della foresta e del dominio sono impostati su Windows Server 2012 R2 nel nuovo dominio e sulla nuova creazione della foresta, ma possono essere ridotti usando Windows PowerShell.  
  
Per aumentare o abbassare il livello di funzionalità della foresta utilizzando Windows PowerShell, utilizzare il cmdlet **Set-ADForestMode** .  
  
**Per impostare la modalità contoso.com FFL su Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Per aumentare o abbassare il livello di funzionalità del dominio usando Windows PowerShell, usare il cmdlet Set-ADDomainMode.  
  
**Per impostare la contoso.com DFL sulla modalità Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
La promozione di un controller di dominio che esegue Windows Server 2012 R2 come replica aggiuntiva in un dominio esistente che esegue 2003 DFL funziona.  
  
Creazione di un nuovo dominio in una foresta esistente  
  
![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
In questa versione non sono presenti nuove operazioni di foresta o di dominio.  
  
Questi file con estensione ldf contengono modifiche dello schema per il **servizio Registrazione dispositivi**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Cartelle di lavoro:**  
  
1.  Sch66  
  
**MSODS**  
  
1.  Sch60  
  
**Criteri di autenticazione e silo**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="deprecation-of-ntfrs"></a><a name="BKMK_NTFRS"></a>Deprecazione di NTFRS  
  
### <a name="overview"></a>Panoramica  
Il servizio Replica file è deprecato in Windows Server 2012 R2.  La deprecazione di FRS viene eseguita applicando un livello di funzionalità del dominio minimo (DFL) di Windows Server 2008.  Questa applicazione è presente solo se il nuovo dominio viene creato con Server Manager o Windows PowerShell.  
  
Usare il parametro-DomainMode con i cmdlet Install-ADDSForest o Install-ADDSDomain per specificare il livello di funzionalità del dominio.  I valori supportati per questo parametro possono essere un numero intero valido o un valore stringa enumerato corrispondente. Ad esempio, per impostare il livello di modalità dominio su Windows Server 2008 R2, è possibile specificare un valore pari a 4 o "Win2008R2".  Quando si eseguono questi cmdlet da Server 2012 R2, i valori validi includono quelli per Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) Windows Server 2012 (5, Win2012) e Windows Server 2012 R2 (6, Win2012R2). Il livello di funzionalità del dominio non può essere inferiore al livello di funzionalità della foresta, ma può essere superiore.  Poiché FRS è stato deprecato in questa versione, Windows Server 2003 (2, Win2003) non è un parametro riconosciuto con questi cmdlet quando viene eseguito da Windows Server 2012 R2.  
  
![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="ldap-query-optimizer-changes"></a><a name="BKMK_LDAPQuery"></a>Modifiche a Query Optimizer LDAP  
  
### <a name="overview"></a>Panoramica  
L'algoritmo di Query Optimizer LDAP è stato rivalutato e ottimizzato ulteriormente.  Il risultato è il miglioramento delle prestazioni nell'efficienza della ricerca LDAP e nei tempi di ricerca LDAP delle query complesse.  
  
> [!NOTE]
> <strong>Dallo sviluppatore:</strong>miglioramenti apportati alle prestazioni delle ricerche nei miglioramenti apportati al mapping dalla query LDAP alla query ESE.  I filtri LDAP oltre un determinato livello di complessità impediscono la selezione ottimizzata degli indici, con conseguente riduzione delle prestazioni (1.000 volte o più). Questa modifica modifica il modo in cui vengono selezionati gli indici per le query LDAP per evitare questo problema.  
> 
> [!NOTE]
> Una revisione completa dell'algoritmo di Query Optimizer LDAP, con conseguente:  
> 
> -   Tempi di ricerca più veloci  
> -   I miglioramenti dell'efficienza consentono ai controller di dominio di eseguire altre operazioni  
> -   Meno richieste di supporto per i problemi di prestazioni di Active Directory  
> -   Back ported to Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Background  
La possibilità di eseguire ricerche Active Directory è un servizio di base fornito dai controller di dominio.  Altri servizi e applicazioni line-of-business si basano sulle ricerche Active Directory.  Se questa funzionalità non è disponibile, è possibile che le operazioni di business smettano di fermarsi.  Come servizio di base e di uso intensivo, è indispensabile che i controller di dominio gestiscano il traffico di ricerca LDAP in modo efficiente.  L'algoritmo LDAP Query Optimizer tenta di rendere le ricerche LDAP più efficienti possibile eseguendo il mapping di filtri di ricerca LDAP a un set di risultati che può essere soddisfatto tramite record già indicizzati nel database.  Questo algoritmo è stato rivalutato e ottimizzato ulteriormente.  Il risultato è il miglioramento delle prestazioni nell'efficienza della ricerca LDAP e nei tempi di ricerca LDAP delle query complesse.  
  
### <a name="details-of-change"></a>Dettagli della modifica  
Una ricerca LDAP contiene:  
  
-   Un percorso (Head NC, OU, Object) all'interno della gerarchia per iniziare la ricerca  
  
-   Un filtro di ricerca  
  
-   Elenco di attributi da restituire.  
  
Il processo di ricerca può essere riepilogato come segue:  
  
1.  Se possibile, semplificare il filtro di ricerca.  
  
2.  Consente di selezionare un set di chiavi di indice che restituirà il set coperto più piccolo.  
  
3.  Eseguire una o più intersezioni delle chiavi di indice per ridurre il set coperto.  
  
4.  Per ogni record nel set coperto, valutare l'espressione di filtro e la sicurezza. Se il filtro restituisce TRUE e viene concesso l'accesso, il record viene restituito al client.  
  
Il lavoro di ottimizzazione delle query LDAP modifica i passaggi 2 e 3 per ridurre le dimensioni del set coperto. In particolare, l'implementazione corrente Seleziona chiavi di indice duplicate ed esegue le intersezioni ridondanti.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Confronto tra algoritmo obsoleto e nuovo  
La destinazione della ricerca LDAP non efficiente in questo esempio è un controller di dominio Windows Server 2012.  La ricerca viene completata in circa 44 secondi, a causa della mancata individuazione di un indice più efficiente.  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=justintu@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfind.txt  
  
Using server: WINSRV-DC1.blue.contoso.com:389  
  
<removed search results>  
  
Statistics  
=====  
Elapsed Time: 44640 (ms)  
Returned 324 entries of 553896 visited - (0.06%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 DNT_index:516615:N  
  
Pages Referenced          : 4619650  
Pages Read From Disk      : 973  
Pages Pre-read From Disk  : 180898  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
### <a name="sample-results-using-the-new-algorithm"></a>Risultati di esempio con il nuovo algoritmo  
In questo esempio viene ripetuta esattamente la stessa ricerca indicata sopra, ma è destinato a un controller di dominio Windows Server 2012 R2.  La stessa ricerca viene completata in meno di un secondo a causa dei miglioramenti apportati all'algoritmo LDAP Query Optimizer.  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=dhunt@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfindBLUE.txt  
  
Using server: winblueDC1.blue.contoso.com:389  
  
.<removed search results>  
  
Statistics  
=====  
Elapsed Time: 672 (ms)  
Returned 324 entries of 648 visited - (50.00%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 idx_userPrincipalName:648:N  
 idx_postalCode:323:N  
 idx_cn:1:N  
  
Pages Referenced          : 15350  
Pages Read From Disk      : 176  
Pages Pre-read From Disk  : 2  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
-   Se non è possibile ottimizzare l'albero:  
  
    -   Ad esempio, un'espressione nell'albero era su una colonna non indicizzata  
  
    -   Registrare un elenco di indici che impediscono l'ottimizzazione  
  
    -   Esposto tramite traccia ETW e ID evento 1644  
  
        ![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="to-enable-the-stats-control-in-ldp"></a><a name="BKMK_EnableStats"></a>Per abilitare il controllo delle statistiche in LDP  
  
1.  Aprire LDP. exe e connettersi e associarlo a un controller di dominio.  
  
2.  Scegliere **controlli**dal menu **Opzioni** .  
  
3.  Nella finestra di dialogo controlli espandere il menu a discesa **Load predefined** , fare clic su **Search Statistics** e quindi fare clic su **OK**.  
  
    ![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Scegliere **Cerca** dal menu **Sfoglia** .  
  
5.  Nella finestra di dialogo Cerca selezionare il pulsante **Opzioni** .  
  
6.  Verificare che la casella di controllo **esteso** sia selezionata nella finestra di dialogo Opzioni di ricerca e selezionare **OK**.  
  
    ![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Prova: USA LDP per restituire le statistiche sulle query  
Eseguire le operazioni seguenti in un controller di dominio o in un client o server aggiunto a un dominio in cui sono installati gli strumenti di servizi di dominio Active Directory.  Ripetere la procedura seguente per il controller di dominio Windows Server 2012 e il controller di dominio Windows Server 2012 R2.  
  
1.  Esaminare l'articolo ["creazione di applicazioni abilitate per Microsoft ad più efficienti"](https://msdn.microsoft.com/library/ms808539.aspx) e farvi riferimento in base alle esigenze.  
  
2.  Utilizzando LDP, abilitare le statistiche di ricerca (vedere [per abilitare il controllo statistiche in LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Eseguire varie ricerche LDAP e osservare le informazioni statistiche all'inizio dei risultati.  Si ripeterà la stessa ricerca in altre attività, quindi la si documenterà in un file di testo del blocco note.  
  
4.  Eseguire una ricerca LDAP che l'Query Optimizer dovrebbe essere in grado di ottimizzare a causa degli indici degli attributi  
  
5.  Tentativo di creare una ricerca che richiede molto tempo per il completamento (è possibile aumentare l'opzione **limite di tempo** in modo che la ricerca non venga impostata su timeout).  
  
### <a name="additional-resources"></a>Risorse aggiuntive  
[Che cosa sono le ricerche Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Come funzionano le ricerche Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Creazione di applicazioni più efficienti abilitate per Active Directory Microsoft](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) le query LDAP vengono eseguite più lentamente del previsto nel servizio directory ad o LDS/Adam ed è possibile che l'ID evento 1644 venga registrato  
  
## <a name="1644-event-improvements"></a><a name="BKMK_1644"></a>miglioramenti agli eventi 1644  
  
### <a name="overview"></a>Panoramica  
Questo aggiornamento consente di aggiungere statistiche aggiuntive per i risultati della ricerca LDAP all'ID evento 1644 per facilitare la risoluzione dei problemi.  Inoltre, è disponibile un nuovo valore del registro di sistema che può essere utilizzato per abilitare la registrazione in una soglia basata sul tempo.  Questi miglioramenti sono stati resi disponibili in Windows Server 2012 e Windows Server 2008 R2 SP1 tramite KB [2800945](https://support.microsoft.com/kb/2800945) e saranno resi disponibili per windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Sono state aggiunte statistiche di ricerca LDAP aggiuntive all'ID evento 1644 per facilitare la risoluzione di ricerche LDAP inefficienti o dispendiose  
> -   È ora possibile specificare una soglia di tempo di ricerca (ad esempio, Evento del log 1644 per le ricerche che richiedono più di 100 ms, anziché specificare i valori di soglia dei risultati di ricerca costosi e non efficienti  
  
### <a name="background"></a>Background  
Durante la risoluzione dei problemi relativi alle prestazioni Active Directory, risulta evidente che l'attività di ricerca LDAP potrebbe contribuire al problema.  Si decide di abilitare la registrazione per poter visualizzare query LDAP costose o inefficienti elaborate dal controller di dominio.  Per abilitare la registrazione, è necessario impostare il valore di diagnostica Field Engineering e, facoltativamente, specificare i valori soglia per i risultati di ricerca costosi e inefficienti.  Quando si Abilita il livello di registrazione progettazione campi su un valore pari a 5, qualsiasi ricerca che soddisfi questi criteri viene registrata nel registro eventi dei servizi directory con ID evento 1644.  
  
L'evento contiene:  
  
-   IP e porta del client  
  
-   Nodo iniziale  
  
-   Filtro  
  
-   Ambito di ricerca  
  
-   Selezione attributi  
  
-   Controlli server  
  
-   Voci visitate  
  
-   Voci restituite  
  
Tuttavia, i dati della chiave non sono presenti nell'evento, ad esempio la quantità di tempo impiegato per l'operazione di ricerca e gli eventuali indici utilizzati.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Ulteriori statistiche di ricerca aggiunte all'evento 1644  
  
-   Indici utilizzati  
  
-   Pagine a cui viene fatto riferimento  
  
-   Pagine lette dal disco  
  
-   Pagine prelette dal disco  
  
-   Pagine pulite modificate  
  
-   Pagine dirty modificate  
  
-   Tempo di ricerca  
  
-   Attributi che impediscono l'ottimizzazione  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nuovo valore del registro di sistema della soglia basata sul tempo per la registrazione dell'evento 1644  
Invece di specificare i valori di soglia per i risultati di ricerca costosi e inefficienti, è possibile specificare la soglia del tempo di ricerca.  Se si desidera registrare tutti i risultati della ricerca che hanno richiesto 50 ms o versione successiva, è necessario specificare 50 esadecimale/32 esadecimale (oltre a impostare il valore di Field Engineering).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Confronto tra il vecchio e il nuovo ID evento 1644  
PRECEDENTE  
  
![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NUOVO  
  
![aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Prova: usare il registro eventi per restituire le statistiche sulle query  
  
1.  Ripetere la procedura seguente per il controller di dominio Windows Server 2012 e il controller di dominio Windows Server 2012 R2. Osservare l'ID evento 1644 in entrambi i controller di dominio dopo ogni ricerca.  
  
2.  Con regedit, abilitare la registrazione con ID evento 1644 usando una soglia basata sul tempo sul controller di dominio Windows Server 2012 R2 e il vecchio metodo sul controller di dominio di Windows Server 2012.  
  
3.  Eseguire varie ricerche LDAP che superano la soglia e osservano le informazioni statistiche all'inizio dei risultati.  Usare le query LDAP documentate in precedenza e ripetere le stesse ricerche.  
  
4.  Eseguire una ricerca LDAP che l'Query Optimizer non è in grado di ottimizzare perché uno o più attributi non vengono indicizzati.  
  
## <a name="active-directory-replication-throughput-improvement"></a><a name="BKMK_ADRepl"></a>Miglioramento della velocità effettiva della replica Active Directory  
  
### <a name="overview"></a>Panoramica  
La replica di Active Directory utilizza RPC per il trasporto di replica. Per impostazione predefinita, RPC usa un buffer di trasmissione da 8 KB e una dimensione del pacchetto 5K. Questo ha l'effetto finale in cui l'istanza mittente trasmette tre pacchetti (approssimativamente 15.000 di dati) e quindi deve attendere un round trip di rete prima di inviarli. Supponendo un tempo di round trip 3ms, la velocità effettiva più elevata è aggirata tra 40Mbps, anche in reti 1 Gbps o 10 Gbps.  
  
> [!NOTE]  
> -   Questo aggiornamento regola la velocità effettiva massima della replica AD da 40Mbps a circa 600 Mbps.  
>   
>     -   Aumenta la dimensione del buffer di invio RPC che riduce il numero di round trip in rete  
> -   L'effetto sarà più evidente sulla rete ad alta velocità e a latenza elevata.  
  
Questi aggiornamenti aumentano la velocità effettiva massima a circa 600 Mbps modificando le dimensioni del buffer di invio RPC da 8 a 256 KB.  Questa modifica consente di aumentare le dimensioni della finestra TCP oltre gli 8 KB, riducendo il numero di round trip in rete.  
  
> [!NOTE]  
> Non sono disponibili impostazioni configurabili per modificare questo comportamento.  
  
### <a name="additional-resources"></a>Risorse aggiuntive  
[Funzionamento del modello di replica Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


