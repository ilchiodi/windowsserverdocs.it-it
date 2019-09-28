---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Advanced AD DS Management Using Active Directory Administrative Center (Level 200)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 00e307da35911189114257eea88ccaf90ceab1ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390720"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Advanced AD DS Management Using Active Directory Administrative Center (Level 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato il Centro di amministrazione di Active Directory aggiornato con una descrizione dettagliata delle nuove funzionalità, ad esempio il Cestino di Active Directory, i criteri granulari per le password e il Visualizzatore della cronologia di Windows PowerShell, e vengono forniti esempi per attività comuni, oltre a informazioni sull'architettura e sulla risoluzione dei problemi. Per un'introduzione, vedere [Introduzione al &#40;livello di miglioramento di centro di amministrazione di Active Directory&#41;100](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
- [Architettura Centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
- [Abilitazione e gestione del Cestino Active Directory tramite Centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
- [Configurazione e gestione dei criteri granulari per le password con Centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
- [Uso del Visualizzatore della cronologia di Windows PowerShell Centro di amministrazione di Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
- [Risoluzione dei problemi di gestione di servizi di dominio Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Architettura Centro di amministrazione di Active Directory  
  
### <a name="active-directory-administrative-center-executables-dlls"></a>File eseguibili di Centro di amministrazione di Active Directory, dll  

Il modulo e l'architettura sottostante del Centro di amministrazione di Active Directory sono rimasti invariati anche in seguito all'introduzione delle funzionalità del nuovo cestino, dei criteri granulari per le password e del visualizzatore della cronologia.  
  
- Microsoft.ActiveDirectory.Management.UI.dll  
- Microsoft.ActiveDirectory.Management.UI.resources.dll  
- Microsoft.ActiveDirectory.Management.dll  
- Microsoft.ActiveDirectory.Management.resources.dll  
- ActiveDirectoryPowerShellResources.dll  
  
L'architettura sottostante di Windows PowerShell e il livello delle operazioni per il nuovo Cestino sono illustrati di seguito:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>Abilitazione e gestione del Cestino Active Directory tramite Centro di amministrazione di Active Directory  
  
### <a name="capabilities"></a>Funzionalità  
  
- Il Centro di amministrazione di Active Directory Windows Server 2012 o versioni successive consente di configurare e gestire il Cestino Active Directory per qualsiasi partizione di dominio in una foresta. Non è più necessario usare Windows PowerShell o Ldp.exe per abilitare il Cestino di Active Directory o ripristinare gli oggetti in partizioni di dominio.
- Il Centro di Amministrazione di Active Directory offre criteri di filtro avanzati per semplificare il ripristino mirato in ambienti di grandi dimensioni con molti oggetti eliminati intenzionalmente.
  
### <a name="limitations"></a>Limitazioni  
  
- Poiché il Centro di amministrazione di Active Directory è in grado di gestire solo partizioni di dominio, non è possibile ripristinare oggetti eliminati dalle partizioni di configurazione, DNS del dominio o DNS della foresta (non è possibile eliminare oggetti dalla partizione dello schema). Per ripristinare gli oggetti da partizioni non di dominio, usare [Restore-Active DirectoryObject](https://technet.microsoft.com/library/ee617262.aspx).  

- Nel Centro di amministrazione di Active Directory non è possibile ripristinare sottoalberi di oggetti in un'unica operazione. Se ad esempio si elimina un'unità organizzativa con unità amministrative, utenti, gruppi e computer annidati, il ripristino dell'unità organizzativa di base non ripristina gli oggetti figlio.  
  
    > [!NOTE]  
    > L'operazione di ripristino batch Centro di amministrazione di Active Directory esegue un tipo di "massimo sforzo" degli oggetti eliminati *all'interno della selezione, in* modo che i padri vengano ordinati prima degli elementi figlio per l'elenco di ripristino. Nei casi di test più semplici, potrebbe essere possibile ripristinare i sottoalberi di oggetti in un'unica operazione. Tuttavia, i casi d'angolo, ad esempio una selezione contenente alberi ad albero parziali in cui mancano alcuni dei nodi padre eliminati, o i casi di errore, ad esempio ignorando gli oggetti figlio quando il ripristino padre ha esito negativo, potrebbero non funzionare come previsto. Per questo motivo, è consigliabile ripristinare sempre i sottoalberi di oggetti con un'operazione separata dopo avere ripristinato gli oggetti padre.  
  
Il cestino del Active Directory richiede un livello di funzionalità della foresta di Windows Server 2008 R2 ed è necessario essere un membro del gruppo Enterprise Admins. Se si abilita il Cestino di Active Directory nell'ambiente in uso, non sarà più possibile disabilitarlo. Il Cestino di Active Directory comporta un aumento delle dimensioni del database di Active Directory (NTDS.DIT) in ogni controller di dominio della foresta. Lo spazio su disco usato dal cestino continua ad aumentare nel tempo, in quanto conserva gli oggetti e tutti i dati degli attributi.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Abilitazione del Cestino di Active Directory dal Centro di amministrazione di Active Directory

Per abilitare il Cestino di Active Directory, aprire il **Centro di amministrazione di Active Directory** e fare clic sul nome della foresta nel pannello di navigazione. Nel riquadro **Attività** fare clic su **Abilita Cestino**.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
Verrà visualizzata la finestra di dialogo **Abilita conferma Cestino** del Centro di amministrazione di Active Directory. La finestra di dialogo indica che l'abilitazione del cestino è irreversibile. Fare clic su **OK** per abilitare il Cestino di Active Directory. Nel Centro di amministrazione di Active Directory verrà visualizzata un'altra finestra di dialogo che ricorda all'utente che il Cestino di Active Directory non sarà pienamente funzionale fintanto che tutti i controller di dominio non replicano la modifica alla configurazione.  
  
> [!IMPORTANT]  
> L'opzione per l'abilitazione del Cestino di Active Directory non è disponibile se:  
>
> - Il livello di funzionalità della foresta è inferiore a Windows Server 2008 R2.  
> - È già abilitato.  

Il cmdlet Active Directory Windows PowerShell equivalente è:  

```powershell
Enable-ADOptionalFeature  
```

Per altre informazioni sull'uso di Windows PowerShell per abilitare il Cestino di Active Directory, vedere [Guida dettagliata al Cestino per Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#active-directory-recycle-bin-step-by-step).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Gestione del Cestino di Active Directory dal Centro di amministrazione di Active Directory

In questa sezione viene usato l'esempio relativo a un dominio esistente denominato **corp.contoso.com**. Il dominio organizza gli utenti in un'unità organizzativa padre denominata **UserAccounts**. L'unità organizzativa **UserAccounts** contiene tre unità organizzative figlio denominate in base al reparto e ognuna di esse contiene altre unità organizzative, utenti e gruppi.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Archiviazione e filtro

Il Cestino di Active Directory conserva tutti gli oggetti eliminati nella foresta. Salva gli oggetti in base all'attributo **msDS-deletedObjectLifetime**, che per impostazione predefinita corrisponde all'attributo **tombstoneLifetime** della foresta. In ogni foresta creata con Windows Server 2003 SP1 o versioni successive, il valore di **tombstoneLifetime** è pari a 180 giorni per impostazione predefinita. Nelle foreste aggiornate da Windows 2000 o installate con Windows Server 2003 (senza Service Pack) l'attributo tombstoneLifetime predefinito NON È IMPOSTATO e pertanto viene usato il valore predefinito interno di Windows di 60 giorni. Queste impostazioni sono configurabili. È possibile usare il Centro di amministrazione di Active Directory per ripristinare gli oggetti eliminati dalle partizioni del dominio della foresta. È necessario continuare a usare il cmdlet **Restore-ADObject** per ripristinare gli oggetti eliminati da altre partizioni, ad esempio la partizione di configurazione. L'abilitazione del Cestino di Active Directory rende visibile il contenitore **Oggetti eliminati** in ogni partizione del dominio nel Centro di amministrazione di Active Directory.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
Nel contenitore **Oggetti eliminati** sono visualizzati tutti gli oggetti che si possono ripristinare in quella partizione del dominio. Gli oggetti eliminati più vecchi di **msDS-deletedObjectLifetime** sono noti come oggetti riciclati. Gli oggetti riciclati non sono visualizzati nel Centro di amministrazione di Active Directory e non è possibile ripristinare questi oggetti tramite il Centro di amministrazione di Active Directory.  
  
Per una spiegazione più approfondita dell'architettura del cestino e delle regole di elaborazione, vedere il cestino di Active Directory [The: Informazioni, implementazione, procedure consigliate e risoluzione dei problemi @ no__t-0.  
  
Il Centro di amministrazione di Active Directory limita artificialmente il numero predefinito di oggetti restituito da un contenitore a 20.000 oggetti. Per aumentare questo limite fino a 100.000 oggetti, fare clic sul menu **Gestisci** e quindi su **Opzioni elenco elementi da gestire**.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Ripristino  
  
##### <a name="filtering"></a>Filtro

Il Centro di amministrazione di Active Directory offre potenti criteri e opzioni di filtro con i quali è necessario acquisire familiarità prima di usarli per un ripristino reale. Nel relativo ciclo di vita, i domini eliminano intenzionalmente molti oggetti. Con una probabile durata dell'oggetto eliminato di 180 giorni, non è possibile ripristinare tutti gli oggetti quando si verifica un incidente.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
Invece di scrivere filtri LDAP complessi e convertire valori UTC in date e ore, è possibile usare il menu **Filtro** con filtri di base e avanzati per visualizzare solo gli oggetti pertinenti. Se si conosce il giorno dell'eliminazione, i nomi degli oggetti o altri dati chiave, è consigliabile usarli per filtrare i dati. Attivare le opzioni relative al filtro avanzato facendo clic sulla freccia di espansione a destra della casella di ricerca.  
  
L'operazione di ripristino supporta tutte le opzioni dei criteri di filtro standard, come qualsiasi altra ricerca. In genere, i filtri incorporati importanti per il ripristino di oggetti sono:  
  
- *ANR (risoluzione del nome ambiguo: non elencato nel menu, ma ciò che viene usato quando si digita nella casella * * * * Filter * * * *)*  
- Ultima modifica compresa nell'intervallo di date specificato  
- L'oggetto è Utente/inetOrgPerson/Computer/Gruppo/Unità organizzativa  
- Nome  
- Data di eliminazione  
- Ultimo padre noto  
- Type  
- Descrizione  
- City  
- Paese/area geografica  
- department  
- ID dipendente  
- Nome  
- Posizione  
- Cognome  
- SamAccountName  
- Provincia  
- Numero di telefono  
- UPN  
- CAP  

È possibile aggiungere più criteri. Ad esempio, è possibile trovare tutti gli oggetti utente eliminati il 24 settembre 2012 da Chicago, Illinois con un titolo di lavoro manager.
  
È anche possibile aggiungere, modificare o riordinare le intestazioni di colonna per visualizzare più dettagli durante la valutazione degli oggetti da recuperare.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Per altre informazioni su Risoluzione nome ambiguo, vedere [Attributi ANR](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Oggetto singolo

Il ripristino di oggetti eliminati può essere da sempre eseguito con una singola operazione  e il Centro di amministrazione di Active Directory la semplifica ulteriormente. Per ripristinare un oggetto eliminato, ad esempio un singolo utente:  
  
1. Nel riquadro di spostamento del Centro di amministrazione di Active Directory fare clic sul nome di dominio.  
2. Fare doppio clic su **Oggetti eliminati** nell'elenco degli elementi da gestire.  
3. Fare clic con il pulsante destro del mouse sull'oggetto e quindi scegliere **Ripristina**oppure fare clic su **Ripristina** dal riquadro **Attività** .  
  
L'oggetto viene ripristinato nel percorso originale.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Fare clic su **Ripristina in...** per modificare il percorso di ripristino. Questa operazione è utile se è stato eliminato anche il contenitore padre dell'oggetto eliminato, ma non si desidera ripristinare l'elemento padre.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Più oggetti peer

È possibile ripristinare più oggetti a livello di peer, ad esempio tutti gli utenti in un'unità organizzativa. Tenere premuto il tasto CTRL e fare clic su uno o più oggetti eliminati da ripristinare. Fare clic su **Ripristina** dal riquadro Attività. Per selezionare tutti gli oggetti visualizzati, tenere premuti i tasti CTRL e A. Per selezionare un intervallo di oggetti tenere premuto il tasto MAIUSC e fare clic.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Più oggetti padre e figlio

È importante comprendere il processo di ripristino per un ripristino con più oggetti padre e figlio, in quanto il Centro di amministrazione di Active Directory non consente di ripristinare un albero annidato di oggetti eliminati con un'unica operazione.  
  
1. Ripristinare il primo oggetto eliminato in un albero.  
2. Ripristinare gli elementi figlio immediati di quell'oggetto padre.  
3. Ripristinare gli elementi figlio immediati di questi oggetti padre.  
4. Ripetere l'operazione fino a quando non si sono ripristinati tutti gli oggetti.  
  
Non è possibile ripristinare un oggetto figlio prima di ripristinare il relativo oggetto padre. Questo tentativo di ripristino restituisce l'errore seguente:  
  
**Il padre dell'oggetto è privo di istanza o è stato eliminato.**  
  
L'attributo **Ultimo padre noto** mostra la relazione padre di ogni oggetto. L'attributo **Ultimo padre noto** passa dal percorso eliminato al percorso ripristinato dopo aver aggiornato il Centro di amministrazione di Active Directory a seguito del ripristino di un oggetto padre. Pertanto, è possibile ripristinare l'oggetto figlio quando il percorso di un oggetto padre non Visualizza più il nome distinto del contenitore degli oggetti eliminati.  
  
Di seguito viene presentato uno scenario in cui un amministratore elimina accidentalmente l'unità organizzativa Sales che contiene unità organizzative e utenti figlio.  
  
Osservare innanzitutto il valore dell'ultimo attributo **padre noto** per tutti gli utenti eliminati e il modo in cui legge **ou = Sales\0ADEL:* < GUID + oggetti eliminati nome distinto del contenitore > * * *:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtrare in base al nome ambiguo Sales per visualizzare l'unità organizzativa eliminata, che verrà quindi ripristinata:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Aggiornare il Centro di amministrazione di Active Directory per vedere la modifica dell'ultimo attributo padre noto dell'oggetto utente eliminato nel nome distinto dell'unità organizzativa Sales ripristinata:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtrare in base a tutti gli utenti Sales. Tenere premuti i tasti CTRL e A per selezionare tutti gli utenti Sales eliminati. Fare clic su **Ripristina** per spostare gli oggetti dal contenitore **Oggetti eliminati** all'unità organizzativa Sales con le appartenenze ai gruppi e gli attributi intatti.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Se l'unità organizzativa **Sales** contiene unità organizzative figlio indipendenti, è necessario ripristinare le unità organizzative figlio prima di ripristinare i relativi oggetti figlio e così via.  
  
Per ripristinare tutti gli oggetti eliminati annidati specificando un contenitore padre eliminato, vedere [Appendix B: Ripristinare più oggetti Active Directory eliminati (script di esempio) ](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
Il cmdlet di Windows PowerShell per Active Directory per il ripristino degli oggetti eliminati è:  

```powershell
Restore-adobject  
```

La funzionalità del cmdlet **Restore-ADObject** non è stata modificata nel passaggio da Windows Server 2008 R2 a Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtro lato server

Nel tempo è possibile che nel contenitore Oggetti eliminati di medie e grandi imprese si accumulino oltre 20.000 (o perfino 100.000) oggetti e che risulti difficile visualizzarli tutti. Poiché il meccanismo di filtro nel Centro di amministrazione di Active Directory si basa sul filtro lato client, non è possibile visualizzare questi oggetti aggiuntivi. Per aggirare questa limitazione, è possibile eseguire una ricerca sul lato server nel modo seguente:  
  
1. Fare clic con il pulsante destro del mouse sul contenitore **Oggetti eliminati** e scegliere **Cerca in questo nodo**.  
2. Fare clic sulla freccia di espansione per visualizzare il menu **+Aggiungi criteri**, quindi selezionare e aggiungere **Ultima modifica compresa nell'intervallo di date specificato**. L'ora dell'ultima modifica (l'attributo **whenChanged**) rappresenta un'approssimazione dell'ora di modifica e, nella maggior parte degli ambienti, questi valori sono identici. Questa query consente di eseguire una ricerca sul lato server.  
3. Individuare gli oggetti eliminati da ripristinare applicando altre opzioni di visualizzazione, filtro e ordinamento ai risultati e quindi ripristinarli normalmente.  
  
## <a name="BKMK_FGPP"></a>Configurazione e gestione dei criteri granulari per le password con Centro di amministrazione di Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configurazione dei criteri granulari per le password

Nel Centro di amministrazione di Active Directory è possibile creare e gestire gli oggetti Criteri granulari per le password. In Windows Server 2008 è stata introdotta la funzionalità Criteri granulari per le password ma solo in Windows Server 2012 è stata aggiunta la prima interfaccia di gestione grafica per gestirla. I criteri granulari per le password si applicano a livello del dominio e consentono di sostituire la singola password di dominio richiesta da Windows Server 2003. La creazione di diversi criteri granulari per le password con impostazioni differenti consente di assegnare a singoli utenti o gruppi criteri per le password diversi in un dominio.  
  
Per altre informazioni sulla gestione delle password, vedere [Guida dettagliata alla configurazione dei criteri specifici per le password e il blocco degli account per utenti di Servizi di dominio Active Directory (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
Nel riquadro di spostamento fare clic su Visualizzazione albero, selezionare il dominio e fare clic su **Sistema**, su **Contenitore Impostazioni password**. Nel riquadro Attività fare clic su **Nuovo** e infine su **Impostazioni password**.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Gestione dei criteri granulari per le password

Durante la creazione di un nuovo criterio granulare per le password o la modifica di uno esistente, viene visualizzato l'editor **Impostazioni password** . Da qui è possibile configurare tutti i criteri per le password desiderati esattamente come in Windows Server 2008 o in Windows Server 2008 R2, solo che ora questa operazione viene eseguita nell'apposito editor.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Compilare tutti i campi obbligatori (contrassegnati con un asterisco rosso) e i campi facoltativi, quindi fare clic su **Aggiungi** per impostare gli utenti o i gruppi destinatari di questo criterio. I criteri granulari per le password sostituiscono le impostazioni dei criteri di dominio predefinite per le entità di sicurezza specificate. Nella figura precedente, un criterio estremamente restrittivo viene applicato solo all'account predefinito Administrator, per impedirne la compromissione. Il criterio è troppo complesso per essere applicato a utenti standard, ma è perfetto per un account ad alto rischio usato esclusivamente dai professionisti IT.  
  
È inoltre possibile impostare le precedenze e definire gli utenti e i gruppi a cui verrà applicato il criterio all'interno di un determinato dominio.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
I cmdlet di Windows PowerShell per Active Directory per i criteri granulari per le password sono:  
  
```powershell
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
```

La funzionalità del cmdlet per i criteri granulari per le password non è stata modificata nel passaggio da Windows Server 2008 R2 a Windows Server 2012. Per maggiore semplicità, il diagramma seguente illustra gli argomenti associati per i cmdlet:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Il Centro di amministrazione di Active Directory consente di individuare il set risultante dei criteri granulari per le password applicati per un utente specifico. Fare clic con il pulsante destro del mouse su un utente e scegliere **Visualizza impostazioni password risultanti...** per aprire la pagina *Impostazioni password* che si applica a tale utente tramite assegnazione implicita o esplicita:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
Nelle **Proprietà** di ogni utente vengono mostrate le **Impostazioni password associate direttamente**, che sono i criteri granulari per le password assegnati in modo esplicito:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
L'assegnazione FGPP implicita non viene visualizzata qui. per questo motivo, è necessario usare l'opzione **Visualizza impostazioni password risultanti** ....  
  
## <a name="BKMK_HistoryViewer"></a>Uso del Visualizzatore della cronologia di Windows PowerShell Centro di amministrazione di Active Directory

Windows PowerShell rappresenta la gestione di Windows del futuro. La sovrapposizione di strumenti grafici su un framework di automazione delle attività garantisce una gestione coerente ed efficiente dei sistemi distribuiti più complessi. Per sfruttare il potenziale offerto da Windows PowerShell e ottimizzare gli investimenti IT, è necessario comprenderne il funzionamento.  
  
Il Centro di amministrazione di Active Directory ora offre la cronologia completa di tutti i cmdlet di Windows PowerShell eseguiti, con i relativi argomenti e valori. È possibile copiare la cronologia dei cmdlet altrove per esaminarli, modificarli e riutilizzarli. È possibile creare note attività per isolare i risultati dei comandi del Centro di amministrazione di Active Directory in Windows PowerShell. È anche possibile filtrare la cronologia per trovare punti di interesse.  
  
Il Visualizzatore della cronologia di Windows PowerShell del Centro di amministrazione di Active Directory consente all'utente di imparare attraverso l'esperienza pratica.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Fare clic sulla freccia di espansione per aprire il Visualizzatore della cronologia di Windows PowerShell.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Creare quindi un utente o modificare l'appartenenza di un gruppo. Il visualizzatore della cronologia viene aggiornato continuamente con una visualizzazione compressa di ogni cmdlet eseguito dal Centro di amministrazione di Active Directory con gli argomenti specificati.  
  
Espandere le righe a cui si è interessati per visualizzare tutti i valori forniti agli argomenti del cmdlet:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Fare clic sul menu **Avvia attività** per creare una notazione manuale prima di usare il Centro di amministrazione di Active Directory per creare, modificare o eliminare un oggetto. Digitare l'operazione eseguita.  Al termine della modifica, selezionare **Termina attività**. La nota dell'attività raggruppa tutte le operazioni eseguite in una nota comprimibile per una migliore comprensione.  
  
Ad esempio, per visualizzare i comandi di Windows PowerShell usati per modificare la password di un utente e rimuovere quest'ultimo da un gruppo:  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
Se si seleziona la casella di controllo Mostra tutto, vengono visualizzati i cmdlet di Windows PowerShell con il verbo Get-* che recuperano solo i dati.  
  
![Gestione avanzata di servizi di dominio Active Directory](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
Il visualizzatore della cronologia mostra i comandi letterali eseguiti dal Centro di amministrazione di Active Directory ed è possibile osservare che l'esecuzione di alcuni cmdlet sembra non essere necessaria. Ad esempio, è possibile creare un nuovo utente con:  

```powershell
new-aduser
```

senza usare:  

```powershell
set-adaccountpassword  
enable-adaccount  
set-aduser  
```

La progettazione del Centro di amministrazione di Active Directory ha richiesto modularità e uso di codice minimi. Di conseguenza, invece di un set di funzioni per la creazione dei nuovi utenti e di un altro set per la modifica degli utenti esistenti, esegue tutte le funzioni e le concatena con i cmdlet. Tenere presente questo concetto durante l'apprendimento di Windows PowerShell per Active Directory. Questa può anche essere considerata una tecnica di apprendimento che consente di visualizzare come è facile usare Windows PowerShell per completare un'attività.  
  
## <a name="BKMK_Tshoot"></a>Risoluzione dei problemi di gestione di servizi di dominio Active Directory  
  
### <a name="introduction-to-troubleshooting"></a>Introduzione alla risoluzione dei problemi

Poiché è stato introdotto di recente e l'uso negli ambienti dei clienti esistenti è limitato, nel Centro di amministrazione di Active Directory è presente un numero ridotto di opzioni di risoluzione dei problemi.  
  
### <a name="troubleshooting-options"></a>Opzioni di risoluzione dei problemi  
  
#### <a name="logging-options"></a>Opzioni di registrazione

Il Centro di amministrazione di Active Directory ora contiene la registrazione incorporata, come parte di un file di configurazione di traccia. Creare/modificare il file seguente nella stessa cartella del file dsac.exe:  
  
**dsac. exe. config**
  
Creare il contenuto seguente:  
  
```xml
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

I livelli di dettaglio per **DsacLogLevel** sono **Nessuno**, **Errore**, **Avviso**, **Info** e **Dettagliato**. Il nome del file di output è configurabile e viene scritto nella stessa cartella del file dsac.exe. L'output comunica maggiori informazioni sul funzionamento di ADAC, quali controller di dominio sono stati contattati, quali comandi di Windows PowerShell sono stati eseguiti, le risposte ricevute e altri dettagli.  

Il livello di dettaglio INFO ad esempio restituisce tutti i risultati ad eccezione dei dettagli a livello di traccia:  
  
- Avvio di DSAC.exe  
- Avvio della registrazione  
- Controller di dominio a cui è stato richiesto di restituire le informazioni sul dominio iniziali  

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
   ```

- Controller di dominio DC1 restituito dal dominio Corp  
- Unità virtuale di PowerShell per Active Directory caricata  

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
   ```
  
- Recupero delle informazioni sull'attributo DSE radice del dominio  

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
   ```

- Recupero delle informazioni sul cestino di Active Directory del dominio  

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- Recupero della foresta di Active Directory  

   ```  
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
   ```

- Recupero delle informazioni sullo schema per i tipi di crittografia supportati, per i criteri granulari per le password e determinate informazioni sugli utenti  

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

- Recupero di tutte le informazioni dell'oggetto dominio per mostrarle all'amministratore che ha fatto clic sul vertice del dominio.  

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

L'impostazione del livello Dettagliato consente inoltre di visualizzare gli stack .NET per ogni funzione, ma poiché contengono un numero ridotto di dati non risultano particolarmente utili se non per la risoluzione dei problemi di Dsac.exe quando si verifica una violazione dell'accesso o un arresto anomalo. Le due cause probabili di questo problema sono le seguenti:
  
- Il servizio Servizi Web Active Directory non è in esecuzione in ogni controller di dominio accessibile.
- Le comunicazioni di rete sono bloccate per il servizio ADWS dal computer in cui è in esecuzione la Centro di amministrazione di Active Directory.

> [!IMPORTANT]  
> È disponibile anche una versione fuori programma del servizio chiamata [Gateway di gestione di Active Directory](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), che viene eseguita in Windows Server 2008 SP2 e Windows Server 2003 SP2.
>

Di seguito sono illustrati gli errori visualizzati quando non sono disponibili istanze per il servizio Servizi Web Active Directory:  
  
|Errore|Operazione|
| --- | --- |  
|"Impossibile connettersi ad alcun dominio. Aggiornare la pagina o riprovare quando sarà disponibile una connessione."|Visualizzato all'avvio del Centro di amministrazione di Active Directory|
|"Impossibile trovare un server disponibile nel dominio *<NetBIOS domain name>* in cui è in esecuzione il servizio Web di Active Directory (ADWS)"|Visualizzato quando si tenta di selezionare un nodo di dominio nell'applicazione Centro di amministrazione di Active Directory|
  
Per risolvere questo problema, eseguire la procedura seguente:  
  
1. Verificare che il servizio Servizi Web Active Directory sia avviato in almeno un controller di dominio nel dominio e preferibilmente in tutti i controller di dominio nella foresta. Verificare inoltre che sia impostato per l'avvio automatico in tutti i controller di dominio.
2. Dal computer che esegue il Centro di amministrazione di Active Directory, verificare che sia possibile individuare un server che esegue Servizi Web Active Directory usando questi due comandi NLTest.exe:  

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   Se i test hanno esito negativo anche se Servizi Web Active Directory è in esecuzione, si tratta di un problema che riguarda la risoluzione dei nomi o LDAP e non Servizi Web Active Directory o il Centro di amministrazione di Active Directory. Il test ha esito negativo e presenta l'errore "1355 0x54B ERROR_NO_SUCH_DOMAIN" se Servizi Web Active Directory non è in esecuzione in alcun controller di dominio, quindi verificare di nuovo prima di trarre conclusioni.  
  
3. Nel controller di dominio restituito da NLTest, acquisire l'elenco delle porte di ascolto con il comando:  

   ```
   Netstat -anob > ports.txt  
   ```

   Esaminare il file ports.txt e verificare che il servizio Servizi Web Active Directory sia in ascolto sulla porta 9389. Esempio:  

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  

   TCP    [::]:9389       [::]:0       LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  
   ```

   Se è in ascolto, verificare le regole di Windows Firewall e assicurarsi che sia consentito il traffico TCP in ingresso sulla porta 9389. Per impostazione predefinita, per i controller di dominio è abilitata la regola del firewall "Servizi Web Active Directory (TCP-In). Se non è in ascolto, verificare di nuovo che il servizio sia in esecuzione nel server e riavviarlo. Verificare che non vi siano altri processi già in ascolto sulla porta 9389.  
  
4. Installare NetMon o un'altra utilità di acquisizione di rete nel computer in cui è in esecuzione il Centro di amministrazione di Active Directory e nel controller di dominio restituito da NLTEST. Raccogliere acquisizioni di rete simultanee da entrambi i computer da quando viene avviato il Centro di amministrazione di Active Directory fino a quando non viene visualizzato l'errore, quindi arrestare le acquisizioni. Verificare che il client sia in grado di inviare e ricevere dal controller di dominio sulla porta TCP 9389. Se i pacchetti vengono inviati ma non vengono recapitati o vengono recapitati e il controller di dominio risponde ma non raggiungono mai il client, è probabile che sia presente un firewall tra i computer nella rete che ignora i pacchetti su quella porta. Il firewall può essere sia software che hardware e può essere parte di un software di protezione degli endpoint (antivirus) di terze parti.  
  
## <a name="see-also"></a>Vedere anche

[Cestino di AD, criteri granulari per le password e cronologia di PowerShell](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
