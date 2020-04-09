---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: Gestione del rilascio di RID
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: aabb48643c58019339b96e9a4c54df8e1d66893c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823244"
---
# <a name="managing-rid-issuance"></a>Gestione del rilascio di RID

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono illustrate le modifiche apportate al ruolo FSMO del master RID, inclusa la nuova funzionalità di rilascio e monitoraggio nel master RID, e viene spiegato come analizzare e risolvere i problemi relativi al rilascio di RID.  
  
-   [Gestione del rilascio di RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Risoluzione dei problemi di rilascio RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Altre informazioni sono disponibili nel [Blog di nel](https://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="managing-rid-issuance"></a><a name="BKMK_Manage"></a>Gestione del rilascio RID  
Per impostazione predefinita, la capacità di un dominio è di circa un miliardo di entità di sicurezza, ad esempio utenti, gruppi e computer. Ovviamente non esistono domini con così tanti oggetti usati attivamente. Tuttavia, il Servizio Supporto Tecnico Clienti Microsoft ha trovato casi in cui:  
  
-   Il provisioning di script software o amministrativi ha accidentalmente creato in blocco utenti, gruppi e computer  
  
-   Molti gruppi di sicurezza e di distribuzione non utilizzati sono stati creati da utenti delegati  
  
-   Molti controller di dominio sono stati abbassati di livello, ripristinati o privati dei metadati  
  
-   Sono stati eseguiti recuperi di foreste  
  
-   L'operazione InvalidateRidPool è stata eseguita di frequente  
  
-   Il valore del Registro di sistema relativo alla dimensione blocco di RID è stato aumentato in modo errato  
  
In tutte queste situazioni i RID vengono consumati senza necessità, spesso per errore. In molti anni, in alcuni ambienti i RID sono stati esauriti ed è stato necessario eseguire la migrazione a un nuovo dominio o eseguire recuperi di foreste.  
  
Windows Server 2012 risolve solo i problemi relativi all'allocazione del RID divenuti gravi con il tempo e la grande diffusione di Active Directory. Sono incluse una migliore registrazione degli eventi, limiti più appropriati e la possibilità di-in un'emergenza di raddoppiare le dimensioni complessive dello spazio globale RID per un dominio.  
  
### <a name="periodic-consumption-warnings"></a>Avvisi periodici sul consumo  
In Windows Server 2012 è stata aggiunta la registrazione degli eventi di spazio RID globale, che avvisa tempestivamente quando vengono superate le principali attività cardine. Il modello calcola il 10% di utilizzo nel pool globale e registra un evento quando viene raggiunto. Calcola quindi il successivo 10% utilizzato della parte rimanente e il ciclo degli eventi continua. Man mano che lo spazio RID globale si esaurisce, gli eventi subiranno un'accelerazione perché il 10% viene raggiunto più rapidamente in un pool decrescente (ma l'attenuazione del registro eventi non consentirà più di una voce all'ora). Il registro eventi di sistema di ogni controller di dominio scrive l'evento avviso Directory-Services-SAM 16658.  
  
Presumendo uno spazio RID globale di 30 bit predefinito, il primo evento viene registrato quando si alloca il pool contenente il RID numero 107.374.182.<sup></sup> La frequenza degli eventi accelera spontaneamente fino all'ultimo checkpoint di 100.000, con 110 eventi generati in totale. Il comportamento è simile per lo spazio RID globale di 31 bit sbloccato: si inizia a 214.748.365 e si termina dopo 117 eventi.  
  
> [!IMPORTANT]  
> Questo evento non è previsto. Analizzare immediatamente i processi di creazione di utenti, computer e gruppi nel dominio. La creazione di più di 100 milioni di oggetti Servizi di dominio Active Directory non è affatto normale.  
  
![Rilascio RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Eventi di invalidamento del pool di RID  
Esistono nuovi avvisi di evento indicanti che un pool di RID di un controller di dominio locale è stato rimosso. Sono informativi e prevedibili, soprattutto a causa della nuova funzionalità di controller di dominio virtuale. Per i dettagli dell'evento, vedere l'elenco di eventi seguente.  
  
### <a name="rid-block-size-limit"></a><a name="BKMK_RIDBlockMaxSize"></a>Limite dimensioni blocco RID  
In genere, un controller di dominio richiede le allocazioni di RID in blocchi di 500 RID per volta. È possibile sostituire questa impostazione predefinita con il seguente valore REG_DWORD del Registro di sistema su un controller di dominio:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Prima di Windows Server 2012, nessun valore massimo veniva applicato a questa chiave del Registro di sistema, tranne il valore massimo DWORD implicito (il cui valore è 0xffffffff o 4294967295). Questo valore è considerevolmente più grande dello spazio RID globale totale. Gli amministratori a volte configuravano in modo inappropriato o casuale la dimensione del blocco di RID con valori che esaurivano il RID globale a una velocità elevatissima.  
  
In Windows Server 2012, questo valore del Registro di sistema può essere impostato al massimo su 15.000 nel sistema decimale (0x3A98 nel sistema esadecimale). Questo impedisce un'allocazione eccessiva involontaria di RID.  
  
Se si imposta un valore *superiore* a 15.000, il valore considerato è comunque 15.000 e il controller di dominio registra l'evento 16653 nel registro eventi di Servizi directory a ogni riavvio finché il valore non è corretto.  
  
### <a name="global-rid-space-size-unlock"></a><a name="BKMK_GlobalRidSpaceUnlock"></a>Sblocco dimensioni spazio RID globale  
Prima di Windows Server 2012, lo spazio RID globale era limitato a 2<sup>30</sup> (o 1.073.741.823) RID totali. Una volta raggiunto, solo una migrazione del dominio o il ripristino di una condizione precedente della foresta consentiva la creazione di nuovi SID: si trattava in pratica di un ripristino di emergenza. A partire da Windows Server 2012, è possibile sbloccare i 2<sup>31</sup> bit in modo da incrementare il pool globale fino a 2.147.483,648 RID.  
  
Servizi di dominio Active Directory memorizza questa impostazione in un speciale attributo nascosto denominato **SidCompatibilityVersion** nel contesto RootDSE di tutti i controller di dominio. Questo attributo non è leggibile con ADSIEdit, LDP o altri strumenti. Per vedere un incremento nello spazio RID globale, cercare nel registro eventi di sistema l'evento di avviso 16655 di Directory-Services-SAM o usare il comando Dcdiag seguente:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Se si incrementa il pool di RID globale, il pool disponibile verrà impostato su 2.147.483.647 invece che sul valore predefinito 1.073.741.823. Ad esempio,  
  
![Rilascio RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Questo sblocco ha il *solo* scopo di evitare l'esaurimento dei RID e deve essere usato *solo* insieme all'imposizione di un limite massimo per il RID (vedere la sezione successiva). Non impostarlo "preventivamente" in ambienti con milioni di RID rimanenti e una crescita bassa, perché esistono potenziali problemi di compatibilità delle applicazioni con i SID generati dal pool di RID sbloccato.  
>   
> Questa operazione di sblocco non può essere invertita o rimossa, se non da un ripristino completo della foresta ai backup precedenti.  
  
#### <a name="important-caveats"></a>Avvertenze importanti  
I controller di dominio di Windows Server 2003 e di Windows Server 2008 non possono rilasciare i RID quando il viene sbloccato il trentunesimo pool di RID<sup></sup>globali. I controller di dominio di Windows Server 2008 R2 *possono* usare RID<sup>a 32 bit</sup> , *ma solo se* hanno installato l'hotfix [KB 2642658](https://support.microsoft.com/kb/2642658) . I controller di dominio non supportati e senza patch considerano il pool di RID globale come esaurito quando viene sbloccato.  
  
Questa funzionalità non viene applicata da tutti i livelli di funzionalità del dominio. Controllare con particolare attenzione che nel dominio esistano solo controller di dominio di Windows Server 2012 o di Windows Server 2008 R2 aggiornati.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implementazione dello spazio RID globale sbloccato  
Per sbloccare il pool di RID al 31<sup>o</sup> bit dopo aver ricevuto l'avviso relativo al limite massimo del RID (vedere sotto), eseguire i passaggi seguenti:  
  
1.  Verificare che il ruolo master RID sia in esecuzione su un controller di dominio di Windows Server 2012. In caso contrario, trasferirlo in un controller di dominio di Windows Server 2012.  
  
2.  Eseguire LDP.exe.  
  
3.  Scegliere **Connetti** dal menu **Connessione** per il master RID di Windows Server 2012 sulla porta 389 e quindi fare clic su **Binding** come amministratore di dominio.  
  
4.  Scegliere **Modifica** dal menu **Sfoglia**.  
  
5.  Verificare che **DN** sia vuoto.  
  
6.  In **Modifica attributo voce** digitare:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  In **Valori** digitare:  
  
    ```  
    1  
    ```  
  
8.  Verificare che **Aggiungi** sia selezionato in **Operazione** e fare clic su **Immetti**. L'**Elenco voci** verrà aggiornato.  
  
9. Selezionare le opzioni **Sincrona** ed **Esteso**, quindi fare clic su **Esegui**.  
  
    ![Rilascio RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. In caso di esito positivo, nella finestra di output LDP viene visualizzato:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Rilascio RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Per verificare l'incremento del pool di RID globale, cercare nel registro eventi di sistema su questo controller di dominio l'evento informativo Directory-Services-SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>Imposizione del limite massimo per il RID  
Per offrire una certa protezione e sensibilizzare maggiormente gli amministratori, in Windows Server 2012 è stato introdotto un limite massimo artificiale per l'intervallo di RID globale, che scatta quando rimane il 10% dei RID nello spazio globale. Se entro l'1% del limite massimo artificiale, i controller di dominio che richiedono i pool di RID scrivono l'evento di avviso Directory-Services-SAM 16656 nei registri eventi di sistema. Quando raggiungono il 10% del limite massimo per il ruolo FSMO del master RID, scrivono l'evento Directory-Services-SAM 16657 nel registro eventi di sistema e non allocheranno nessun altro pool di RID finché il limite massimo non viene sostituito. In questo modo non solo si è costretti a valutare lo stato del master RID nel dominio e a risolvere una potenziale allocazione eccessiva di RID, ma si evita anche che i domini esauriscano l'intero spazio RID.  
  
Questo limite massimo è hardcoded al 10% dello spazio RID disponibile rimanente, vale a dire che il limite massimo viene attivato quando il master RID alloca un pool che include il RID corrispondente al 90% dello spazio RID globale.  
  
-   Per i domini predefiniti, il primo punto trigger è 2<sup>30</sup>-1 * 0,90 = 966.367.640 (o 107.374.183 RID rimanenti).  
  
-   Per i domini con uno spazio RID di 31 bit sbloccato, il punto trigger è 2<sup>31</sup>-1 * 0,90 = 1.932.735.282 RID (o 214.748.365 RID rimanenti).  
  
Se attivato, il master RID imposta l'attributo di Active Directory **msDS-RIDPoolAllocationEnabled** (nome comune **ms-DS-RID-Pool-Allocation-Enabled**) su FALSE nell'oggetto:  
  
CN = RID Manager $, CN = System, DC = *<domain>*  
  
Viene scritto l'evento 16657 e si impedisce l'ulteriore rilascio di blocchi di RID a tutti i controller di dominio. I controller di dominio continuano a utilizzare i pool di RID in sospeso già rilasciati.  
  
Per rimuovere il blocco e consentire il proseguimento dell'allocazione del pool di RID, impostare il valore su TRUE. Nell'allocazione di RID successiva eseguita dal master RID, verrà ripristinato il valore predefinito NOT SET dell'attributo. In seguito non sono previsti altri limiti massimi e infine lo spazio RID globale si esaurisce e si rende necessario il ripristino della foresta o la migrazione del dominio.  
  
#### <a name="removing-the-ceiling-block"></a>Rimozione del blocco di limite massimo  
Per rimuovere il blocco una volta raggiunto il limite massimo artificiale, eseguire i passaggi seguenti:  
  
1.  Verificare che il ruolo master RID sia in esecuzione su un controller di dominio di Windows Server 2012. In caso contrario, trasferirlo in un controller di dominio di Windows Server 2012.  
  
2.  Eseguire LDP.exe.  
  
3.  Scegliere **Connetti** dal menu *Connessione* per il master RID di Windows Server 2012 sulla porta 389 e quindi fare clic su **Binding** come amministratore di dominio.  
  
4.  Scegliere **Albero** dal menu **Visualizza**, quindi per **DN di base** selezionare il contesto dei nomi del dominio del master RID. Fare clic su **OK**.  
  
5.  Nel pannello di navigazione eseguire il drill-down nel contenitore **CN=System** e fare clic sull'oggetto **CN=RID Manager$** . Fare clic con il pulsante destro del mouse e scegliere **Modifica**.  
  
6.  In Modifica attributo voce digitare:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  In **Valori** digitare (in maiuscolo):  
  
    ```  
    TRUE  
    ```  
  
8.  Selezionare **Sostituisci** in **Operazione** e fare clic su **Immetti**. L'**Elenco voci** verrà aggiornato.  
  
9. Attivare le opzioni **Sincrona** ed **Esteso**, quindi fare clic su **Esegui**:  
  
    ![Rilascio RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. In caso di esito positivo, nella finestra di output LDP viene visualizzato:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Rilascio RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Altre correzioni relative al RID  
Nei precedenti sistemi operativi Windows Server si verifica una perdita di pool di RID quando l'attributo rIDSetReferences è mancante. Per risolvere questo problema nei controller di dominio che eseguono Windows Server 2008 R2, installare l'hotfix da [KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problemi del RID non risolti  
È sempre esistita una perdita di RID in caso di errore di creazione di un account. Quando si crea un account, l'errore causa ancora il consumo di un RID. L'esempio più comune è la creazione di un utente con una password che non soddisfa i requisiti di complessità.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Correzioni relative al RID per versioni precedenti di Windows Server  
Tutte le correzioni e le modifiche illustrate sopra sono state rilasciate come hotfix per Windows Server 2008 R2. Nessun hotfix per Windows Server 2008 è attualmente pianificato o in corso.  
  
## <a name="troubleshooting-rid-issuance"></a><a name="BKMK_Tshoot"></a>Risoluzione dei problemi di rilascio RID  
  
### <a name="introduction-to-troubleshooting"></a>Introduzione alla risoluzione dei problemi  
La risoluzione dei problemi di rilascio di RID richiede un metodo logico e lineare. A meno che non si monitorino attentamente i registri eventi per individuare gli avvisi e gli errori generati dal RID, le prime indicazioni di un problema saranno probabilmente date da creazioni di account non riuscite. Per risolvere i problemi di rilascio di RID, è indispensabile comprendere quando il sintomo è previsto oppure no. Molti problemi di rilascio di RID potrebbero interessare un solo controller di dominio e non avere nulla a che fare con i miglioramenti dei componenti. Il semplice diagramma seguente aiuta a decidere più facilmente:  
  
![Rilascio RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Opzioni di risoluzione dei problemi  
  
#### <a name="logging-options"></a>Opzioni di registrazione  
Tutta la registrazione nel rilascio di RID viene eseguita nel registro eventi di sistema, in Directory-Services-SAM di origine. La registrazione è abilitata e configurata per il massimo livello di dettaglio. Se non vengono registrate voci per le modifiche dei nuovi componenti in Windows Server 2012, trattare il problema come un classico (ovvero legacy, prima di Windows Server 2012) problema di rilascio di RID in Windows 2008 R2 o sistemi operativi precedenti.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilità e comandi per la risoluzione dei problemi  
Per risolvere i problemi non spiegati dai log menzionati in precedenza, in particolare i problemi di rilascio di RID precedenti, usare l'elenco seguente di strumenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia generale per la risoluzione dei problemi di configurazione dei controller di dominio  
  
1.  L'errore è causato da un semplice problema di autorizzazioni o di disponibilità del controller di dominio?  
  
    1.  Si sta cercando di creare un'entità di sicurezza senza le autorizzazioni necessarie? Esaminare l'output per individuare errori di accesso negato.  
  
    2.  È disponibile un controller di dominio? Esaminare l'errore restituito oppure i messaggi sulla disponibilità di LDAP o del controller di dominio.  
  
2.  L'errore restituito cita alcuni RID in particolare ed è abbastanza specifico per ricavarne delle linee guida? Se sì, seguire le linee guida.  
  
3.  L'errore restituito cita alcuni RID in particolare, ma non fornisce altre indicazioni specifiche? Ad esempio, "Impossibile creare l'oggetto a causa dell'errore: Il servizio directory non è in grado di allocare l'identificatore relativo".  
  
    1.  Esaminare il registro eventi di sistema nel controller di dominio per gli eventi RID "Legacy" (precedenti a Windows Server 2012) descritti in dettaglio nella [richiesta del pool di RID](https://technet.microsoft.com/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Esaminare l'evento di sistema sul controller di dominio e il master RID per individuare nuovi eventi indicanti un blocco descritti in dettaglio più avanti in questo argomento (16655, 16656, 16657).  
  
    3.  Convalidare l'integrità della replica di Active Directory con Repadmin.exe e la disponibilità del master RID con **Dcdiag.exe /test:ridmanager /v**. Se questi test sono inconcludenti, abilitare le acquisizioni di rete su due lati tra il controller di dominio e il master RID.  
  
### <a name="troubleshooting-specific-problems"></a>Risoluzione di problemi specifici  
I nuovi messaggi seguenti vengono registrati nel registro eventi di sistema sui controller di dominio di Windows Server 2012. I sistemi di registrazione dell'integrità AD automatizzata, come System Center Operations Manager, monitoreranno questi eventi. Sono tutti rilevanti e alcuni sono indicatori di problemi critici dei domini.  
  
|||  
|-|-|  
|ID evento|16653|  
|Origine|Directory-Services-SAM|  
|Severity|Avviso|  
|Message|Una dimensione del pool per gli identificatori di account (RID) configurata da un amministratore supera il limite massimo supportato. Quando il controller di dominio sarà il master RID, verrà utilizzato il valore massimo %1.<p>Per ulteriori informazioni, vedere [Limite della dimensione del blocco RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Note e risoluzione|Il valore massimo per la dimensione del blocco RID è ora 15000 nel sistema decimale (3A98 nel sistema esadecimale). Un controller di dominio non può richiedere più di 15.000 RID. Questo evento viene registrato a ogni bootstrap finché il valore viene impostato su un valore pari o inferiore a questo valore massimo.|  
  
|||  
|-|-|  
|ID evento|16654|  
|Origine|Directory-Services-SAM|  
|Severity|Informazioni|  
|Message|Un pool di identificatori di account (RID) è stato invalidato. Questa situazione può verificarsi nei seguenti casi previsti:<p>1. il backup viene ripristinato da un controller di dominio.<p>2. un controller di dominio in esecuzione in una macchina virtuale viene ripristinato da uno snapshot.<p>3. un amministratore ha invalidato manualmente il pool.<p>Per ulteriori informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=226247.|  
|Note e risoluzione|Se questo evento è imprevisto, contattare tutti gli amministratori di dominio e determinare chi tra essi ha eseguito l'azione. Il registro eventi Servizi directory contiene anche altre informazioni su quando è stato eseguito uno di questi passaggi.|  
  
|||  
|-|-|  
|ID evento|16655|  
|Origine|Directory-Services-SAM|  
|Severity|Informazioni|  
|Message|Il limite massimo globale per gli identificatori di account (RID) è stato portato a %1.|  
|Note e risoluzione|Se questo evento è imprevisto, contattare tutti gli amministratori di dominio e determinare chi tra essi ha eseguito l'azione. Questo evento evidenzia l'incremento della dimensione totale del pool di RID oltre il valore predefinito di 2<sup>30</sup> e non si verificherà automaticamente, ma solo in seguito a un'azione amministrativa.|  
  
|||  
|-|-|  
|ID evento|16656|  
|Origine|Directory-Services-SAM|  
|Severity|Avviso|  
|Message|Il limite massimo globale per gli identificatori di account (RID) è stato portato a %1.|  
|Note e risoluzione|Azione necessaria: un pool di identificatori di account (RID) è stato allocato al controller di dominio corrente. Il valore del pool indica che il dominio ha utilizzato una parte considerevole degli identificatori di account disponibili.<p>Verrà attivato un meccanismo di protezione quando il dominio raggiunge la soglia seguente di totale identificatori di account disponibili rimanenti: %1.  Tale meccanismo impedirà la creazione di account finché non si riabiliterà manualmente l'allocazione di identificatori di account sul controller di dominio del master RID.<p>Per ulteriori informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=228610.|  
  
|||  
|-|-|  
|ID evento|16657|  
|Origine|Directory-Services-SAM|  
|Severity|Errore|  
|Message|Azione necessaria: il dominio ha utilizzato una parte considerevole degli identificatori di account (RID) disponibili. È stato attivato un meccanismo di protezione perché gli identificatori di account disponibili rimanenti sono inferiori a: X% [argomento di Ceiling artificiale].<p>Tale meccanismo impedisce la creazione di account finché non si riabilita manualmente l'allocazione di identificatori di account sul controller di dominio del master RID.<p>È estremamente importante eseguire determinate operazioni diagnostiche prima di riabilitare la creazione di account per garantire che il dominio non utilizzi gli identificatori di account in modo eccessivo. Eventuali problemi rilevati devono essere risolti prima di riabilitare la creazione di account.<p>Se i problemi sottostanti che causano un tasso eccessivo di utilizzo degli identificatori di account non vengono diagnosticati e risolti, gli identificatori di account potrebbero esaurirsi nel dominio, dopo di che la creazione di account verrebbe disabilitata in modo permanente nel dominio.<p>Per ulteriori informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=228610.|  
|Note e risoluzione|Contattare tutti gli amministratori di dominio e informarli che non è possibile creare altre entità di sicurezza in questo dominio finché la protezione non viene sostituita. Per altre informazioni sulla sostituzione della protezione e sulla possibilità di incrementare il pool di RID generale, vedere [Sblocco della dimensione dello spazio del RID globale](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|ID evento|16658|  
|Origine|Directory-Services-SAM|  
|Severity|Avviso|  
|Message|Questo evento è un aggiornamento periodico sulla quantità totale rimanente di identificatori di account (RID) disponibili. Il numero di identificatori di account rimanenti è approssimativamente: %1.<p>Gli identificatori di account vengono utilizzati per la creazione di account e quando si esauriscono non è più possibile creare nuovi account nel dominio.<p>Per ulteriori informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=228745.|  
|Note e risoluzione|Contattare tutti gli amministratori di dominio e informarli che l'utilizzo del RID ha superato un'attività cardine principale. Determinare se questo comportamento sia previsto o meno esaminando gli schemi di creazione del dominio trusted di sicurezza. In realtà questo evento è del tutto insolito, in quanto significa che sono stati allocati circa 100 milioni di RID.|  
  
## <a name="see-also"></a>Vedi anche  
[Gestione del rilascio del RID in Windows Server 2012](https://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


