---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: Gestione del rilascio di RID
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f84bcc1aa32e9993903e094fc43feffcbe16a05b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="managing-rid-issuance"></a>Gestione del rilascio di RID

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra la modifica per il ruolo master RID FSMO, tra cui il nuovo rilascio e il monitoraggio delle funzionalità nel master RID e su come analizzare e risolvere i problemi relativi al rilascio di RID.  
  
-   [Gestione del rilascio di RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Risoluzione dei problemi relativi al rilascio di RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Ulteriori informazioni sono disponibili nel [AskDS Blog](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="BKMK_Manage"></a>Gestione del rilascio di RID  
Per impostazione predefinita, un dominio dispone di capacità per circa un miliardo entità di sicurezza, ad esempio utenti, gruppi e computer. Naturalmente, esistono domini con così tanti oggetti usati attivamente. Tuttavia, supporto tecnico clienti Microsoft ha trovato casi in cui:  
  
-   Provisioning di script software o amministrativi accidentalmente in blocco creare utenti, gruppi e computer.  
  
-   Molti gruppi di distribuzione e della sicurezza non utilizzato sono stati creati da utenti delegati  
  
-   Molti controller di dominio sono stati abbassati di livello, ripristinati, o pulire i metadati  
  
-   Sono stati eseguiti recuperi di foreste  
  
-   L'operazione InvalidateRidPool è stata eseguita di frequente  
  
-   Il valore del Registro di sistema dimensione del blocco RID è stato aumentato in modo errato  
  
Tutte queste situazioni consumare RID inutilmente, spesso per errore. In molti anni, alcuni ambienti sono stati esauriti ed è stato necessario eseguire la migrazione a un nuovo dominio o eseguire recuperi di foreste.  
  
Windows Server 2012 risolve i problemi di allocazione del RID che solo divenuti gravi con il tempo e la grande diffusione di Active Directory. Queste includono una migliore registrazione degli eventi, limiti più appropriati e in caso di emergenza - la capacità di raddoppiare la dimensione totale dello spazio RID globale per un dominio.  
  
### <a name="periodic-consumption-warnings"></a>Avvisi periodici sul consumo  
Windows Server 2012 aggiunge eventi di spazio RID globali, verifica che avvisa tempestivamente quando vengono superate principali attività cardine. Il modello calcola la percentuale di dieci (10) utilizzo nel pool globale e registra un evento quando viene raggiunto. Quindi calcola il successivo 10% utilizzato della parte rimanente e il ciclo degli eventi continua. Come si esaurisce lo spazio RID globale, gli eventi subiranno un'accelerazione perché 10% viene raggiunto più rapidamente in un pool decrescente (ma attenuazione del registro eventi impedirà più di una voce all'ora). Registro eventi di sistema in ogni controller di dominio scrive l'evento di avviso Directory-Services-SAM 16658.  
  
Supponendo che un bit 30 globale RID spazio su predefinito, i registri eventi primo, durante l'allocazione del pool che contiene il 107,374,182<sup>nd</sup> RID. La frequenza degli eventi accelera spontaneamente fino all'ultimo checkpoint di 100.000, con 110 eventi generati in totale. Il comportamento è simile per un spazio RID globale 31 bit sbloccato: inizia a 214.748.365 e 117 eventi.  
  
> [!IMPORTANT]  
> Questo evento non è previsto. analizzare l'utente, computer e processi di creazione gruppo immediatamente nel dominio. Creazione di oggetti di più di 100 milioni di dominio Active Directory non è affatto normale.  
  
![Rilascio di RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Eventi di invalidamento del Pool di RID  
Esistono nuovi avvisi di evento indicanti che un pool di RID di controller di dominio locale è stato eliminato. Sono informativi e prevedibili, soprattutto a causa della nuova funzionalità di controller di dominio virtuale. Vedere l'elenco di eventi di seguito per ulteriori informazioni sull'evento.  
  
### <a name="BKMK_RIDBlockMaxSize"></a>Limite della dimensione del blocco di RID  
In genere, un controller di dominio richiede le allocazioni di RID in blocchi di 500 RID in una sola volta. È possibile ignorare questa impostazione predefinita con il valore REG_DWORD del Registro di sistema di seguito in un controller di dominio:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Prima di Windows Server 2012, non esiste nessun valore massimo veniva applicato in tale chiave del Registro di sistema, tranne il valore massimo DWORD implicito (il cui valore 0xffffffff o 4294967295). Questo valore è notevolmente maggiore spazio RID globale totale. Gli amministratori a volte modo inappropriato o casuale configurato dimensione del blocco RID con valori che esaurivano il RID globale a una velocità elevatissima.  
  
In Windows Server 2012, è possibile impostare questo valore del Registro di sistema rispetto a 15.000 nel sistema decimale (0x3a98 nel sistema esadecimale). In questo modo notevole allocazione del RID non autorizzati.  
  
Se si imposta il valore *superiore* a 15.000, il valore viene trattato come 15.000 e il controller di dominio registra l'evento 16653 nel registro eventi dei servizi di Directory a ogni riavvio finché non viene risolto il valore.  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>Sblocco della dimensione dello spazio RID globale  
Prima di Windows Server 2012, lo spazio RID globale era limitato a 2<sup>30</sup> (o 1.073.741.823) RID totali. Una volta raggiunto, solo un dominio foresta o la migrazione di una condizione precedente consentito il ripristino SID la creazione di nuovi - il ripristino di emergenza, da qualsiasi misura. A partire da Windows Server 2012, il 2<sup>31</sup> bit può essere sbloccato per incrementare il pool globale fino a 2.147.483,648 RID.  
  
Servizi di dominio Active Directory memorizza questa impostazione in un speciale attributo nascosto denominato **SidCompatibilityVersion** nel contesto RootDSE di tutti i controller di dominio. Questo attributo non è leggibile con ADSIEdit, LDP o altri strumenti. Per vedere un incremento nello spazio RID globale, esaminare il registro eventi di sistema per l'evento di avviso 16655 di Directory-Services-SAM o usare il comando Dcdiag seguente:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Se si aumenta il pool di RID globale, il pool disponibile verrà impostato su 2.147.483.647 invece il valore predefinito 1.073.741.823. Per esempio:  
  
![Rilascio di RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Questo sblocco ha il *solo* per evitare l'esaurimento dei RID e deve essere utilizzato *solo* in combinazione con imposizione del limite massimo RID (vedere la sezione successiva). Non impostarlo "preventivamente" in questo in ambienti con milioni di RID rimanenti e una crescita bassa, problemi di compatibilità delle applicazioni potenzialmente esistenti con i SID generati dal pool di RID sbloccato.  
>   
> Questa operazione di sblocco non può essere invertita o rimossa, se non da un ripristino completo della foresta ai backup precedenti.  
  
#### <a name="important-caveats"></a>Avvertenze importanti  
Windows Server 2003 e controller di dominio di Windows Server 2008 non possono rilasciare i RID quando 31 del pool RID globale<sup>st</sup> viene sbloccato. Windows Server 2008 R2 domain controllers *can* use 31<sup>st</sup> bit RIDs *but only if* they have hotfix [KB 2642658](https://support.microsoft.com/kb/2642658) installed. Controller di dominio non supportati e senza patch considerano il pool di RID globale come esaurito quando viene sbloccato.  
  
Questa funzionalità non viene applicata da qualsiasi livello funzionale di dominio. Prestare molta attenzione che esiste solo aggiornato i controller di dominio di Windows Server 2008 R2 o Windows Server 2012 nel dominio.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implementazione dello spazio RID globale sbloccato  
Per sbloccare il pool di RID di 31<sup>st</sup> bit dopo aver ricevuto l'avviso di limite massimo del RID (vedere sotto) eseguire i passaggi seguenti:  
  
1.  Verificare che il Master RID ruolo è in esecuzione in un controller di dominio Windows Server 2012. In caso contrario, trasferirlo in un controller di dominio Windows Server 2012.  
  
2.  Eseguire LDP.exe  
  
3.  Fare clic su di **connessione** dal menu **Connetti** per Windows Server 2012 RID Master sulla porta 389 e quindi fare clic su **binding** come amministratore di dominio.  
  
4.  Fare clic su di **Sfoglia** dal menu **modifica**.  
  
5.  Assicurarsi che **DN** è vuoto.  
  
6.  In **Modifica attributo voce**, tipo:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  In **valori**, tipo:  
  
    ```  
    1  
    ```  
  
8.  Assicurarsi che **Aggiungi** sia selezionato nel **operazione** e fare clic su **invio**. Aggiorna il **elenco voci**.  
  
9. Selezionare il **sincrona** e **Extended** opzioni, quindi fare clic su **eseguire**.  
  
    ![Rilascio di RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. Se ha esito positivo, Mostra finestra di output di LDP:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Rilascio di RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Verificare il pool di RID globale aumentato esaminando il registro eventi di sistema sul controller di dominio per l'evento informativo Directory-Services-SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>Imposizione del limite massimo di RID  
Per offrire una certa protezione e sensibilizzare maggiormente gli amministratori, Windows Server 2012 introduce un limite massimo artificiale per l'intervallo di RID globale a dieci (10) % rimanente RID nello spazio globale. Se all'interno di uno (1) % del limite massimo artificiale, i controller di dominio che richiedono i pool di RID scrivono evento avviso Directory-Services-SAM 16656 registro degli eventi di sistema. Quando raggiungono il limite massimo di 10% per FSMO del Master RID, scrive l'evento Directory-Services-SAM 16657 al proprio registro eventi di sistema e verrà non allocheranno nessun altro pool di RID finché il limite massimo. In questo modo è possibile valutare lo stato del master RID nel dominio e risolvere i potenziali allocazione del RID fuori controllo; si evita anche i domini esauriscano l'intero spazio RID.  
  
Questo limite massimo è hardcoded al 10% dello spazio RID disponibile rimanente. Vale a dire il limite massimo viene attivato quando il master RID alloca un pool che include il RID corrispondente al novanta (90) % dello spazio RID globale.  
  
-   Per i domini predefiniti, il primo punto trigger è 2<sup>30</sup>-1 * 0,90 = 966.367.640 (o 107.374.183 RID rimanenti).  
  
-   Per i domini con uno spazio RID di 31 bit sbloccato, il punto trigger è 2<sup>31</sup>-1 * 0,90 = 1.932.735.282 RID (o 214.748.365 RID rimanenti).  
  
Se attivato, il master RID imposta l'attributo di Active Directory **msDS-RIDPoolAllocationEnabled** (nome comune **MS**) su FALSE nell'oggetto:  
  
CN = RID Manager$, CN = System, DC =*<domain>*  
  
Questo scritto l'evento 16657 e si impedisce l'ulteriore rilascio di blocchi di RID a tutti i controller di dominio. I controller di dominio continuano a utilizzare tutti i pool di RID in sospeso già rilasciati.  
  
Per rimuovere il blocco e consentire l'allocazione del pool RID continuare, impostare il valore su TRUE. Su allocazione di RID successiva eseguita dal master RID, verrà restituito l'attributo per il valore predefinito NOT SET. In seguito, non esistono altri limiti massimi e infine, lo spazio RID globale si esaurisce, che richiede la migrazione di ripristino o dominio dell'insieme di strutture.  
  
#### <a name="removing-the-ceiling-block"></a>Rimozione del blocco di limite massimo  
Per rimuovere il blocco una volta raggiunto il limite massimo artificiale, eseguire i passaggi seguenti:  
  
1.  Verificare che il Master RID ruolo è in esecuzione in un controller di dominio Windows Server 2012. In caso contrario, trasferirlo in un controller di dominio Windows Server 2012.  
  
2.  Eseguire LDP.exe.  
  
3.  Fare clic su di **connessione** dal menu *Connetti* per Windows Server 2012 RID Master sulla porta 389 e quindi fare clic su **binding** come amministratore di dominio.  
  
4.  Fare clic su di **visualizzazione** dal menu **albero**, quindi per la **DN di Base** selezionare il contesto dei nomi di dominio del Master RID. Fare clic su **Ok**.  
  
5.  Nel riquadro di spostamento, analizza il **CN = System** contenitore e fare clic su di **CN = RID Manager$** oggetto. Fare clic su di esso e fare clic su **modifica**.  
  
6.  In Modifica attributo voce digitare:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  In **valori**, tipo (in maiuscolo):  
  
    ```  
    TRUE  
    ```  
  
8.  Selezionare **sostituire** in **operazione** e fare clic su **invio**. Aggiorna il **elenco voci**.  
  
9. Abilitare il **sincrona** e **Extended** opzioni, quindi fare clic su **eseguire**:  
  
    ![Rilascio di RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. Se ha esito positivo, Mostra finestra di output di LDP:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Rilascio di RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Altre correzioni relative al RID  
Sistemi operativi Windows Server precedenti aveva un pool di RID perdita quando l'attributo rIDSetReferences è mancante. To resolve this problem on domain controllers that run Windows Server 2008 R2, install the hotfix from [KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problemi RID non risolti  
È sempre esistita una perdita di RID in errore durante la creazione di account; Quando si crea un account, errore Usa ancora di un RID. L'esempio più comune consiste nel creare un utente con una password che non soddisfa la complessità.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Correzioni relative al RID per versioni precedenti di Windows Server  
Tutte le correzioni e le modifiche illustrate sopra sono rilasciate come hotfix per Windows Server 2008 R2. Non esistono attualmente alcun hotfix di Windows Server 2008 pianificato o in corso.  
  
## <a name="BKMK_Tshoot"></a>Risoluzione dei problemi relativi al rilascio di RID  
  
### <a name="introduction-to-troubleshooting"></a>Introduzione alla risoluzione dei problemi  
Risoluzione dei problemi di rilascio di RID richiede un metodo logico e lineare. A meno che non si siano monitorando i registri eventi attentamente per errori e avvisi generati dal RID, le prime indicazioni di un problema saranno probabilmente le creazioni di account non riuscita. La chiave per la risoluzione dei problemi di rilascio di RID consiste nel comprendere quando il sintomo è previsto oppure No. molti problemi di rilascio di RID possono influire solo un controller di dominio e non avere nulla a che fare con miglioramenti dei componenti. Il semplice diagramma seguente aiuta a prendere le decisioni più chiaro:  
  
![Rilascio di RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Opzioni di risoluzione dei problemi  
  
#### <a name="logging-options"></a>Opzioni di registrazione  
Tutta la registrazione nel rilascio di RID viene eseguita nel registro eventi di sistema, in Directory-Services-SAM di origine. La registrazione è abilitata e configurata per ottenere il massimo livello di dettaglio, per impostazione predefinita. Se non vengono registrate voci per le modifiche dei nuovi componenti in Windows Server 2012, trattare il problema come un classico (ovvero legacy, prima di Windows Server 2012) problema rilascio di RID in Windows 2008 R2 o sistemi operativi precedenti.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilità e comandi per la risoluzione dei problemi  
Per risolvere i problemi non spiegati dai log menzionati in precedenza - problemi di rilascio di RID in particolare precedenti - utilizzare il seguente elenco di strumenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia generale per la risoluzione dei problemi di configurazione del Controller di dominio  
  
1.  L'errore è causato da un semplice problema di disponibilità dei controller di dominio o di autorizzazioni?  
  
    1.  Si sta tentando di creare una sicurezza entità senza le autorizzazioni necessarie? Esaminare l'output per errori di accesso negato.  
  
    2.  È disponibile un controller di dominio? Esaminare l'errore o LDAP o dominio controller disponibilità messaggi restituiti.  
  
2.  L'errore restituito in particolare cita RID ed è abbastanza specifico per ricavarne delle linee guida? In caso affermativo, seguire le linee guida.  
  
3.  L'errore restituito in particolare cita RID, ma in caso contrario non specifiche? Ad esempio, "Impossibile creare l'oggetto perché il servizio Directory è stato in grado di allocare l'identificatore relativo."  
  
    1.  Examine the System Event log on the domain controller for "legacy" (pre-Windows Server 2012) RID events detailed in [RID Pool Request](https://technet.microsoft.com/en-us/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Esaminare l'evento di sistema sul controller di dominio e il Master RID per nuovi eventi indicanti un blocco descritti di seguito in questo argomento (16655, 16656, 16657).  
  
    3.  Convalidare l'integrità della replica di Active Directory con Repadmin.exe e Master RID disponibilità con **Dcdiag.exe /test: RidManager /v**. Se questi test sono inconcludenti, abilitare le acquisizioni di rete su due lati tra il controller di dominio e il Master RID.  
  
### <a name="troubleshooting-specific-problems"></a>Risoluzione di problemi specifici  
I seguenti nuovi messaggi accedere nel registro eventi di sistema nei controller di dominio Windows Server 2012. Sistemi, ad esempio System Center Operations Manager, di verifica dell'integrità AD automatizzata monitoreranno questi eventi. sono tutti rilevanti e alcuni sono indicatori di problemi critici dei domini.  
  
|||  
|-|-|  
|ID evento|16653|  
|Origine|Directory-Services-SAM|  
|Livello di gravità|Avviso|  
|Messaggio|Una dimensione del pool di identificatori di account (RID) che è stata configurata da un amministratore è maggiore del valore massimo supportato. Verrà utilizzato il valore massimo %%1 quando il controller di dominio è il master RID.<br /><br />Per ulteriori informazioni, vedere [limite della dimensione del blocco RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Note e risoluzione|Il valore massimo per la dimensione del blocco RID è ora 15000 nel sistema decimale (3a98 nel sistema esadecimale). Un controller di dominio non può richiedere più di 15.000 RID. Questo evento viene registrato a ogni bootstrap finché il valore è impostato su un valore pari o inferiore a questo valore massimo.|  
  
|||  
|-|-|  
|ID evento|16654|  
|Origine|Directory-Services-SAM|  
|Livello di gravità|Informativo|  
|Messaggio|Un pool di identificatori di account (RID) è stato invalidato. Ciò può verificarsi nei seguenti casi previsti:<br /><br />1. un controller di dominio viene ripristinato da backup.<br /><br />2. un controller di dominio in una macchina virtuale viene ripristinato da uno snapshot.<br /><br />3. un amministratore ha invalidato manualmente il pool.<br /><br />See https://go.microsoft.com/fwlink/?LinkId=226247 for more information.|  
|Note e risoluzione|Se questo evento è imprevisto, contattare tutti gli amministratori di dominio e determinare quale di essi ha eseguito l'azione. Il registro eventi dei servizi Directory contiene anche altre informazioni su quando è stata eseguita una delle seguenti operazioni.|  
  
|||  
|-|-|  
|ID evento|16655|  
|Origine|Directory-Services-SAM|  
|Livello di gravità|Informativo|  
|Messaggio|Il limite massimo globale per gli identificatori di account (RID) è stato aumentato a %1.|  
|Note e risoluzione|Se questo evento è imprevisto, contattare tutti gli amministratori di dominio e determinare quale di essi ha eseguito l'azione. Questo evento evidenzia l'incremento del RID globale del pool di dimensione oltre il valore predefinito è 2<sup>30</sup>e non si verificherà automaticamente. solo mediante un'azione amministrativa.|  
  
|||  
|-|-|  
|ID evento|16656|  
|Origine|Directory-Services-SAM|  
|Livello di gravità|Avviso|  
|Messaggio|Il limite massimo globale per gli identificatori di account (RID) è stato aumentato a %1.|  
|Note e risoluzione|Azione necessaria: Un pool di identificatori di account (RID) è stato allocato al controller di dominio. Il valore del pool indica che il dominio ha utilizzato una parte considerevole degli account-identificatori disponibili.<br /><br />Quando il dominio raggiungerà la soglia seguenti del totale disponibile gli identificatori di account rimanente verrà attivato un meccanismo di protezione: %1.  Tale meccanismo impedirà la creazione di account fino a quando non si riabiliterà manualmente l'allocazione di identificatori di account nel controller di dominio del master RID.<br /><br />See https://go.microsoft.com/fwlink/?LinkId=228610 for more information.|  
  
|||  
|-|-|  
|ID evento|16657|  
|Origine|Directory-Services-SAM|  
|Livello di gravità|Errore|  
|Messaggio|Azione necessaria: Il dominio ha utilizzato una parte considerevole dei totale disponibile gli identificatori di account (RID). Un meccanismo di protezione è stato attivato perché i totale disponibile gli identificatori di account rimanenti è inferiore a: X % [argomento del limite massimo artificiale].<br /><br />Tale meccanismo impedisce la creazione di account finché non si riabiliterà manualmente l'allocazione di identificatori di account nel controller di dominio del master RID.<br /><br />È molto importante che vengano eseguite determinate operazioni diagnostiche prima di riabilitare la creazione per garantire che il dominio non utilizzi gli identificatori di account in modo eccessivo di account. Eventuali problemi rilevati devono essere risolti prima di riabilitare la creazione di account.<br /><br />Possono causare errori per diagnosticare e risolvere eventuali problemi sottostanti che causano un tasso eccessivo del consumo di identificatori di account di esaurimento di identificatori di account nel dominio dopo i quali la creazione di account in modo permanente disattivate in questo dominio.<br /><br />See https://go.microsoft.com/fwlink/?LinkId=228610 for more information.|  
|Note e risoluzione|Contattare tutti gli amministratori di dominio e informarli che è non possibile creare alcun altre entità di sicurezza in questo dominio finché non viene eseguito l'override di questo tipo di protezione. Per ulteriori informazioni su come eseguire l'override della protezione e possibilità di incrementare il RID globale del pool, vedere [globale RID spazio sblocco della dimensione](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|ID evento|16658|  
|Origine|Directory-Services-SAM|  
|Livello di gravità|Avviso|  
|Messaggio|Questo evento è un aggiornamento periodico sulla quantità totale rimanente di disponibili gli identificatori di account (RID). Il numero di identificatori di account rimanenti è circa: %1.<br /><br />Creazione di account, quando si esauriscono che non nuovi account può essere creato nel dominio, vengono utilizzati gli identificatori di account.<br /><br />See https://go.microsoft.com/fwlink/?LinkId=228745 for more information.|  
|Note e risoluzione|Contattare tutti gli amministratori di dominio e informarli che il consumo di RID ha superato un'attività cardine principale; determinare se si tratta di un comportamento previsto o meno esaminando gli schemi di creazione della protezione dominio trusted. In realtà questo evento sarebbe insolito, quanto significa che almeno circa 100 milioni di RID sono stati assegnati.|  
  
## <a name="see-also"></a>Vedere anche  
[Gestione del rilascio di RID in Windows Server 2012](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


