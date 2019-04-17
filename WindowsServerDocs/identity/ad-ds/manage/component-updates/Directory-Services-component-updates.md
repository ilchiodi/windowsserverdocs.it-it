---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Aggiornamenti dei componenti di servizi directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac42591450038240ced273555fb01e66b1ff5546
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="directory-services-component-updates"></a>Aggiornamenti dei componenti di servizi directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.  
  
Questa lezione illustra gli aggiornamenti dei componenti dei servizi Directory in Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Contenuto dell'esercitazione  
Illustrano gli aggiornamenti dei componenti nuovi servizi di Directory seguenti:  
  
-   Illustrano gli aggiornamenti dei componenti nuovi servizi di Directory seguenti:  
  
    -   [Livelli di funzionalità del dominio e foresta](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Elementi deprecati di NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Modifiche a Query Optimizer LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Miglioramenti dell'evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Miglioramento della velocità effettiva della replica di Directory Active](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Livelli di funzionalità del dominio e foresta  
  
### <a name="overview"></a>Panoramica  
La sezione fornisce una breve introduzione alle modifiche di livello di funzionalità foresta e del dominio.  
  
### <a name="new-dfl-and-ffl"></a>FFL e nuove funzionalità del dominio  
Con il rilascio, sono disponibili nuovi livelli di funzionalità di domini e foreste:  
  
-   Livello di funzionalità della foresta: Windows Server 2012 R2  
  
-   Livello di funzionalità dominio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 livello funzionale di dominio attiva il supporto per le operazioni seguenti:  
  
1.  Protezioni sul lato controller di dominio per *utenti protetti*  
  
    *Gli utenti protetti* l'autenticazione a un dominio Windows Server 2012 R2 può **non è più**:  
  
    -   Eseguire l'autenticazione con l'autenticazione NTLM  
  
    -   Utilizzare i pacchetti di crittografia DES o RC4 nella preautenticazione Kerberos  
  
    -   Delega vincolata o non usare la delega  
  
    -   Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore  
  
2.  Criteri di autenticazione  
  
    Basato sul nuovo insieme di strutture Active Directory i criteri che possono essere applicati ad account in domini di Windows Server 2012 R2 per controllare quali host un account può sign-on da e applicare condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account  
  
3.  Silo di criteri di autenticazione  
  
    Nuova foresta oggetto basato su Active Directory che è possibile creare una relazione tra l'utente, servizi gestiti e account computer per essere utilizzate per classificare gli account per i criteri di autenticazione o per l'isolamento di autenticazione.  
  
Vedere come configurare gli account protetti per ulteriori informazioni.  
  
Oltre alle funzionalità precedente, il livello di funzionalità del dominio Windows Server 2012 R2 assicura che qualsiasi controller di dominio nel dominio esegue Windows Server 2012 R2.  
Il livello di funzionalità foresta Windows Server 2012 R2 non fornisce nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta venga automaticamente utilizzato a livello funzionale di dominio di Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Livello di funzionalità minimo imposta su creazione del nuovo dominio  
Livello di funzionalità di Windows Server 2008 è il livello di funzionalità minimo supportato nella creazione del nuovo dominio.  
  
> [!NOTE]  
> Rimuovendo la possibilità di installare un nuovo dominio con un livello di funzionalità dominio inferiore a Windows Server 2008 con Server Manager o tramite Windows PowerShell viene eseguita la deprecazione di FRS.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Ridurre i livelli di funzionalità foresta e del dominio  
I livelli di funzionalità foresta e del dominio a Windows Server 2012 R2 per impostazione predefinita al momento della creazione nuovo dominio e della nuova foresta, ma possono essere ridotto utilizzando Windows PowerShell.  
  
Per aumentare o diminuire il livello di funzionalità della foresta con Windows PowerShell, usare il **Set-ADForestMode** cmdlet.  
  
**Per impostare il contoso.com FFL alla modalità di Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Per aumentare o diminuire il livello di funzionalità del dominio con Windows PowerShell, usare il cmdlet Set-ADDomainMode.  
  
**Per impostare il livello di funzionalità contoso.com alla modalità di Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Funzionamento della promozione di un controller di dominio che esegue Windows Server 2012 R2 come una replica aggiuntiva in un dominio esistente che esegue DFL 2003.  
  
Creazione del nuovo dominio in una foresta esistente  
  
![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
Non esistono operazioni di dominio in questa versione o nuova foresta.  
  
Questi file con estensione ldf contengono modifiche allo schema di **Device Registration Service**.  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**Cartelle di lavoro:**  
  
1.  Sch66  
  
**MSODS:**  
  
1.  Sch60  
  
**Silo e criteri di autenticazione**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Elementi deprecati di NTFRS  
  
### <a name="overview"></a>Panoramica  
Il servizio Replica file è deprecato in Windows Server 2012 R2.  La deprecazione del servizio Replica file viene eseguita applicando un livello funzionalità dominio minimo (livello) di Windows Server 2008.  Questa applicazione è presente solo se il nuovo dominio viene creato utilizzando Server Manager o Windows PowerShell.  
  
Usare il parametro - DomainMode con i cmdlet Install-ADDSForest o Install-ADDSDomain per specificare il livello di funzionalità del dominio.  I valori supportati per questo parametro possono essere un numero intero valido o un valore enumerato stringa corrispondente. Ad esempio, per impostare il livello di dominio in modalità a Windows Server 2008 R2, è possibile specificare un valore pari a 4 o "Win2008R2".  Durante l'esecuzione di questi cmdlet da Server 2012 R2 valido i valori includono quelli per Windows Server 2008 (3, Win2008), Windows Server 2008 R2 (4, Win2008R2) (5, Win2012) di Windows Server 2012 e Windows Server 2012 R2 (6, Win2012R2). Il livello di funzionalità del dominio non può essere inferiore rispetto al livello funzionale di foresta, ma può essere superiore.  Poiché il servizio Replica file è deprecato in questa versione, Windows Server 2003 (2, Win2003) non è un parametro riconosciuto con questi cmdlet quando viene eseguito da Windows Server 2012 R2.  
  
![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Modifiche a Query Optimizer LDAP  
  
### <a name="overview"></a>Panoramica  
L'algoritmo di query optimizer LDAP query è stato rivalutato e ulteriormente ottimizzato.  Il risultato è il miglioramento delle prestazioni nell'efficienza della ricerca LDAP e durata ricerca LDAP delle query complesse.  
  
> [!NOTE]  
> **Dallo sviluppatore:**miglioramenti sulle prestazioni di ricerche tramite miglioramenti nel mapping da LDAP eseguono una query per query ESE.  Selezione di indice ottimizzato, risultante drastica riduzione delle prestazioni (o x 1000) impedita dai filtri LDAP oltre un determinato livello di complessità. Questa modifica modifica il modo in cui abbiamo selezionare gli indici per le query LDAP evitare questo problema.  
  
> [!NOTE]  
> Una revisione completa dell'algoritmo di ottimizzazione di query LDAP, risultante:  
>   
> -   Tempi di ricerca  
> -   Efficienza consentono di fare di più controller di dominio  
> -   Meno chiamate al supporto tecnico problemi relative alle prestazioni di Active Directory  
> -   Eseguire il convertita in Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Sfondo  
La possibilità di eseguire ricerche di Active Directory è un servizio principale fornito dai controller di dominio.  Altri servizi e applicazioni aziendali si basano su ricerche di Active Directory.  Se questa funzionalità non è disponibile, le operazioni aziendali possono smettere di arresto.  Come un servizio molto usato e dei componenti di base, è fondamentale che i controller di dominio di gestire in modo efficiente il traffico di ricerca LDAP.  L'algoritmo di query optimizer LDAP query tenta di effettuare ricerche LDAP efficienti possibile eseguendo il mapping di filtri di ricerca LDAP a un set di risultati che può essere soddisfatte tramite record già indicizzati nel database.  Questo algoritmo è stato rivalutato e ulteriormente ottimizzato.  Il risultato è il miglioramento delle prestazioni nell'efficienza della ricerca LDAP e durata ricerca LDAP delle query complesse.  
  
### <a name="details-of-change"></a>Dettagli di modifica  
Contiene una ricerca LDAP:  
  
-   Un percorso (NC testa, unità Organizzativa, oggetto) all'interno della gerarchia per avviare la ricerca  
  
-   Un filtro di ricerca  
  
-   Un elenco di attributi da restituire  
  
Il processo di ricerca può essere riassunto come segue:  
  
1.  Se possibile, semplificare il filtro di ricerca.  
  
2.  Selezionare un set di chiavi di indice che verrà restituito il valore più piccolo set coperto.  
  
3.  Eseguire le intersezioni uno o più delle chiavi di indice, per ridurre l'insieme di coperto.  
  
4.  Per ogni record nel set di coperto, valutare l'espressione di filtro, nonché la sicurezza. Se il risultato del filtro su TRUE e viene concesso l'accesso, restituire il record al client.  
  
Il lavoro di ottimizzazione di query LDAP modifica i passaggi 2 e 3, per ridurre le dimensioni del set di coperto. In particolare, l'implementazione corrente Seleziona indice duplicato ed esegue le intersezioni ridondanti.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Confronto tra algoritmo nuovo e precedente  
La destinazione della ricerca LDAP inefficiente in questo esempio è un controller di dominio Windows Server 2012.  La ricerca è stata completata in circa 44 secondi in seguito a riesce a trovare un indice più efficiente.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Risultati di esempio con l'algoritmo di nuovo  
Questo esempio si ripete la stessa ricerca esattamente come illustrato in precedenza, ma è destinato a un controller di dominio Windows Server 2012 R2.  La stessa ricerca viene completato in meno di un secondo grazie ai miglioramenti dell'algoritmo di ottimizzazione di query LDAP.  
  
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
  
-   Se non è possibile ottimizzare la struttura ad albero:  
  
    -   Ad esempio: un'espressione della struttura ad albero non è una colonna non indicizzata  
  
    -   Registrare un elenco di indici che impediscono di ottimizzazione  
  
    -   Vengono esposte tramite traccia ETW e ID evento 1644  
  
        ![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Per abilitare il controllo di statistiche in LDP  
  
1.  Aprire LDP.exe e connessione e il binding a un controller di dominio.  
  
2.  Nel **opzioni** menu, fare clic su **controlli**.  
  
3.  Nella finestra di dialogo controlli, espandere il **carico predefiniti** dal menu a discesa, fare clic su **statistiche ricerca** e quindi fare clic su **OK**.  
  
    ![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Nel **Sfoglia** menu, fare clic su **ricerca**  
  
5.  Nella finestra di dialogo ricerca, seleziona il **opzioni** pulsante.  
  
6.  Assicurarsi di **Extended** casella di controllo è selezionata nella finestra di dialogo Opzioni di ricerca e seleziona **OK**.  
  
    ![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Procedura: Utilizzare LDP per restituire le statistiche sulle query  
Eseguire il comando seguente in un controller di dominio o da un client appartenenti a un dominio o server in cui sono installati gli strumenti di dominio Active Directory.  Ripetere il comando seguente il controller di dominio di Windows Server 2012 e Windows Server 2012 R2 controller di dominio di destinazione.  
  
1.  Esaminare il ["Creazione di più efficiente Microsoft AD applicazioni abilitate all'uso"](https://msdn.microsoft.com/library/ms808539.aspx) articolo e fare riferimento a essa in base alle esigenze.  
  
2.  Utilizzare LDP, attivare le statistiche di ricerca (vedere [per abilitare il controllo di statistiche in LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Eseguire ricerche LDAP diversi e osservare le informazioni statistiche all'inizio dei risultati.  Si verrà ripetere la ricerca stesso in altre attività in modo documentarli in un file di testo blocco note.  
  
4.  Eseguire una ricerca LDAP che query optimizer deve essere in grado di ottimizzare a causa di indici di attributi  
  
5.  Tenta di creare una ricerca che richiede molto tempo per completare (è possibile aumentare la **limite di tempo** opzione in modo che la ricerca viene timeout non).  
  
### <a name="additional-resources"></a>Risorse aggiuntive  
[Quali sono le ricerche di Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[La modalità di lavoro di ricerche di Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Creazione di più efficiente Microsoft Active Directory applicazioni abilitate all'uso](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) query LDAP vengono eseguite più lentamente del previsto in Active Directory o servizio directory LDS/ADAM e 1644 ID evento potrebbero essere registrati.  
  
## <a name="BKMK_1644"></a>Miglioramenti dell'evento 1644  
  
### <a name="overview"></a>Panoramica  
Questo aggiornamento aggiunge altre statistiche dei risultati di ricerca LDAP all'ID evento 1644 per facilitare la risoluzione dei problemi.  Inoltre, non esiste un nuovo valore del Registro di sistema che può essere usato per abilitare la registrazione su una soglia basati sul tempo.  Questi miglioramenti sono stati resi disponibili in Windows Server 2012 e Windows Server 2008 R2 SP1 tramite KB [2800945](https://support.microsoft.com/kb/2800945) e saranno resi disponibili per Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Statistiche di ricerca LDAP aggiuntive vengono aggiunte all'ID evento 1644 per agevolare la risoluzione dei problemi relativi a ricerche LDAP inefficiente o costose  
> -   È ora possibile specificare una soglia durata di ricerca (ad es. Registro eventi 1644 per la ricerca impiega più di 100 ms) invece di specificare i valori di soglia risultati di ricerca Expensive e Inefficient  
  
### <a name="background"></a>Sfondo  
Per la risoluzione dei problemi di prestazioni di Active Directory, risulta evidente che attività di ricerca LDAP può contribuire a circoscrivere il problema.  Si decide di abilitare la registrazione in modo che sia possibile visualizzare le query LDAP costose o poco efficiente elaborate dal controller di dominio.  Per abilitare la registrazione, è necessario impostare il valore di diagnostica Field Engineering e facoltativamente possibile specificare i valori di soglia risultati di ricerca costose / poco efficiente.  Dopo aver attivato il livello di registrazione su un valore pari a 5 Field Engineering, tutte le ricerche che soddisfano questi criteri viene registrata nel registro eventi di servizi di Directory con un ID evento 1644.  
  
L'evento contiene:  
  
-   Client IP e porta  
  
-   Nodo di avvio  
  
-   Filtro  
  
-   Ambito di ricerca  
  
-   Selezione di attributo  
  
-   Controlli server  
  
-   Voci visitate  
  
-   Voci restituite  
  
Tuttavia, dati della chiave sono presenti l'evento, ad esempio la quantità di tempo impiegato per l'operazione di ricerca e cosa (se presente) è stato utilizzato indice.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Statistiche di ricerca aggiuntivi aggiunte all'evento 1644  
  
-   Indici utilizzati  
  
-   Pagine di riferimento  
  
-   Le pagine lette dal disco  
  
-   Pagine pre-lette dal disco  
  
-   Pulisci pagine modificate  
  
-   Pagine dirty modificate  
  
-   Tempo di ricerca  
  
-   Attributi impedendo ottimizzazione  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nuovo valore del Registro di sistema soglia basati sul tempo per la registrazione evento 1644  
Anziché specificare i valori di soglia risultati di ricerca Expensive e Inefficient, è possibile specificare una soglia durata di ricerca.  Se è maggiore o desiderati per accedere a tutti i risultati di ricerca che ha avuto 50 ms, è necessario specificare 50 decimale o esadecimale 32 (oltre a impostare il valore campo Engineering).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Confronto tra l'obsoleti e nuovo ID evento 1644  
PRECEDENTE  
  
![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
Nuovo  
  
![Aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Procedura: Utilizzare il registro eventi per restituire le statistiche sulle query  
  
1.  Ripetere il comando seguente il controller di dominio di Windows Server 2012 e Windows Server 2012 R2 controller di dominio di destinazione. Osservare l'evento ID 1644s su entrambi i controller di dominio dopo ogni ricerca.  
  
2.  Utilizzando regedit, abilitare la registrazione di ID evento 1644 utilizzando una soglia basati sul tempo di Windows Server 2012 R2 controller di dominio e il metodo precedente nel controller di dominio di Windows Server 2012.  
  
3.  Eseguire ricerche LDAP diversi che superano la soglia e osservare le informazioni statistiche all'inizio dei risultati.  Utilizzare le query LDAP che è documentato in precedenza e ripetere le ricerche stesso.  
  
4.  Eseguire una ricerca LDAP che l'utilità di ottimizzazione di query non è in grado di ottimizzare perché uno o più attributi non vengono indicizzati.  
  
## <a name="BKMK_ADRepl"></a>Miglioramento della velocità effettiva della replica di Directory Active  
  
### <a name="overview"></a>Panoramica  
Replica di Active Directory utilizza RPC per il trasporto di replica. Per impostazione predefinita, RPC Usa un buffer di trasmissione di 8 KB e dimensioni di un pacchetto di 5 KB. Questo è il risultato finale in cui l'istanza invio verrà trasmettere pacchetti di tre (circa 15K vale la pena di dati) e quindi è necessario attendere una rete round trip prima di inviare più. Supponendo che un 3 MS tempo di andata e ritorno, la velocità effettiva massima sarebbe attorno a 40 Mbps, anche su 1 Gbps o 10 Gbps reti.  
  
> [!NOTE]  
> -   Questo aggiornamento regola la velocità effettiva massima replica di Active Directory da 40 Mbps a circa 600 Mbps.  
>   
>     -   Aumenta la dimensione del buffer di invio RPC riducendo il numero di rete di andata e ritorno  
> -   L'effetto sarà più evidente in ad alta velocità, rete ad alta latenza.  
  
Gli aggiornamenti di aumentare la velocità effettiva massima a circa 600 Mbps modificando le dimensioni del buffer di invio RPC da 8K di 256KB.  Questa modifica consente le dimensioni della finestra TCP superare 8 KB, riducendo il numero di rete di andata e ritorno.  
  
> [!NOTE]  
> Non sono presenti impostazioni configurabili per modificare questo comportamento.  
  
### <a name="additional-resources"></a>Risorse aggiuntive  
[Funzionamento del modello di replica di Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


