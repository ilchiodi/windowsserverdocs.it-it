---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Avanzate AD DS gestione utilizzando il centro di amministrazione di Active Directory (livello 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19e83ce0123bd484c61d5a4522ebdd90cd40241e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Avanzate AD DS gestione utilizzando il centro di amministrazione di Active Directory (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra il centro di amministrazione di Active Directory aggiornato con il nuovo Cestino per Active Directory, criteri granulari per le Password e Visualizzatore della cronologia di Windows PowerShell in modo più dettagliato, tra cui architettura, esempi per attività comuni, nonché informazioni sulla risoluzione. Per un'introduzione, vedere [Introduzione a Active Directory amministrativi miglioramenti apportati a centro & #40; Livello 100 & #41; ](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
-   [Architettura di centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
  
-   [Cestino abilitazione e gestione di Active Directory utilizzando il centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
  
-   [Configurazione e gestione dei criteri granulari per le Password utilizzando il centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
  
-   [Utilizzando il Visualizzatore della cronologia di PowerShell Windows centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
  
-   [Risoluzione dei problemi relativi AD DS gestione](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Architettura di centro di amministrazione di Active Directory  
  
### <a name="adprep-executables-dlls"></a>Eseguibili ADPrep, DLL  
Il modulo e l'architettura sottostante del centro di amministrazione di Active Directory non è stato modificato con il nuovo Cestino, Granulari e le funzionalità Visualizzatore della cronologia.  
  
-   Microsoft.ActiveDirectory.Management.UI.dll  
  
-   Microsoft.ActiveDirectory.Management.UI.resources.dll  
  
-   Microsoft.ActiveDirectory.Management.dll  
  
-   Microsoft.ActiveDirectory.Management.resources.dll  
  
-   ActiveDirectoryPowerShellResources.dll  
  
Windows PowerShell e livello delle operazioni per la nuova funzionalità Cestino sottostanti sono illustrati di seguito:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>Cestino abilitazione e gestione di Active Directory utilizzando il centro di amministrazione di Active Directory  
  
### <a name="capabilities"></a>Funzionalità di  
  
-   Windows Server 2012 Active Directory amministrativi Center consente di configurare e gestire il Cestino per Active Directory per qualsiasi partizione di dominio in una foresta. Non è più necessario usare Windows PowerShell o Ldp.exe per abilitare il Cestino per Active Directory o ripristinare gli oggetti in partizioni di dominio.  
  
-   Il centro di amministrazione di Active Directory ha avanzata Criteri di filtro, rendendo più facile ripristino mirato in ambienti di grandi dimensioni con molti oggetti eliminati intenzionalmente.  
  
### <a name="limitations"></a>Limitazioni  
  
-   Poiché il centro di amministrazione di Active Directory possono gestire solo partizioni di dominio, non è possibile ripristinare oggetti eliminati dalle partizioni di configurazione, DNS del dominio o DNS della foresta (non è possibile eliminare oggetti dalla partizione dello Schema). Per ripristinare gli oggetti da partizioni non di dominio, usare [Restore-ADObject](https://technet.microsoft.com/library/ee617262.aspx).  
  
-   Il centro di amministrazione di Active Directory è possibile ripristinare sottoalberi di oggetti in un'unica azione. Ad esempio, se si elimina un'unità Organizzativa con unità organizzative, utenti, gruppi e computer annidati, il ripristino di unità Organizzativa di base non ripristina gli oggetti figlio.  
  
    > [!NOTE]  
    > L'operazione di ripristino batch di centro di amministrazione di Active Directory viene "best effort" sorta di oggetti eliminati *all'interno della selezione solo* in modo padre siano ordinati prima di quelli figlio per l'elenco di ripristino. Nei casi semplici test, i sottoalberi di oggetti possono essere ripristinati in una singola azione. Ma casi estremi, ad esempio una selezione contenente alberi parziali - alberi con alcuni dei nodi padre eliminati mancante - o casi di errore, ad esempio ignorando gli oggetti figlio quando ripristino padre non riesce, potrebbe non funzionare come previsto. Per questo motivo, è consigliabile ripristinare sempre i sottoalberi di oggetti come azione separata dopo avere ripristinato gli oggetti padre.  
  
Cestino per Active Directory richiede un Windows Server 2008 R2 livello di funzionalità foresta ed è necessario essere un membro del gruppo Enterprise Admins. Una volta abilitata, è possibile disabilitare Cestino per Active Directory. Cestino per Active Directory aumenta la dimensione del database di Active Directory (NTDS. DIT) in ogni controller di dominio nella foresta. Spazio su disco utilizzato dal Cestino continua ad aumentare nel tempo quanto conserva gli oggetti e tutti i dati degli attributi.  
  
Per ulteriori informazioni, vedere [Active Directory Guida dettagliata al Cestino](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx) e [valutare i requisiti Hardware (per Cestino per Active Directory)](https://technet.microsoft.com/library/cc753439(WS.10).aspx).  
  
È possibile *inferiore* la foresta e dominio livelli di funzionalità da Windows Server 2012 a Windows Server 2008 R2, anche dopo aver abilitato il Cestino per Active Directory. È necessario usare il **Set-ADForestMode** e **Set-ADDomainMode** cmdlet di Active Directory.  
  
Per esempio:  
  
```  
Set-AdForestMode -identity corp.contoso.com -server dc1.corp.contoso.com -forestmode Windows2008R2Forest  
Set-AdDomainMode -identity research.corp.contoso.com -server dc3.research.corp.contoso.com -domainmode Windows2008R2Domain  
  
```  
  
È possibile utilizzare il centro di amministrazione di Active Directory per apportare questa modifica, solo per aumentare i livelli di funzionalità.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Abilitazione Cestino per Active Directory utilizzando il centro di amministrazione di Active Directory  
Per abilitare il Cestino per Active Directory, aprire il **centro di amministrazione di Active Directory** e fare clic sul nome della foresta nel riquadro di spostamento. Dal **attività** riquadro, fare clic su **Abilita Cestino**.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
Mostra il centro di amministrazione di Active Directory il **Abilita conferma Cestino** finestra di dialogo. Questa finestra di dialogo indica che l'abilitazione del Cestino è irreversibile. Fare clic su **OK** per abilitare il Cestino per Active Directory. Il centro di amministrazione di Active Directory Mostra un'altra finestra di dialogo per ricordarti che il Cestino per Active Directory non è completamente funzionale finché tutti i controller di dominio di replica della modifica di configurazione.  
  
> [!IMPORTANT]  
> L'opzione per abilitare il Cestino per Active Directory non è disponibile se:  
>   
> -   Il livello funzionale della foresta è inferiore a Windows Server 2008 R2  
> -   È già abilitato  
  
Il cmdlet di Windows PowerShell per Active Directory equivalente è ancora:  
  
```  
Enable-ADOptionalFeature  
```  
  
Per ulteriori informazioni sull'uso di Windows PowerShell per abilitare il Cestino per Active Directory, vedere il [Active Directory Guida dettagliata al Cestino](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Gestione Cestino per Active Directory utilizzando il centro di amministrazione di Active Directory  
Questa sezione viene usato l'esempio di un dominio esistente denominato **corp.contoso.com**. Questo dominio organizza gli utenti in un'unità Organizzativa padre denominata **UserAccounts**. Il **UserAccounts** OU contiene tre unità organizzative figlio denominate dal reparto, ciascuna delle quali contengono altre unità organizzative, utenti e gruppi.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Archiviazione e filtro  
Il Cestino per Active Directory conserva tutti gli oggetti eliminati nella foresta. Salva gli oggetti in base al **msDS-deletedObjectLifetime** attributo, che per impostazione predefinita è impostato per trovare la corrispondenza di **tombstoneLifetime** attributo dell'insieme di strutture. In ogni foresta creata con Windows Server 2003 SP1 o versione successiva, il valore di **tombstoneLifetime** è impostato su 180 giorni per impostazione predefinita. Nelle foreste aggiornate da Windows 2000 o installate con Windows Server 2003 (senza service pack), non è impostato l'attributo tombstoneLifetime predefinito e di conseguenza, Windows utilizza il valore predefinito interno di 60 giorni. Tutto questo è configurabile. È possibile utilizzare il centro di amministrazione di Active Directory per ripristinare gli oggetti eliminati dalle partizioni del dominio della foresta. È necessario continuare a usare il cmdlet **Restore-ADObject** per ripristinare oggetti eliminati da altre partizioni, ad esempio Configuration.Enabling il Cestino per Active Directory rende il **oggetti eliminati** contenitore visibili in ogni partizione di dominio nel centro di amministrazione di Active Directory.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
Il **oggetti eliminati** contenitore Mostra tutti gli oggetti ripristinabili in quella partizione del dominio. Più vecchi di oggetti eliminati **msDS-deletedObjectLifetime** sono noti come oggetti riciclati. Il centro di amministrazione di Active Directory non vengono visualizzati oggetti riciclati e non è possibile ripristinare questi oggetti tramite il centro di amministrazione di Active Directory.  
  
Per una spiegazione più approfondita dell'architettura del Cestino e delle regole di elaborazione, vedere [il Cestino per Active Directory: informazioni, implementazione, procedure consigliate e risoluzione dei problemi](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx).  
  
Il centro di amministrazione di Active Directory limita artificialmente il numero predefinito di oggetti restituiti da un contenitore a 20.000 oggetti. È possibile aumentare questo limite fino a 100.000 oggetti facendo il **Gestisci** menu, quindi **Opzioni elenco**.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Ripristino  
  
##### <a name="filtering"></a>Filtro  
Centro di amministrazione di Active Directory offre potenti criteri e opzioni di filtro che è necessario acquisire familiarità con prima di poterli utilizzare in un ripristino reale. Domini eliminano intenzionalmente molti oggetti tramite la loro durata. Con una durata oggetto probabilmente eliminato di 180 giorni, è possibile ripristinare tutti gli oggetti quando si verifica un incidente.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
Invece di scrivere filtri LDAP complessi e convertire valori UTC in date e ore, utilizzare la base e avanzate **filtro** menu per elencare solo gli oggetti pertinenti. Se si conosce il giorno dell'eliminazione, i nomi degli oggetti o altri dati chiave, utilizzare che a proprio vantaggio per filtrare i dati. Attiva o disattiva le opzioni di filtro avanzato facendo clic sulla freccia di espansione a destra della casella di ricerca.  
  
L'operazione di ripristino supporta tutte le standard criteri opzioni di filtro, lo stesso come qualsiasi altra ricerca. I filtri predefiniti, quelli importanti per il ripristino degli oggetti sono in genere:  
  
-   *ANR (risoluzione nome ambiguo, non è elencata nel menu ma usato quando si digita di * * * filtro * * * casella)*  
  
-   Ultima modificata compresa nel date specificato  
  
-   Oggetto è utente/inetorgperson/computer/gruppo/unità organizzativa  
  
-   Nome  
  
-   Quando eliminato  
  
-   Ultimo padre noto  
  
-   Tipo  
  
-   Descrizione  
  
-   Città  
  
-   Il paese  
  
-   Reparto  
  
-   ID dipendente  
  
-   Nome  
  
-   Posizione professionale  
  
-   Cognome  
  
-   SAMaccountname  
  
-   Stato o provincia  
  
-   Numero di telefono  
  
-   UPN  
  
-   CAP  
  
È possibile aggiungere più criteri. Ad esempio, è possibile trovare tutti gli oggetti utente eliminati in 24 settembre<sup>th</sup> 2012 da Chicago, Illinois, con una posizione di Manager.  
  
È inoltre possibile aggiungere, modificare o riordinare le intestazioni di colonna per visualizzare più dettagli durante la valutazione degli oggetti da recuperare.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Per ulteriori informazioni sulla risoluzione nome ambiguo, vedere [attributi ANR](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Oggetto singolo  
Ripristino degli oggetti eliminati è sempre stata un'unica operazione.  Il centro di amministrazione di Active Directory la semplifica ulteriormente. Per ripristinare un oggetto eliminato, ad esempio un singolo utente:  
  
1.  Fare clic sul nome di dominio nel riquadro di spostamento del centro di amministrazione di Active Directory.  
  
2.  Fare doppio clic su **oggetti eliminati** nell'elenco di gestione.  
  
3.  Fare doppio clic su oggetto e quindi fare clic su **ripristinare**, oppure fare clic su **ripristinare** dal **attività** riquadro.  
  
L'oggetto viene ripristinato nel percorso originale.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Fare clic su **ripristinare... ** per modificare il percorso di ripristino. Ciò è utile se è stato eliminato anche contenitore padre dell'oggetto eliminato, ma non si desidera ripristinare l'elemento padre.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Più oggetti Peer  
È possibile ripristinare più oggetti a livello di peer, ad esempio tutti gli utenti in un'unità Organizzativa. Tenere premuto il tasto CTRL e fare clic su uno o più oggetti eliminati da ripristinare. Fare clic su **ripristinare** dal riquadro attività. È inoltre possibile selezionare tutti gli oggetti visualizzati tenendo premuto i tasti CTRL e A, o un intervallo di oggetti MAIUSC e fare clic.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Più oggetti padre e figlio  
È fondamentale comprendere il processo di ripristino per un ripristino più padre e figlio, in quanto il centro di amministrazione di Active Directory è possibile ripristinare un albero annidato di oggetti eliminati con un'unica azione.  
  
1.  Ripristinare l'oggetto eliminato superiore in una struttura ad albero.  
  
2.  Ripristinare gli elementi figlio immediati di tale oggetto padre.  
  
3.  Ripristinare gli elementi figlio immediati di questi oggetti padre.  
  
4.  Ripetere l'operazione fino a quando non ripristinati tutti gli oggetti.  
  
È possibile ripristinare un oggetto figlio prima di ripristinare il relativo elemento padre. Questo tentativo di ripristino restituisce l'errore seguente:  
  
**L'operazione non può essere eseguita padre dell'oggetto è privo di istanza o eliminato.**  
  
Il **ultimo padre noto** attributo Mostra la relazione padre di ogni oggetto. Il **ultimo padre noto** Modifica attributo dal percorso eliminato al percorso ripristinato quando si aggiorna il centro di amministrazione di Active Directory dopo il ripristino di un elemento padre. Pertanto, è possibile ripristinare tale oggetto figlio quando il percorso dell'oggetto padre non viene più visualizzato il nome distinto del contenitore oggetti eliminati.  
  
Considerare lo scenario in cui un amministratore elimina accidentalmente l'unità Organizzativa Sales che contiene unità organizzative figlio e gli utenti.  
  
Osservare innanzitutto il valore del **ultimo padre noto** attributo per tutti gli utenti eliminati e la modalità di lettura * *OU = Sales\0ADEL:*< guid + contenitore oggetti eliminati il nome distinto >** *:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtrare il nome ambiguo Sales per restituire l'unità Organizzativa eliminata, che verrà quindi ripristinata:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Il centro di amministrazione di Active Directory per modificare il nome distinto dell'unità Organizzativa Sales ripristinato attributo ultimo padre noto dell'oggetto utente eliminato di aggiornamento:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Applicare filtri per tutti gli utenti Sales. Tieni premuti i tasti CTRL e A per selezionare tutti gli utenti Sales eliminati. Fare clic su **ripristinare** per spostare gli oggetti dal **oggetti eliminati** contenitore per l'unità Organizzativa Sales con le appartenenze ai gruppi e gli attributi intatti.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Se il **vendite** OU contiene unità organizzative figlio indipendenti, quindi ripristinare le unità organizzative figlio prima di ripristinare i relativi elementi figlio e così via.  
  
Per ripristinare gli oggetti eliminati tutti annidati specificando un contenitore padre eliminato, vedere [appendice b: ripristinare più oggetti Active Directory eliminati (Script di esempio)](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
Il cmdlet di Windows PowerShell per Active Directory per il ripristino degli oggetti eliminati è:  
  
```  
Restore-adobject  
  
```  
  
Il **Restore-ADObject** cmdlet funzionalità non è stata modificata tra Windows Server 2008 R2 e Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtro lato server  
È possibile che nel tempo, il contenitore oggetti eliminati verrà che si accumulino oltre 20.000 (o perfino 100.000) oggetti medie e grandi imprese e che risulti difficile visualizzarli tutti gli oggetti. Poiché il meccanismo di filtro nel centro di amministrazione di Active Directory si basa sul filtro lato client, non è possibile visualizzare questi oggetti aggiuntivi. Per ovviare a questa limitazione, utilizzare la procedura seguente per eseguire una ricerca sul lato server:  
  
1.  Fare clic il **oggetti eliminati** contenitore e fare clic su **Cerca in questo nodo**.  
  
2.  Fare clic sulla freccia di espansione per esporre il **+ Aggiungi criteri** menu, selezionare e aggiungere **ultima modifica compresa nel specificato date**. L'ora dell'ultima modifica (la **whenChanged** attributo) rappresenta un'approssimazione dell'ora di modifica; Nella maggior parte degli ambienti, sono identici. Questa query esegue una ricerca sul lato server.  
  
3.  Individuare gli oggetti eliminati da ripristinare utilizzando ulteriore visualizzazione filtro, ordinamento, e così via nei risultati della e quindi ripristinarli normalmente.  
  
## <a name="BKMK_FGPP"></a>Configurazione e gestione dei criteri granulari per le Password utilizzando il centro di amministrazione di Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configurazione dei criteri granulari per le Password  
Il centro di amministrazione di Active Directory consente di creare e gestire gli oggetti Criteri di Password specifici (Granulari). Windows Server 2008 è stata introdotta la funzionalità Granulari ma Windows Server 2012 è la prima interfaccia di gestione grafica per tale. Applicare i criteri granulari per le Password a livello di dominio e consentono di sostituire la singola password di dominio richiesta da Windows Server 2003. La creazione di diversi Granulari con diverse impostazioni di singoli utenti o gruppi ottengono diversi criteri password in un dominio.  
  
Per informazioni sulla gestione delle Password, vedere [AD DS granulari per le Password e criterio di blocco Account Step-by-Step Guida (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
Nel riquadro di spostamento, fare clic su visualizzazione albero, fare clic con il dominio, fare clic su **sistema**, fare clic su **contenitore Impostazioni Password**e quindi nel riquadro attività fare clic su **New** e **impostazioni Password**.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Gestione dei criteri granulari per le Password  
Creazione di un nuovo FGPP o la modifica di uno esistente, viene visualizzata la **impostazioni Password** editor. Da qui configurare tutti i criteri password desiderata, come in Windows Server 2008 o Windows Server 2008 R2, ora nell'apposito editor.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Compilare i campi (asterisco rosso) tutte le necessarie e i campi facoltativi, quindi fare clic su **Aggiungi** per impostare gli utenti o gruppi di destinatari di questo criterio. Granulari esegue l'override di impostazioni di criteri di dominio predefinito per le entità di sicurezza specificato. Nella figura precedente, un criterio estremamente restrittivo viene applicato solo per l'account Administrator predefinito, per impedirne la compromissione. Il criterio è troppo complesso per gli utenti standard di rispettare, ma è perfetto per un account ad alto rischio usato esclusivamente dai professionisti IT.  
  
Anche impostare le precedenze e a quali utenti e gruppi il criterio si applica all'interno di un dominio specifico.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
I cmdlet di Windows PowerShell per Active Directory per criteri Password granulari sono:  
  
```  
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
  
```  
  
Granulari per le funzionalità del cmdlet criteri Password non è stato modificato tra il server Windows Server 2008 R2 e Windows Server 2012. Per comodità, il diagramma seguente illustra gli argomenti associati per i cmdlet:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Il centro di amministrazione di Active Directory consente inoltre di individuare il set risultante dei Granulari applicate per un utente specifico. Fare clic con il pulsante destro qualsiasi utente e fare clic su **Visualizza impostazioni password risultanti... ** per aprire il *impostazioni Password* pagina che si applica all'utente tramite assegnazione implicita o esplicita:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
Esaminare il **proprietà** di qualsiasi utente vengono mostrate le **impostazioni Password associate direttamente**, quali sono i FGPPs assegnate in modo esplicito:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
Assegnati in modo implicito Granulari non viene visualizzato qui. a tal fine, è necessario utilizzare il **Visualizza impostazioni password risultanti... ** opzione.  
  
## <a name="BKMK_HistoryViewer"></a>Utilizzando il Visualizzatore della cronologia di PowerShell Windows centro di amministrazione di Active Directory  
Il futuro della gestione di Windows è Windows PowerShell. Per sovrapposizione di strumenti grafici su un framework di automazione di attività, gestione dei sistemi distribuiti più complessi diventa coerente ed efficiente. È necessario comprendere il funzionamento di Windows PowerShell per raggiungere le potenzialità e ottimizzare gli investimenti di elaborazione.  
  
Il centro di amministrazione di Active Directory offre ora una cronologia completa di tutti i cmdlet di Windows PowerShell che viene eseguita e i relativi argomenti e valori. È possibile copiare la cronologia dei cmdlet altrove per esaminarli, modificarli e riutilizzare. È possibile creare note attività per isolare i quali il centro di amministrazione Active Directory risultati dei comandi in Windows PowerShell. È anche possibile filtrare la cronologia per trovare punti di interesse.  
  
Di Visualizzatore della cronologia PowerShell Windows centro amministrativi Directory Active è utente di imparare attraverso l'esperienza pratica.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Fare clic sulla freccia di espansione (freccia) per visualizzare il Visualizzatore della cronologia di Windows PowerShell.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Quindi, creare un utente o modificare l'appartenenza a un gruppo. Il Visualizzatore della cronologia viene aggiornato continuamente con una visualizzazione compressa di ogni cmdlet il centro di amministrazione di Active Directory è stato eseguito con gli argomenti specificati.  
  
Espandere le righe a cui interessati per visualizzare tutti i valori forniti agli argomenti del cmdlet:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Fare clic su di **Avvia attività** menu per creare una notazione manuale prima di utilizzare il centro di amministrazione di Active Directory per creare, modificare o eliminare un oggetto. Digitare le operazioni.  Al termine della modifica, selezionare **Termina attività**. La nota dell'attività raggruppa tutte le operazioni eseguite in una nota comprimibile che è possibile utilizzare per una migliore comprensione.  
  
Ad esempio, per visualizzare i comandi di Windows PowerShell consente di modificare la password dell'utente e rimuovere quest'ultimo da un gruppo:  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
Selezionando la casella di controllo Mostra tutto Mostra anche Get-* verbo cmdlet Windows PowerShell che recuperano solo i dati.  
  
![Avanzate AD DS gestione](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
Il Visualizzatore della cronologia mostra il valore letterale, i comandi eseguiti dal centro di amministrazione di Active Directory ed è possibile osservare che alcuni cmdlet sembra eseguire inutilmente. Ad esempio, è possibile creare un nuovo utente con:  
  
```  
new-aduser   
```  
  
e non è necessario utilizzare:  
  
```  
set-adaccountpassword  
enable-adaccount  
set-aduser  
  
```  
  
Progettazione del centro amministrativo di Active Directory richiesto modularità e quantità minima di codice. Di conseguenza, invece di un set di funzioni per la creazione di nuovi utenti e un altro set per modificare gli utenti esistenti, esegue ogni funzione e concatena con i cmdlet. Tienilo a mente durante l'apprendimento di Windows PowerShell per Active Directory. È possibile anche utilizzare che come una tecnica di apprendimento in cui viene visualizzato come è possibile utilizzare Windows PowerShell per completare un'attività.  
  
## <a name="BKMK_Tshoot"></a>Risoluzione dei problemi relativi AD DS gestione  
  
### <a name="introduction-to-troubleshooting"></a>Introduzione alla risoluzione dei problemi  
A causa il relativo introdotto di recente e l'utilizzo in ambienti dei clienti esistenti, il centro di amministrazione di Active Directory è limitato di opzioni di risoluzione dei problemi.  
  
### <a name="troubleshooting-options"></a>Opzioni di risoluzione dei problemi  
  
#### <a name="logging-options"></a>Opzioni di registrazione  
Il centro di amministrazione di Active Directory contiene ora la registrazione incorporata in Windows Server 2012, come parte di un file di configurazione di traccia. Creazione e modifica il file seguente nella stessa cartella dsac.exe:  
  
**dsac.exe.config**  
  
Creare il contenuto seguente:  
  
```  
<appSettings>  
  <add key="DsacLogLevel" value="Verbose" />  
</appSettings>  
<system.diagnostics>   
 <trace autoflush="false" indentsize="4">   
  <listeners>   
   <add name="myListener"   
    type="System.Diagnostics.TextWriterTraceListener"   
    initializeData="dsac.trace.log" />   
   <remove name="Default" />   
  </listeners>   
 </trace>   
</system.diagnostics>  
  
```  
  
I livelli di dettaglio per **DsacLogLevel** sono **Nessuno**, **errore**, **avviso**, **Info**, e **Verbose**. Il nome del file di output è configurabile e scrive nella stessa cartella dsac.exe. Per maggiori informazioni sul funzionamento di adac, quali controller di dominio contattati, quali comandi di Windows PowerShell eseguiti, quali sono le risposte e ulteriori dettagli, può indicare l'output.  
  
Quando si utilizza il livello di informazioni, ad esempio, restituisce tutti i risultati ad eccezione del livello di dettaglio livello di traccia:  
  
-   Avvio di DSAC.exe  
  
-   Avvio della registrazione  
  
-   Controller di dominio richiesto di restituire informazioni sul dominio iniziali  
  
    ```  
    [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
    [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
    ```  
  
-   Controller di dominio DC1 restituito dal dominio Corp  
  
-   PS Unità virtuale Active Directory caricata  
  
    ```  
    [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
    [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
  
    ```  
  
-   Ottenere informazioni sull'attributo DSE radice di dominio  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
  
    ```  
  
-   Ottenere informazioni sul Cestino di Active Directory del dominio  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50  
  
    ```  
  
-   Ottenere una foresta di Active Directory  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADForest  
    -Identity:"corp.contoso.com"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Ottenere informazioni sullo Schema per tipi di crittografia supportati, Granulari, determinate informazioni utente  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"  
    -Properties:lDAPDisplayName  
    -ResultPageSize:"100"  
    -ResultSetSize:$null  
    -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"  
    -SearchScope:"OneLevel"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7  
    [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Ottenere tutte le informazioni sull'oggetto dominio per mostrarle all'amministratore che ha fatto clic sul vertice del dominio.  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -IncludeDeletedObjects:$false  
    -LDAPFilter:"(objectClass=*)"  
    -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence  
    -ResultPageSize:"100"  
    -ResultSetSize:"20201"  
    -SearchBase:"DC=corp,DC=contoso,DC=com"  
    -SearchScope:"Base"  
    -Server:"dc1.corp.contoso.com"  
  
    ```  
  
L'impostazione livello di Verbose Mostra anche gli stack .NET per ogni funzione, ma queste non includono dati sufficienti per essere particolarmente utile, ad eccezione di quando la risoluzione dei problemi di Dsac.exe subire un arresto anomalo o violazione di accesso.  
  
> [!IMPORTANT]  
> È inoltre disponibile una versione fuori banda del servizio chiamato il [Active Directory Management Gateway](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), che viene eseguito in Windows Server 2008 SP2 e Windows Server 2003 SP2.  
  
Le due cause probabili di questo problema sono:  
  
-   Il servizio servizi Web Active Directory non è in esecuzione sui controller di dominio accessibile.  
  
-   Le comunicazioni di rete vengono bloccate per il servizio servizi Web Active Directory nel computer che esegue il centro di amministrazione di Active Directory  
  
Gli errori visualizzati quando non le istanze di servizi Web Active Directory sono disponibili sono:  
  
|||  
|-|-|  
|Errore|Operazione|  
|"Impossibile connettersi a un dominio. Aggiornare o riprovare quando sarà disponibile connessione"|Visualizzato all'avvio dell'applicazione centro di amministrazione di Active Directory|  
|"Impossibile trovare un server disponibile nel * <NetBIOS domain name> * dominio che esegue il servizio di Web Active Directory (ADWS)"|Visualizzato quando si tenta di selezionare un nodo di dominio nell'applicazione centro di amministrazione di Active Directory|  
  
Per risolvere questo problema, utilizzare questi passaggi:  
  
1.  Convalida Web servizi di Active Directory viene avviato in almeno un controller di dominio nel dominio (e preferibilmente tutti i controller di dominio nella foresta). Assicurarsi che sia impostata per l'avvio automatico in anche tutti i controller di dominio.  
  
2.  Nel computer che esegue il centro di amministrazione di Active Directory, verificare che sia possibile individuare un server che esegue servizi Web Active Directory eseguendo questi comandi NLTest.exe:  
  
    ```  
    nltest /dsgetdc:<domain NetBIOS name> /ws /force   
    nltest /dsgetdc:<domain fully qualified DNS name> /ws /force  
  
    ```  
  
    Se i test hanno esito negativo anche se è in esecuzione il servizio servizi Web Active Directory, il problema riguarda la risoluzione dei nomi o LDAP e non servizi Web Active Directory o il centro di amministrazione di Active Directory. Questo test ha esito negativo con errore "1355 0x54B ERROR_NO_SUCH_DOMAIN" se servizi Web Active Directory non è in esecuzione sui controller di dominio però, quindi verificare di nuovo prima di trarre conclusioni.  
  
3.  Nel controller di dominio restituito da NLTest, scarica l'elenco delle porte di ascolto con il comando:  
  
    ```  
    Netstat -anob > ports.txt  
  
    ```  
  
    Esaminare il file ports.txt e verificare che il servizio servizi Web Active Directory è in ascolto sulla porta 9389. Esempio:  
  
    ```  
    TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    TCP    [::]:9389       [::]:0       LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    ```  
  
    Se è in ascolto, verificare le regole del Firewall di Windows e assicurarsi che sia consentito TCP 9389 in entrata. Per impostazione predefinita, i controller di dominio Abilita regola del firewall "Servizi Web Active Directory (TCP-in)". Se non è in ascolto, verificare di nuovo che il servizio sia in esecuzione su questo server e riavviarla. Verificare che nessun altro processo è già in ascolto sulla porta 9389.  
  
4.  Installare NetMon o un'altra utilità di acquisizione di rete del computer che esegue il centro di amministrazione di Active Directory e nel controller di dominio restituito da NLTEST. Raccogliere acquisizioni di rete simultanee da entrambi i computer in cui avviare centro di amministrazione di Active Directory e visualizzato l'errore prima di arrestare le acquisizioni. Verificare che il client sia in grado di inviare e ricevere dal controller di dominio sulla porta TCP 9389. Se i pacchetti vengono inviati ma non vengono recapitati o arrivano e il controller di dominio risponde ma non raggiungono mai il client, è probabile che sia presente un firewall tra i computer nella rete ignori i pacchetti su quella porta. Questo firewall potrebbe essere software o hardware e può essere parte del software (antivirus) di terze parti endpoint protection.  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problemi noti e probabili e scenari di supporto  
Il problema probabilmente solo con il centro di amministrazione di Active Directory è l'impossibilità di connettersi per il Web Active Directory Service (ADWS) in esecuzione su un controller di dominio Windows Server 2012 o Windows Server 2008 R2.  
  
## <a name="see-also"></a>Vedere anche  
[Introduzione ai miglioramenti di centro di amministrazione di Active Directory & #40; Livello 100 & #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
  

