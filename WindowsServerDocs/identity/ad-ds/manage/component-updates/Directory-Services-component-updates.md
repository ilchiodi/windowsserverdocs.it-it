---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: Aggiornamenti dei componenti di Servizi directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f3e0553b1919a7f9129d47616d0ffb66b6ff48f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874442"
---
# <a name="directory-services-component-updates"></a>Aggiornamenti dei componenti di Servizi directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin. Turner, Senior Support Escalation Engineer presso il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  
  
In questa lezione vengono illustrati gli aggiornamenti dei componenti dei servizi di Directory in Windows Server 2012 R2.  
  
## <a name="what-you-will-learn"></a>Contenuto dell'esercitazione  
Illustrano gli aggiornamenti dei componenti nuovi servizi di Directory seguenti:  
  
-   Illustrano gli aggiornamenti dei componenti nuovi servizi di Directory seguenti:  
  
    -   [Livelli di funzionalità di domini e foreste](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [Elementi deprecati di NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [Modifiche a Query Optimizer LDAP](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [Miglioramenti dell'evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Miglioramento della velocità effettiva della replica di Active Directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>Livelli di funzionalità del dominio e foresta  
  
### <a name="overview"></a>Panoramica  
La sezione fornisce una breve introduzione alle modifiche di livello funzionale di domini e foreste.  
  
### <a name="new-dfl-and-ffl"></a>FFL e nuove funzionalità del dominio  
Con il rilascio, esistono nuovi livelli di funzionalità di domini e foreste:  
  
-   Livello di funzionalità della foresta: Windows Server 2012 R2  
  
-   Livello funzionale del dominio: Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 livello funzionale di dominio abilita il supporto per le operazioni seguenti:  
  
1.  Protezioni sul lato controller di dominio per *utenti protetti*  
  
    *Agli utenti protetti* l'autenticazione a un dominio di Windows Server 2012 R2 può **non è più**:  
  
    -   Eseguire l'autenticazione con l'autenticazione NTLM  
  
    -   Usare pacchetti di crittografia DES o RC4 nella preautenticazione Kerberos  
  
    -   Delega vincolata o non usare la delega  
  
    -   Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore  
  
2.  Authentication Policies  
  
    Nuova foresta Active Directory i criteri che possono essere applicati agli account nei domini Windows Server 2012 R2 per controllare quali host un account può sign-on da e applicare condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account  
  
3.  Authentication Policy Silos  
  
    Nuova foresta Active Directory oggetti che è possono creare una relazione tra utente, servizi gestiti e gli account computer per essere usato per classificare gli account per i criteri di autenticazione o per l'isolamento di autenticazione.  
  
Vedere come configurare gli account protetti per altre informazioni.  
  
Oltre alle funzionalità precedente, il livello di funzionalità dominio di Windows Server 2012 R2 garantisce che qualsiasi controller di dominio nel dominio esegua Windows Server 2012 R2.  
Il livello di funzionalità foresta Windows Server 2012 R2 non fornisce tutte le nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta venga automaticamente utilizzato a livello funzionale di dominio di Windows Server 2012 R2.  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>Funzionalità del dominio minimo applicati per la creazione del nuovo dominio  
Funzionalità del dominio di Windows Server 2008 è il livello di funzionalità minimo supportato nella creazione del nuovo dominio.  
  
> [!NOTE]  
> Deprecazione di FRS viene eseguita, eliminando la possibilità di installare un nuovo dominio con un livello di funzionalità dominio inferiore a Windows Server 2008 con Server Manager o tramite Windows PowerShell.  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>Abbassando i livelli di funzionalità foresta e del dominio  
I livelli di funzionalità foresta e del dominio vengono impostati per Windows Server 2012 R2 per impostazione predefinita al momento della creazione nuovo nuova foresta e dominio ma possono essere ridotto mediante Windows PowerShell.  
  
Per aumentare o diminuire il livello di funzionalità della foresta con Windows PowerShell, usare il **Set-ADForestMode** cmdlet.  
  
**Per impostare contoso.com FFL in modalità Windows Server 2008:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
Per aumentare o diminuire il livello funzionale del dominio tramite Windows PowerShell, usare il cmdlet Set-ADDomainMode.  
  
**Per impostare la funzionalità del dominio contoso.com in modalità Windows Server 2008:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Innalzamento di livello un controller di dominio che eseguono Windows Server 2012 R2 come un'altra replica in un dominio esistente che esegue funzionalità del dominio 2003 funziona.  
  
Creazione del nuovo dominio in una foresta esistente  
  
![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
Non esistono operazioni di dominio in questa versione o nuova foresta.  
  
Questi file con estensione ldf contengono modifiche dello schema per il **Device Registration Service**.  
  
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
  
**I silo e criteri di autenticazione**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Elementi deprecati di NTFRS  
  
### <a name="overview"></a>Panoramica  
Il servizio Replica file è deprecato in Windows Server 2012 R2.  Deprecazione di FRS viene eseguita tramite l'applicazione di un livello funzionale del dominio minimo (funzionalità del dominio) di Windows Server 2008.  Questa applicazione è presente solo se il nuovo dominio viene creato usando Server Manager o Windows PowerShell.  
  
Utilizzare il parametro - Modalitàdominio con Install-ADDSForest o i cmdlet Install-ADDSDomain per specificare il livello funzionale del dominio.  I valori supportati per questo parametro possono essere un numero intero valido o un valore stringa enumerata corrispondente. Ad esempio, per impostare il livello di modalità di dominio a Windows Server 2008 R2, è possibile specificare un valore pari a 4 o "Win2008R2".  Durante l'esecuzione di questi cmdlet da Server 2012 R2 validi i valori includono quelle per Windows Server 2008 (3, Win2008) Windows Server 2008 R2 (4, Win2008R2) (5, Win2012) di Windows Server 2012 e Windows Server 2012 R2 (6, Win2012R2). Il livello di funzionalità del dominio non può essere inferiore al livello di funzionalità della foresta, ma può essere superiore.  Poiché il servizio Replica file è deprecato in questa versione, Windows Server 2003 (2, Win2003) non è un parametro riconosciuto con questi cmdlet quando eseguita da Windows Server 2012 R2.  
  
![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>Modifiche di Query Optimizer LDAP  
  
### <a name="overview"></a>Panoramica  
L'algoritmo di ottimizzazione query LDAP è stata rivalutata e ulteriormente ottimizzato.  Il risultato è il miglioramento delle prestazioni nell'efficienza della ricerca LDAP e durata ricerca LDAP delle query complesse.  
  
> [!NOTE]  
> **Da parte dello sviluppatore:** miglioramenti delle prestazioni delle ricerche attraverso i miglioramenti nel mapping da LDAP di query a query ESE.  Indice con ottimizzazione per la selezione, risultante in modo significativo calo delle prestazioni (x 1000 o superiore) impedita dai filtri LDAP oltre un certo livello di complessità. Questa modifica consente di modificare il modo in cui si selezionano gli indici per le query LDAP evitare questo problema.  
  
> [!NOTE]  
> Una revisione completa dell'algoritmo, query optimizer LDAP query risultante:  
>   
> -   Tempi di ricerca  
> -   Consentire l'aumento della produttività controller di dominio eseguire altre operazioni  
> -   Meno chiamate al supporto tecnico problemi relative alle prestazioni di Active Directory  
> -   Eseguire il porting in Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>Informazioni  
La possibilità di eseguire ricerche in Active Directory è un servizio fondamentale fornito da controller di dominio.  Altri servizi e applicazioni line of business si basano sulle ricerche di Active Directory.  Operazioni di business possono smettere di arresto se questa funzionalità non è disponibile.  Come un core e un servizio utilizzato molto frequentemente, è fondamentale che i controller di dominio di gestire il traffico di ricerca LDAP in modo efficiente.  L'algoritmo di ottimizzazione query LDAP tenta di rendere più efficiente possibile le ricerche LDAP eseguendo il mapping di filtri di ricerca LDAP a un set di risultati che può essere soddisfatte tramite record già indicizzati nel database.  Questo algoritmo è stato rivalutato e ulteriormente ottimizzato.  Il risultato è il miglioramento delle prestazioni nell'efficienza della ricerca LDAP e durata ricerca LDAP delle query complesse.  
  
### <a name="details-of-change"></a>Dettagli di modifica  
Contiene una ricerca LDAP:  
  
-   Un percorso (intestazione NC dell'unità Organizzativa, oggetto) all'interno della gerarchia per iniziare la ricerca  
  
-   Un filtro di ricerca  
  
-   Un elenco di attributi da restituire  
  
Il processo di ricerca può essere riepilogato come segue:  
  
1.  Se possibile, semplificare il filtro di ricerca.  
  
2.  Selezionare un set di chiavi di indice che restituirà il set più piccolo maggiore coperto.  
  
3.  Eseguire le intersezioni di uno o più delle chiavi di indice, per ridurre il set di coperto.  
  
4.  Per ogni record nel set di copertura, valutare l'espressione di filtro, nonché la sicurezza. Se il filtro restituisce TRUE e viene concesso l'accesso, quindi restituiscono questo record al client.  
  
Le operazioni di ottimizzazione query LDAP consente di modificare i passaggi 2 e 3, per ridurre le dimensioni del set di copertura. In particolare, l'implementazione corrente consente di selezionare le chiavi di indice duplicati ed esegue le intersezioni ridondanti.  
  
### <a name="comparison-between-old-and-new-algorithm"></a>Confronto tra vecchio e nuovo di algoritmo  
La destinazione della ricerca LDAP inefficiente in questo esempio è un controller di dominio di Windows Server 2012.  La ricerca viene completato in circa 44 secondi in seguito a non riuscire a trovare un indice più efficiente.  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>Risultati di esempio usando il nuovo algoritmo  
In questo esempio si ripete la stessa ricerca esatta come illustrato in precedenza ma è destinata a un controller di dominio di Windows Server 2012 R2.  La ricerca stessa viene completato in meno di un secondo a causa di miglioramenti apportate all'algoritmo di ottimizzazione query LDAP.  
  
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
  
    -   Ad esempio: un'espressione dell'albero non su una colonna non indicizzata  
  
    -   Registrare un elenco di indici che impediscono l'ottimizzazione  
  
    -   Vengono esposte tramite l'analisi ETW e ID evento 1644 come  
  
        ![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>Per abilitare il controllo delle statistiche in LDP  
  
1.  Aprire LDP.exe e connettersi ed eseguire l'associazione a un controller di dominio.  
  
2.  Nel **le opzioni** menu, fare clic su **controlli**.  
  
3.  Nella finestra di dialogo controlli, espandere la **Load Predefined** dal menu a discesa, fare clic su **ricerca Stats** e quindi fare clic su **OK**.  
  
    ![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  Nel **esplorare** menu, fare clic su **ricerca**  
  
5.  Nella finestra di dialogo di ricerca, selezionare la **opzioni** pulsante.  
  
6.  Verificare che il **Extended** casella di controllo è selezionata nella finestra di dialogo Opzioni di ricerca e selezionare **OK**.  
  
    ![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>Provare quanto segue: Utilizzare LDP per restituire le statistiche sulle query  
Eseguire le operazioni seguenti in un controller di dominio o da un client aggiunti a un dominio o un server in cui sono installati gli strumenti di Active Directory Domain Services.  Ripetere le operazioni seguenti come destinazione il controller di dominio di Windows Server 2012 e Windows Server 2012 R2 controller di dominio.  
  
1.  Rivedere le ["Creazione più efficiente Microsoft applicazioni abilitate per AD"](https://msdn.microsoft.com/library/ms808539.aspx) articolo e fare riferimento ad esso in base alle esigenze.  
  
2.  Utilizzare LDP per abilitare le statistiche di ricerca (vedere [per abilitare il controllo delle statistiche in LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  Eseguire diverse ricerche LDAP e osservare le informazioni statistiche nella parte superiore dei risultati.  Si ripeterà la stessa ricerca in altre attività in modo documentarli blocco note il file di testo.  
  
4.  Eseguire una ricerca LDAP che query optimizer deve essere in grado di ottimizzare a causa di indici di attributi  
  
5.  Tenta di costruire una ricerca che richiede molto tempo per il completamento (è possibile aumentare la **limite di tempo** opzione in modo che la ricerca senza timeout).  
  
### <a name="additional-resources"></a>Risorse aggiuntive  
[Quali sono le ricerche di Active Directory?](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[La modalità di lavoro di ricerche di Active Directory](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[Creazione di applicazioni abilitate alla Directory Microsoft Active più efficiente](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) query LDAP vengono eseguite più lentamente del previsto in Active Directory o servizio di directory LDS/ADAM e 1644 ID evento potrebbero essere registrati  
  
## <a name="BKMK_1644"></a>Evento 1644  
  
### <a name="overview"></a>Panoramica  
Questo aggiornamento aggiunge le statistiche aggiuntive risultati di ricerca LDAP per ID evento 1644 come per facilitare la risoluzione dei problemi.  Inoltre, vi è un nuovo valore del Registro di sistema che può essere utilizzato per abilitare la registrazione su una soglia basati sul tempo.  Questi miglioramenti sono stati resi disponibili in Windows Server 2012 e Windows Server 2008 R2 SP1 tramite KB [2800945](https://support.microsoft.com/kb/2800945) e verrà reso disponibile per Windows Server 2008 SP2.  
  
> [!NOTE]  
> -   Statistiche di ricerca LDAP aggiuntive vengono aggiunte per ID evento 1644 come per facilitare la risoluzione dei problemi non efficiente o costose le ricerche LDAP  
> -   È ora possibile specificare una soglia durata di ricerca (ad es. Registra evento 1644 per le ricerche impiegano più di 100 ms) invece di specificare i valori di soglia risultati di ricerca Expensive e Inefficient  
  
### <a name="background"></a>Informazioni  
Durante la risoluzione dei problemi relativi alle prestazioni di Active Directory, risulta evidente che attività di ricerca LDAP può essere aggiunta come contributo al problema.  Si decide di abilitare la registrazione in modo che è possibile visualizzare le query LDAP costosa o poco efficiente elaborate dal controller di dominio.  Per abilitare la registrazione, è necessario impostare il valore di diagnostica Engineering di campo e può facoltativamente specificare i valori di soglia dei risultati di ricerca costosa o poco efficiente.  Dopo aver attivato il livello di registrazione su un valore pari a 5 campo Engineering, una ricerca che soddisfa questi criteri viene registrata nel registro eventi dei servizi di Directory con un ID evento 1644 come.  
  
L'evento contiene:  
  
-   Client IP e porta  
  
-   Nodo di inizio  
  
-   Filter  
  
-   Ambito di ricerca  
  
-   Selezione dell'attributo  
  
-   Controlli server  
  
-   Voci visitate  
  
-   Voci restituite  
  
Tuttavia, dati chiave mancano l'evento, ad esempio la quantità di tempo impiegato per l'operazione di ricerca e cosa (se presente) indice è stato utilizzato.  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>Statistiche di ricerca aggiuntivo aggiunte all'evento 1644  
  
-   Indici usati  
  
-   Pagine di cui viene fatto riferimento  
  
-   Pagine lette dal disco  
  
-   Pagine pre-lette dal disco  
  
-   Pagine Clean modificate  
  
-   Pagine dirty modificate  
  
-   Tempo di ricerca  
  
-   Attributi impedendo l'ottimizzazione  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>Nuovo valore del Registro di sistema soglia basati sul tempo per la registrazione evento 1644  
Invece di specificare i valori di soglia risultati di ricerca Expensive e Inefficient, è possibile specificare una soglia durata di ricerca.  Se è maggiore o desiderati per registrare tutti i risultati di ricerca che richiedeva 50 ms, si specificherà decimale 50 / 32 hex (oltre a impostare il valore di campo Engineering).  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>Confronto tra i vecchio e nuovo ID evento 1644 come  
PRECEDENTE  
  
![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
NUOVO  
  
![gli aggiornamenti dei servizi directory](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>Provare quanto segue: Usare il registro eventi per restituire le statistiche sulle query  
  
1.  Ripetere le operazioni seguenti come destinazione il controller di dominio di Windows Server 2012 e Windows Server 2012 R2 controller di dominio. Osservare l'evento ID 1644s su entrambi i controller di dominio dopo ogni ricerca.  
  
2.  Utilizzando regedit, abilitare la registrazione di ID evento 1644 come con una soglia basati sul tempo in Windows Server 2012 R2 controller di dominio e il metodo precedente in Windows Server 2012 controller di dominio.  
  
3.  Eseguire diverse ricerche LDAP che superano la soglia e osservare le informazioni statistiche nella parte superiore dei risultati.  Usare le query LDAP che è documentato in precedenza e ripetere le stesse ricerche.  
  
4.  Eseguire una ricerca LDAP che query optimizer non è in grado di ottimizzare perché uno o più attributi non sono indicizzati.  
  
## <a name="BKMK_ADRepl"></a>Miglioramento della velocità effettiva di Active Directory replica  
  
### <a name="overview"></a>Panoramica  
Replica di Active Directory Usa RPC per il trasporto di replica. Per impostazione predefinita, RPC utilizza un buffer di trasmissione da 8 KB e un pacchetto di dimensioni pari a 5 KB. Questo è il risultato finale in cui l'istanza l'invio trasmettere pacchetti di tre (circa 15K vale la pena di dati) e quindi essere necessario attendere per una rete di round trip prima dell'invio di più. Supponendo un 3 MS tempo di round trip, la velocità effettiva più alta saranno circa 40Mbps, anche in 1 Gbps o 10 Gbps reti.  
  
> [!NOTE]  
> -   Questo aggiornamento consente di regolare la velocità massima effettiva replica di Active Directory dal 40Mbps a circa 600 Mbps.  
>   
>     -   Aumenta le dimensioni del buffer di trasmissione RPC che riduce il numero di rete di andata e ritorno  
> -   L'effetto sarà più evidente su ad alta velocità, la rete ad alta latenza.  
  
Questi aggiornamenti aumentano la velocità effettiva massima di circa 600 Mbps modificando le dimensioni del buffer di invio RPC da 8 KB a 256KB.  Questa modifica consente le dimensioni della finestra TCP più di 8 KB, riducendo il numero di rete dei round trip.  
  
> [!NOTE]  
> Non esistono impostazioni configurabili per modificare questo comportamento.  
  
### <a name="additional-resources"></a>Risorse aggiuntive  
[Come funziona il modello di replica di Active Directory](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


