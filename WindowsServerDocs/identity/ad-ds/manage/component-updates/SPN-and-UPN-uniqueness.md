---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: "Unicità SPN e UPN"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 81d686c81082ad29384585d541c1304d654e1924
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="spn-and-upn-uniqueness"></a>Unicità SPN e UPN

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
I controller di dominio che esegue Windows Server 2012 R2 blocco la creazione di duplicare i nomi dell'entità servizio (SPN) e i nomi dell'entità utente (UPN). Ciò vale anche se il ripristino o il recupero di un oggetto eliminato o la ridenominazione di un oggetto comporta un duplicato.  
  
### <a name="background"></a>Sfondo  
Duplicati principale nomi servizio (SPN) in genere si verificano e causare errori di autenticazione e potrebbe causare l'utilizzo eccessivo della CPU di LSASS. Non esiste alcun metodo incorporato per impedire l'aggiunta di un nome SPN duplicati o UPN. *  
  
I valori UPN duplicati interrompere la sincronizzazione tra locale AD e Office 365.  
  
*Setspn.exe viene comunemente usato per creare nuovi nomi SPN e funzionale è stato creato nella versione rilasciata con Windows Server 2008 che consente di aggiungere un controllo di duplicati.  
  
**Tabella SEQ tabella \ \ \ * 1 ARABO: unicità SPN e UPN**  
  
|Funzionalità|Commento|  
|-----------|-----------|  
|Univocità UPN|Sincronizzazione di interruzione UPN duplicato locale gli account di Active Directory con servizi basati su Active Directory di Windows Azure, ad esempio Office 365.|  
|Unicità SPN|Kerberos richiede nomi SPN per l'autenticazione reciproca.  SPN duplicati causare errori di autenticazione.|  
  
Per ulteriori informazioni sui requisiti di univocità per i nomi SPN e UPN, vedere [vincoli di unicità](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Sintomi  
Codici di errore 8467 o 8468 o loro esadecimale, simbolici o stringhe equivalenti sono registrati sullo schermo in diverse finestre di dialogo e nell'evento 2974: ID nel registro eventi di servizi di Directory. Il tentativo di creare un nome UPN o SPN duplicati viene bloccato solo nelle circostanze seguenti:  
  
-   La scrittura viene elaborata da Windows Server 2012 R2 controller di dominio  
  
**Tabella SEQ tabella \ \ \ * ARABIC 2: codici di errore di unicità SPN e UPN**  
  
|Decimale|Esadecimale|Simbolici|Stringa|  
|-----------|-------|------------|----------|  
|8467|7 21C|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|L'operazione non riuscita perché il valore di nome SPN specificato per l'aggiunta/modifica non è univoco a livello di foresta.|  
|8648|8 21C|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|L'operazione non riuscita perché il valore UPN specificato per l'aggiunta/modifica non è univoco a livello di foresta.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Creazione di nuovi utenti ha esito negativo se non è univoco UPN  
  
### <a name="dsamsc"></a>DSA. msc  
Il nome di accesso utente che si è scelto è già in uso nell'organizzazione. Scegliere un altro nome di accesso e quindi riprovare.  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificare un account esistente:  
  
Il nome di accesso utente specificato esiste già nell'organizzazione. Specificare uno nuovo, tramite la modifica del prefisso o selezionando un suffisso diverso dall'elenco.  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro di amministrazione di Active Directory (DSAC.exe)  
Un tentativo di creare un nuovo utente nel centro di amministrazione di Active Directory con un nome UPN che esiste già, verrà restituito l'errore seguente.  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura SEQ Figure \ * ARABO 1 errore visualizzato nel centro di amministrazione di Active Directory quando si verifica un errore di creazione di nuovi utenti a causa di UPN duplicato**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Origine evento 2974:: ActiveDirectory DomainService  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura SEQ Figure \ * ARABIC 2974 di ID evento 2 con errore 8648**  
  
Evento 2974: Elenca il valore che è stato bloccato e un elenco di uno o più oggetti (fino a 10) che già contengono quel valore.  Nella figura seguente, è possibile visualizzare il valore dell'attributo UPN *** dhunt@blue.contoso.com *** è già presente nei quattro altri oggetti.  Poiché si tratta di una nuova funzionalità di Windows Server 2012 R2, la creazione accidentale di duplicati UPN e SPN in un ambiente misto verrà generato anche quando i controller di dominio di livello inferiore di elaborare il tentativo di scrittura.  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura SEQ Figure \ * 2974 di eventi 3 ARABO che mostra tutti gli oggetti che contiene il nome UPN duplicato**  
  
> [!TIP]  
> Esaminare regolarmente a 2974s ID evento:  
>   
> -   identificare i tentativi di creare nomi SPN o UPN duplicato  
> -   identificare gli oggetti che contengono duplicati  
  
8648 = "Operazione non riuscita perché il valore UPN specificato per l'aggiunta/modifica non è univoco a livello di foresta".  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe ha SPN il rilevamento dei duplicati incorporato dopo il rilascio di Windows Server 2008 quando si utilizza il **"-S"** opzione.  È possibile ignorare il rilevamento di SPN duplicato utilizzando il **"-A"** opzione tuttavia.  Creazione di un SPN duplicato viene bloccata quando la destinazione è Windows Server 2012 R2 controller di dominio utilizzando SetSPN con l'opzione - A.  Il messaggio di errore visualizzato è lo stesso di quello visualizzato quando si utilizza l'opzione -S: "Duplicate SPN trovato, l'operazione di interruzione!"  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura SEQ Figure \ * il messaggio di errore di 4 ARABO visualizzato in ADSIEdit quando viene bloccato l'aggiunta di UPN duplicato**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS in esecuzione da Server 2012, Windows Server 2012 R2 controller di dominio di destinazione:  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe in esecuzione su Windows Server 2012, Windows Server 2012 R2 controller di dominio di destinazione:  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura SEQ Figure \ * errore di creazione utente ARABO DSAC 5 su non - Windows Server 2012 R2 durante l'assegnazione di controller di dominio di Windows Server 2012 R2**  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura SEQ Figure \ * errore di modifica utente DSAC 6 ARABO sul non - Windows Server 2012 R2 durante l'assegnazione di controller di dominio di Windows Server 2012 R2**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Ripristino di un oggetto che può determinerebbe un UPN duplicato ha esito negativo:  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Nessun evento viene registrato quando un oggetto non riesce a ripristinare a causa di un UPN duplicato o SPN.  
  
L'UPN dell'oggetto deve essere univoco in modo tale da ripristinare.  
  
1.  Identificare il nome UPN che esiste nell'oggetto nel Cestino  
  
2.  Identificare tutti gli oggetti che hanno lo stesso valore  
  
3.  Rimuovere i duplicati UPN (s)  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identificare l'UPN in conflitto di repadmin.exe di objectUsing eliminato  
  
```  
Repadmin /showattr DCName "DN of deleted objects container" /subtree /filter:"(msDS-LastKnownRDN=<NAME>)" /deleted /atts:userprincipalname  
```  
  
```  
repadmin /showattr DCName "CN=Deleted Objects,DC=blue,DC=contoso,DC=com" /subtree /filter:"(msDS-LastKnownRDN=Dianne Hunt2)" /deleted /atts:userprincipalname  
  
C:\>repadmin /showattr winbluedc1 "cn=deleted objects,dc=blue,dc=contoso,dc=com" /subtree /filter:"(msds-lastknownrdn=Dianne Hunt2)" /deleted /atts:userprincipalname  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Object  
s,DC=blue,DC=contoso,DC=com  
    1> userPrincipalName: dhunt@blue.contoso.com  
```  
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Per identificare tutti gli oggetti con la stessa UPN: utilizzando Repadmin.exe  
  
```  
repadmin /showattr WinBlueDC1 "DC=blue,DC=contoso,DC=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
  
C:\>repadmin /showattr winbluedc1 "dc=blue,dc=contoso,dc=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
DN: CN=Administrator,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser1,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser10,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser100,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt,OU=Marketing,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Objects,DC=blue,DC=contoso,DC=com  
```  
  
> [!TIP]  
> Precedentemente non documentati **/ eliminato** parametro Repadmin.exe viene utilizzato per includere gli oggetti eliminati nel set di risultati  
  
### <a name="using-global-search"></a>Utilizzo della ricerca globale  
  
-   Centro di amministrazione aprire Active Directory e passare a **ricerca globale**  
  
-   Selezionare il **Converti in LDAP** pulsante di opzione  
  
-   Tipo * *(userPrincipalName =*ConflictingUPN*) * *  
  
    -   Sostituire ***ConflictingUPN*** con l'UPN effettivo che è in conflitto  
  
-   Selezionare **applicare**  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Utilizzo di Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Se l'oggetto deve essere ripristinato, sarà necessario rimuovere UPN duplicato da altri oggetti.  Per un solo oggetto, è abbastanza semplice per utilizzare ADSIEdit per rimuovere il duplicato.  Se sono presenti più oggetti con i duplicati, Windows PowerShell potrebbe essere lo strumento migliore da utilizzare.  
  
Per annullare l'attributo UserPrincipalName utilizzando Windows PowerShell:  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> L'attributo userPrincipalName è l'attributo a valore singolo, pertanto questa procedura verrà rimosso solo il nome UPN duplicato.  
  
### <a name="duplicate-spn"></a>SPN duplicato  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura SEQ Figure \ * ARABIC 8 errore visualizzato in ADSIEdit quando viene bloccato l'aggiunta di SPN duplicato**  
  
Registrato nei servizi Directory registro eventi è un **ActiveDirectory DomainService** ID evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Unicità SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura SEQ Figure \ * ARABO 9 errore registrato quando viene bloccata la creazione di SPN duplicato**  
  
### <a name="workflow"></a>Flusso di lavoro  
  
-   **Se DC = = GC**  
  
    -   Nessuna chiamata offbox necessari, query può essere soddisfatta localmente  
  
    -   ***Caso UPN***  
  
        -   Query a livello di foresta UPN indice locale per UPN specificato (*userPrincipalName; un indice globale*)  
  
            -   Se le voci restituite = = 0 -> procede di scrittura  
  
            -   Se le voci restituite! = 0 -> scrittura ha esito negativo  
  
                -   Evento registrato  
  
                -   Restituisce inoltre errore esteso:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Query a livello di foresta SPN indice locale per nome SPN fornito (*servicePrincipalName; un indice globale*)  
  
            -   Se le voci restituite = = 0 -> procede di scrittura  
  
            -   Se le voci restituite! = 0 -> scrittura ha esito negativo  
  
                -   Evento registrato  
  
                -   Restituisce inoltre errore esteso:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Se DC! = GC**  
  
    -   Chiamata di Offbox **auspicabile** ma non critico, ad esempio, si tratta di un controllo di univocità sforzo  
  
        -   Controllo procede contro DIT locale solo se non è possibile individuare GC  
  
        -   Evento registrato per indicare ad  
  
    -   ***Caso UPN***  
  
        -   Inviare query LDAP GC più vicino? query del catalogo globale a livello di foresta UPN indice per UPN specificato (*userPrincipalName; un indice globale*)  
  
            -   Se le voci restituite = = 0 -> procede di scrittura  
  
            -   Se le voci restituite! = 0 -> scrittura ha esito negativo  
  
                -   Evento registrato  
  
                -   Restituisce inoltre errore esteso:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Inviare query LDAP GC più vicino? query del catalogo globale a livello di foresta SPN indice per nome SPN fornito (*servicePrincipalName; un indice globale*)  
  
            -   Se le voci restituite = = 0 -> procede di scrittura  
  
            -   Se le voci restituite! = 0 -> scrittura ha esito negativo  
  
                -   Evento registrato  
  
                -   Restituisce inoltre errore esteso:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Quando gli oggetti eliminati vengono nuovamente animati, i valori SPN o UPN presenti sono verificati l'unicità. Se viene trovato un duplicato, la richiesta ha esito negativo.  
  
-   Per alcune modifiche di attributo come nome Host DNS, nome Account SAM e così via, quando viene effettuata la modifica, nomi SPN vengono aggiornati di conseguenza. Durante il processo, i nomi SPN obsoleti vengono eliminati e nuovi nomi SPN vengono costruiti e aggiunti al database. Le modifiche necessarie attributo rispetto alla quale viene attivato questo percorso sono:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Se un nuovo valore SPN è un duplicato, correttamente la modifica. L'elenco precedente, gli attributi più importanti sono ATT_DNS_HOST_NAME (nome del computer) e ATT_SAM_ACCOUNT_NAME (nome Account SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Procedura: Esplorare unicità SPN e UPN  
Questo è il primo di diversi "**prova**" attività nel modulo.  Non è disponibile una Guida di laboratorio separato per questo modulo.  Il **prova** le attività sono essenzialmente le attività in formato libero che consentono esplorare il materiale della lezione nell'ambiente di laboratorio.  È possibile dopo il prompt o attraversa script e ideare proprie attività.  
  
> [!NOTE]  
> -   Questo è il primo di diversi "**prova**" attività.  
> -   Non è disponibile una Guida di laboratorio separato per questo modulo.  
> -   Il **prova** le attività sono essenzialmente le attività in formato libero che consentono esplorare il materiale della lezione nell'ambiente di laboratorio.  
> -   È possibile dopo il prompt o attraversa script e ideare proprie attività.  
> -   Anche se non tutte le sezioni sono un **prova** prompt dei comandi, sono ancora invitati a esplorare il contenuto della lezione nel laboratorio dove appropriato.  
  
Sperimentare unicità SPN e UPN.  Seguire queste istruzioni o completare il proprio.  
  
1.  Creare nuovi utenti con UPN  
  
2.  Creare gli account con SPN  
  
3.  Creare un nuovo utente con un nome UPN già precedentemente definito o cambiare l'UPN di un account esistente.  Ripetere l'operazione per un nome SPN per un altro account  
  
    1.  Popola un account utente con un nome UPN già in uso  
  
        1.  Utilizzando Active Directory, ADSIEDIT o PowerShell centro di amministrazione (DSAC.exe)  
  
    2.  Popola un account esistente con un nome SPN già in uso  
  
        1.  Uso di Windows PowerShell, ADSIEDIT o SetSPN  
  
4.  Osservare gli errori  
  
**Facoltativamente**  
  
1.  Verificare con il docente di corsi che è comunque possibile abilitare il * [Cestino di Active Directory](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin) * nel centro di amministrazione di Active Directory.  In tal caso, passare al passaggio successivo.  
  
2.  Popolare l'UPN di un account utente  
  
3.  Eliminare l'account  
  
4.  Inserire un altro account con il nome UPN stesso come l'account eliminato  
  
5.  Tentativo di utilizzare la GUI di bin Cestino per ripristinare l'account  
  
6.  Si supponga che sono appena state presentate con l'errore che viene visualizzato nel passaggio precedente.  (e non dispone di una cronologia dei passaggi che appena eseguita) L'obiettivo consiste nel completare il ripristino dell'account.  Vedere i che passaggi, ad esempio la cartella di lavoro.  
  


