---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: Distribuire un criterio di accesso centrale (procedura dimostrativa)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Distribuire un criterio di accesso centrale (procedura dimostrativa)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo scenario, le operazioni di protezione del reparto finanziario collabora con sicurezza centrale delle informazioni per specificare l'esigenza di un criterio di accesso centrale in modo da permettere di proteggere le informazioni finanziarie archiviate nei file server. Le informazioni finanziarie archiviate per ogni paese sono accessibili in sola lettura da dipendenti del reparto finanziario dello stesso paese. Un gruppo di amministratori finanziari centrali può accedere alle informazioni finanziarie da tutti i paesi.  
  
La distribuzione di criteri di accesso centrale include le fasi seguenti:  
  
|Fase|Descrizione  
|---------|---------------  
|[Pianificazione: Identificare la necessità di criteri e la configurazione necessaria per la distribuzione](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identificare la necessità di un criterio e la configurazione necessaria per la distribuzione. 
|[Implementare: Configurare i componenti e i criteri](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configurare i componenti e i criteri.  
|[Distribuire il criterio di accesso centrale](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Distribuire il criterio.  
|[Gestisci: Modificare e gestire i criteri](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Le modifiche dei criteri e gestione temporanea. 
  
## <a name="BKMK_1.1"></a>Configurare un ambiente di test  
Prima di iniziare, è necessario impostare laboratorio di test di questo scenario. I passaggi per configurare il lab sono spiegati in dettaglio in [appendice b: Setting Up the Test Environment ](Appendix-B--Setting-Up-the-Test-Environment.md).  
  
## <a name="BKMK_1.2"></a>Pianificazione: Identificare la necessità di criteri e la configurazione necessaria per la distribuzione  
Questa sezione fornisce ad alto livello serie di passaggi che semplificano la fase di pianificazione della distribuzione.  
  
||Passaggio|Esempio|  
|-|--------|-----------|  
|1.1|Business determina che è necessario un criterio di accesso centrale|Per proteggere le informazioni finanziarie archiviate nei file server, le operazioni di protezione del reparto finanziario collabora con sicurezza centrale delle informazioni per specificare l'esigenza di un criterio di accesso centrale.|  
|1.2|Esprimere il criterio di accesso|I documenti finanziari devono essere letti solo dai membri del reparto finanziario. I membri del reparto finanziario devono accedere solo ai documenti relativi al proprio paese. L'accesso in scrittura deve essere solo agli amministratori di Finanza. Potrà essere un'eccezione per i membri del gruppo FinanceException. Questo gruppo sarà assegnato l'accesso in lettura.|  
|1.3|Esprimere il criterio di accesso in costrutti di Windows Server 2012|Destinazione:<br /><br />-Resource.Department contiene Finanza<br /><br />Regole di accesso:<br /><br />-Consente di lettura User.Country=Resource.Country e User.department = Resource.Department<br />-Controllo completo User.MemberOf(FinanceAdmin)<br /><br />Eccezione:<br /><br />Consenti memberOf(FinanceException) lettura|  
|1.4|Determinare le proprietà di file necessari per i criteri|Contrassegnare i file con:<br /><br />-Reparto<br />-Paese|  
|1.5|Determinare i tipi di attestazione e i gruppi necessari per i criteri|Tipi di attestazione:<br /><br />-Paese<br />-Reparto<br /><br />Gruppi di utenti:<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|Determinare i server a cui applicare questo criterio|Applicare il criterio finanziario tutti i file server.|  
  
## <a name="BKMK_1.3"></a>Implementare: Configurare i componenti e i criteri  
In questa sezione offre un esempio che consente di distribuire un criterio di accesso centrale per documenti finanziari.  
  
|No|Passaggio|Esempio|  
|------|--------|-----------|  
|2.1|Creare tipi di attestazioni|Creare i tipi di attestazione seguenti:<br /><br />-Reparto<br />-Paese|  
|2.2|Creare proprietà delle risorse|Creare e abilitare le proprietà delle risorse seguenti:<br /><br />-Reparto<br />-Paese|  
|2.3|Configurare una regola di accesso centrale|Creare una regola Finance Documents che include il criterio definito nella sezione precedente.|  
|2.4|Configurare un criterio di accesso centrale (CAP)|Creare un criterio di autorizzazione connessioni denominato Finance Policy e aggiungervi la regola Finance Documents che Criteri di autorizzazione connessioni.|  
|2.5|Criteri di accesso centrale ai file server di destinazione|Pubblicare i file server di accesso centrale Finance Policy.|  
|2.6|Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos.|Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos per contoso.com.|  
  
Nella procedura seguente creare due tipi di attestazione: Country e Department.  
  
#### <a name="to-create-claim-types"></a>Per creare tipi di attestazione  
  
1.  Aprire Server DC1 nella gestione di Hyper-V e accedere come contoso\administrator, con la password **pass@word1**.  
  
2.  Aprire Centro di amministrazione di Active Directory.  
  
3.  Fare clic su di **icona di visualizzazione albero**, espandere **controllo dinamico degli accessi**e quindi seleziona **tipi di attestazione **.  
  
    Fare doppio clic su **tipi di attestazione**, fare clic su **New**, quindi fare clic su **tipo di attestazione **.  
  
    > [!TIP]  
    > È anche possibile aprire un **Crea tipo di attestazione:** finestra il **attività** riquadro. Nel **attività** riquadro, fare clic su **New**, quindi fare clic su **tipo di attestazione **.  
  
4.  Nel **attributo di origine** scorrere verso il basso l'elenco di attributi e fare clic **reparto**. Sarà inserito il **nome visualizzato** campo **reparto**. Fare clic su **OK**.  
  
5.  In **attività** riquadro, fare clic su **New**, quindi fare clic su **tipo di attestazione**.  
  
6.  Nel **attributo di origine** elenco, scorrere verso il basso l'elenco di attributi e quindi fare clic su di **c** attributo (Country-Name). Nel **nome visualizzato** digitare **paese**.  
  
7.  Nel **valori suggeriti** selezionare **i valori suggeriti:**, quindi fare clic su **Aggiungi**.  
  
8.  Nel **valore** e **nome visualizzato** campi, digitare **degli Stati Uniti**, quindi fare clic su **OK**.  
  
9. Ripetere il passaggio precedente. Nel **Aggiungi valore suggerito** la finestra di dialogo, digitare **JP** nel **valore** e **nome visualizzato** campi, quindi fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> È possibile utilizzare il Visualizzatore della cronologia di Windows PowerShell nel centro di amministrazione di Active Directory per cercare i cmdlet di Windows PowerShell per ogni procedura eseguita nel centro di amministrazione di Active Directory. Per ulteriori informazioni, vedere [Visualizzatore della cronologia di Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
Il passaggio successivo consiste nel creare proprietà delle risorse. Nella procedura seguente creare una proprietà della risorsa che viene automaticamente aggiunto all'elenco di proprietà delle risorse globali nel controller di dominio, in modo che sia disponibile per il file server.  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Per creare e abilitare le proprietà delle risorse create in precedenza  
  
1.  Nel riquadro sinistro del centro di amministrazione di Active Directory, fare clic su **visualizzazione albero**. Espandere **controllo dinamico degli accessi**e quindi seleziona **le proprietà delle risorse**.  
  
2.  Fare doppio clic su **le proprietà delle risorse**, fare clic su **New**, quindi fare clic su **proprietà risorsa di riferimento**.  
  
    > [!TIP]  
    > È inoltre possibile scegliere una proprietà della risorsa dal **attività** riquadro. Fare clic su **New** e quindi fare clic su **proprietà risorsa di riferimento**.  
  
3.  In **selezionare viene suggerito un tipo di attestazione di condividerlo in valori di elenco**, fare clic su **paese**.  
  
4.  Nel **nome visualizzato** digitare **paese**, quindi fare clic su **OK**.  
  
5.  Fare doppio clic il **le proprietà delle risorse** elenco, scorrere verso il basso il **reparto** proprietà della risorsa. Pulsante destro del mouse e quindi fare clic su **abilitare**. In questo modo sarà predefinito **reparto** proprietà della risorsa.  
  
6.  Nel **le proprietà delle risorse** elenco nel riquadro di spostamento del centro di amministrazione di Active Directory, saranno disponibili due proprietà abilitate delle risorse:  
  
    -   Paese  
  
    -   Reparto  
  
![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
Il passaggio successivo consiste nel creare regole di accesso centrale che definiscono chi possono accedere alle risorse. In questo scenario le regole business sono:  
  
-   I documenti finanziari possono essere letti solo dai membri del reparto finanziario.  
  
-   I membri del reparto finanziario possono accedere solo ai documenti relativi al proprio paese.  
  
-   Solo gli amministratori finanziari possono avere accesso in scrittura.  
  
-   Sarà permessa un'eccezione per i membri del gruppo FinanceException. Questo gruppo sarà assegnato l'accesso in lettura.  
  
-   L'amministratore e il proprietario del documento continueranno ad avere accesso completo.  
  
O per esprimere le regole con costrutti di Windows Server 2012:  
  
Destinazione: Resource.Department contiene Finanza  
  
Regole di accesso:  
  
-   Consenti lettura User.Country=Resource.Country e User.department = Resource.Department  
  
-   Consentire il controllo completo User.MemberOf(FinanceAdmin)  
  
-   Consenti User.MemberOf(FinanceException) lettura  
  
#### <a name="to-create-a-central-access-rule"></a>Per creare una regola di accesso centrale  
  
1.  Nel riquadro a sinistra di centro di amministrazione di Active Directory, fare clic su **visualizzazione albero**selezionare **controllo dinamico degli accessi**, quindi fare clic su **regole di accesso centrale**.  
  
2.  Fare doppio clic su **regole di accesso centrale**, fare clic su **New**, quindi fare clic su **regola di accesso centrale**.  
  
3.  Nel **nome** digitare **Finance Documents Rule**.  
  
4.  Nel **risorse di destinazione** fare clic su **modifica**e nel **regola di accesso centrale** la finestra di dialogo, fare clic su **aggiungere una condizione**. Aggiungere la condizione seguente:   
    [**Risorse**] [**Reparto**] [**è uguale a**] [**Value**] [**Finance**], quindi fare clic su **OK**.  
  
5.  Nel **autorizzazioni** selezionare **utilizzare le seguenti autorizzazioni come correnti**, fare clic su **modifica**e il **impostazioni di sicurezza avanzate per autorizzazioni** finestra di dialogo fare clic su **Aggiungi**.  
  
    > [!NOTE]  
    > **Usa queste autorizzazioni come proposte** opzione consente di creare il criterio in gestione temporanea. Per ulteriori informazioni su come eseguire questa operazione consultare la sezione: modifica e fase la sezione relativa ai criteri in questo argomento.  
  
6.  Nel **voce autorizzazione per autorizzazioni** la finestra di dialogo, fare clic su **seleziona un'entità**, tipo **Authenticated Users**, quindi fare clic su **OK**.  
  
7.  Nel **voce autorizzazione per autorizzazioni** la finestra di dialogo, fare clic su **aggiungere una condizione**e aggiungere le condizioni seguenti:   
    [**User**] [**paese**] [**Any of**] [**Risorse**] [**paese**]   
     Fare clic su **aggiungere una condizione**.   
     [**And**]   
    Fare clic su [**utente**] [**reparto**] [**uno qualsiasi dei**] [**risorse**] [**reparto**]. Impostare il **autorizzazioni** a **lettura**.  
  
8.  Fare clic su **OK**, quindi fare clic su **Aggiungi**. Fare clic su **seleziona un'entità**, tipo **FinanceAdmin**, quindi fare clic su **OK**.  
  
9. Selezionare il **modifica, lettura ed esecuzione, lettura, scrittura** autorizzazioni, quindi fare clic su **OK**.  
  
10. Fare clic su **Aggiungi**, fare clic su **seleziona un'entità**, tipo **FinanceException**, quindi fare clic su **OK**. Selezionare le autorizzazioni per essere **lettura** e **lettura ed esecuzione**.  
  
11. Fare clic su **OK** tre volte per completare e tornare al centro di amministrazione di Active Directory.  
  
    ![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
    Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
 
    $countryClaimType = Get-ADClaimType paese  
    $departmentClaimType = Get-ADClaimType reparto  
    $countryResourceProperty = Get-ADResourceProperty paese  
    $departmentResourceProperty = Get-ADResourceProperty reparto  
    $currentAcl = "O:SYG:SYD:AR(A;; FA;; O W) (A; FA;; BA) (A; 0 x1200a9;; S-1-5-21-1787166779-1215870801-2157059049-1113) (A; 0 x1301bf;; S-1-5-21-1787166779-1215870801-2157059049-1112) (A; FA;; SY) (XA; 0 x1200a9;; AU;((@USER." + $countryClaimType.Name + "Any_of @RESOURCE." + $countryResourceProperty.Name + ") & & (@USER." + $departmentClaimType.Name + "Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
    $resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + "Contains {`"Finance`"}) "  
    New-ADCentralAccessRule "Finance Documents Rule" - CurrentAcl $currentAcl - ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> Nell'esempio precedente, cmdlet di identificatori di sicurezza (SID) per il gruppo FinanceAdmin e gli utenti è determinato al momento della creazione e saranno diversi nell'esempio. Ad esempio, il SID valore fornito (S-1-5-21-1787166779-1215870801-2157059049-1113) per FinanceAdmins deve essere sostituito con il SID effettivo per il gruppo FinanceAdmin da creare nella distribuzione. È possibile utilizzare Windows PowerShell per cercare il valore del SID del gruppo, assegnare il valore a una variabile e quindi utilizzare tale variabile qui. Per ulteriori informazioni, vedere [suggerimento per Windows PowerShell: uso dei SID](https://go.microsoft.com/fwlink/?LinkId=253545).  
  
È ora una regola di accesso centrale che permette agli utenti di accedere a documenti dello stesso paese e dello stesso reparto. La regola permette al gruppo FinanceAdmin di modificare i documenti e consente al gruppo FinanceException di leggere i documenti. Questa regola è relativa solo ai documenti classificati come Finance.  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Per aggiungere una regola di accesso centrale a un criterio di accesso centrale  
  
1.  Nel riquadro a sinistra di centro di amministrazione di Active Directory, fare clic su **controllo dinamico degli accessi**, quindi fare clic su **criteri di accesso centrale**.  
  
2.  Nel **attività** riquadro, fare clic su **New**, quindi fare clic su **criteri di accesso centrale**.  
  
3.  In **creare criteri di accesso centrale:**, tipo **Finance Policy** nel **nome** casella.  
  
4.  In **regole di accesso centrale membri**, fare clic su **Aggiungi**.  
  
5.  Fare doppio clic il **Finance Documents Rule** per l'aggiunta per il **aggiungere le regole di accesso centrale seguenti** elenco e quindi fare clic su **OK **.  
  
6.  Fare clic su **OK** per terminare. È ora un criterio di accesso centrale denominato Finance Policy.  
  
    ![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
    Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Per applicare i criteri di accesso centrale nei file server utilizzando criteri di gruppo  
  
1.  Nel **Start** schermata il **ricerca** digitare **Gestione criteri di gruppo **. Fare doppio clic su **Gestione criteri di gruppo **.  
  
    > [!TIP]  
    > Se il **Mostra strumenti di amministrazione** impostazione è disabilitata, la **strumenti di amministrazione** cartella e il relativo contenuto non verrà visualizzati nel **impostazioni** risultati.  
  
    > [!TIP]  
    > Nell'ambiente di produzione, si deve creare un File Server unità Organizzativa e aggiungere tutti i file server per l'unità organizzativa specifica, in cui si desidera applicare questo criterio. Puoi quindi creare un criterio di gruppo e aggiungervi questa unità Organizzativa a tale criterio.  
  
2.  In questo passaggio si modifica l'oggetto Criteri di gruppo creato in [Build il controller di dominio](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) sezione nell'ambiente di testing per includere il criterio di accesso centrale creato. In Editor Gestione criteri di gruppo, individuare e selezionare l'unità organizzativa nel dominio (contoso.com in questo esempio): **Gestione criteri di gruppo**, **foresta: contoso.com**, **domini**, **contoso.com**, **Contoso**, **FileServerOU **.  
  
3.  Fare doppio clic su **FlexibleAccessGPO**, quindi fare clic su **modifica **.  
  
4.  Nella finestra Editor Gestione criteri di gruppo, passare a **configurazione Computer**, espandere **criteri**, espandere **le impostazioni di Windows**e fare clic su **le impostazioni di sicurezza **.  
  
5.  Espandere **File System**, fare doppio clic su **criteri di accesso centrale**, quindi fare clic su **criteri di accesso centrale gestire**.  
  
6.  Nel **configurazione di criteri di accesso centrale** finestra di dialogo, aggiungere **Finance Policy**, quindi fare clic su **OK **.  
  
7.  Scorrere verso il basso **configurazione avanzata dei criteri di controllo**ed espanderlo.  
  
8.  Espandere **criteri di controllo**e seleziona **accesso agli oggetti **.  
  
9. Fare doppio clic su **Controlla gestione temporanea criteri di accesso centrale **. Selezionare tutte e tre le caselle di controllo e quindi fare clic su **OK **. Questo passaggio consente al sistema di ricevere eventi di controllo relativi ai criteri di gestione temporanea di accesso centrale.  
  
10. Fare doppio clic su **controllo proprietà File System **. Selezionare tutte e tre le caselle di controllo, quindi fare clic su **OK **.  
  
11. Chiudere Editor Gestione criteri di gruppo. È stato incluso in Criteri di accesso centrale per i criteri di gruppo.  
  
Per i controller di dominio del dominio offrire attestazioni o dati di autorizzazione dispositivo, è necessario configurare per controllo dinamico degli accessi di supportare i controller di dominio.  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Per abilitare il supporto per le attestazioni e autenticazione composta per contoso.com  
  
1.  Aprire Gestione criteri di gruppo, fare clic su **contoso.com**, quindi fare clic su **i controller di dominio **.  
  
2.  Fare doppio clic su **criterio controller di dominio predefiniti**, quindi fare clic su **modifica**.  
  
3.  Nella finestra Editor Gestione criteri di gruppo, fare doppio clic su **configurazione Computer**, fare doppio clic su **criteri**, fare doppio clic su **modelli amministrativi**, fare doppio clic su **sistema**e quindi fare doppio clic su **KDC**.  
  
4.  Fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos **. Nel **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos** la finestra di dialogo, fare clic su **abilitato** e seleziona **supportati** dal **opzioni** elenco a discesa. (È necessario abilitare questa impostazione usare le attestazioni utente in Criteri di accesso centrale).  
  
5.  Chiudi **Gestione criteri di gruppo**.  
  
6.  Aprire un prompt dei comandi e digitare `gpupdate /force`.  
  
## <a name="BKMK_1.4"></a>Distribuire il criterio di accesso centrale  
  
||Passaggio|Esempio|  
|-|--------|-----------|  
|3.1|Assegnare i criteri di autorizzazione connessioni alle cartelle condivise appropriate sul file server.|Assegnare il criterio di accesso centrale alla cartella condivisa appropriata sul file server.|  
|3.2|Verificare che l'accesso è configurato correttamente.|Controllare l'accesso per gli utenti da diversi paesi e reparti.|  
  
In questo passaggio si assegnerà il criterio di accesso centrale a un file server. Accedere a un file server che riceve i criteri di accesso centrale creato i passaggi precedenti e assegnare i criteri in una cartella condivisa.  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Per assegnare un criterio di accesso centrale a un file server  
  
1.  Nella console di gestione Hyper-V connettersi al server FILE1. Accedere al server usando contoso\administrator con la password:**pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati e digitare: **gpupdate /force**. Ciò garantisce che le modifiche di criteri di gruppo vengono applicate al server.  
  
3.  È anche necessario aggiornare le proprietà delle risorse globali da Active Directory. Aprire una finestra di Windows PowerShell con privilegi elevati e digitare `Update-FSRMClassificationpropertyDefinition`. Fare clic su invio e quindi chiudere Windows PowerShell.  
  
    > [!TIP]  
    > È inoltre possibile aggiornare le proprietà delle risorse globali accedendo al file server. Per aggiornare le proprietà delle risorse globali dal file server, eseguire le operazioni seguenti  
    >   
    > 1.  Accesso al File Server FILE1 come contoso\administrator, usando la password **pass@word1**.  
    > 2.  Aprire Gestione risorse File Server. Per aprire Gestione risorse File Server, fare clic su **Start**, tipo **Gestione risorse file server**, quindi fare clic su **Gestione risorse File Server**.  
    > 3.  In Gestione risorse File Server, fare clic su **gestione classificazione File**, fare doppio clic su **le proprietà di classificazione** e quindi fare clic su **aggiornare **.  
  
4.  Aprire Esplora risorse e nel riquadro a sinistra, fare clic su unità D. fare doppio clic su di **Finance Documents** cartella e scegliere **proprietà **.  
  
5.  Fare clic su di **classificazione** scheda, fare clic su **paese**e quindi seleziona **degli Stati Uniti** nel **valore** campo.  
  
6.  Fare clic su **reparto**e quindi seleziona **Finance** nel **valore** campo e quindi fare clic su **applica **.  
  
    > [!NOTE]  
    > Tenere presente che è stato configurato il criterio di accesso centrale ai file di destinazione per il reparto finanziario. I passaggi precedenti contrassegno tutti i documenti nella cartella con gli attributi Country e Department.  
  
7.  Fare clic su di **sicurezza** scheda e quindi fare clic su **avanzate **. Fare clic su di **criteri centrali** scheda.  
  
8.  Fare clic su **modifica**selezionare **Finance Policy** dal menu a discesa e quindi fare clic su **applica **. È possibile visualizzare il **Finance Documents Rule** elencato nei criteri. Espandere l'elemento per visualizzare tutte le autorizzazioni impostate durante la creazione della regola in Active Directory.  
  
9. Fare clic su **OK** per tornare a Windows Explorer.  
  
Nel passaggio successivo, assicurarsi che l'accesso è configurato correttamente.  Gli account utente, devi avere il set di attributo Department appropriato (impostata questa utilizzando il centro di amministrazione di Active Directory). Il modo più semplice per visualizzare i risultati effettivi del nuovo criterio consiste nell'usare il **accesso valido** scheda in Esplora risorse. Il **accesso valido** scheda Mostra i diritti di accesso per un determinato account utente.  
  
#### <a name="to-examine-the-access-for-various-users"></a>Per esaminare l'accesso per i diversi utenti  
  
1.  Nella console di gestione Hyper-V connettersi al server FILE1. Accedere al server usando contoso\administrator. Passare a D:\ in Esplora risorse. Fare doppio clic su di **Finance Documents** cartella, quindi fare clic su **proprietà **.  
  
2.  Fare clic sul **sicurezza** scheda, fare clic su **avanzate**, quindi fare clic sul **accesso valido** scheda.  
  
3.  Per esaminare le autorizzazioni per un utente, fare clic su **selezionare un utente**, digitare il nome dell'utente e quindi fare clic su **Visualizza accesso valido** per visualizzare i diritti di accesso valido. Per esempio:  
  
    -   Myriam Delesalle (MDelesalle) sia nel reparto finanziario e deve disporre di accesso in lettura alla cartella.  
  
    -   Miles Reid (MReid) è un membro del gruppo FinanceAdmin e deve disporre di modifica accesso alla cartella.  
  
    -   Esther Valle (EValle) non è presente nel reparto finanziario; Tuttavia, è un membro del gruppo FinanceException e deve avere accesso in lettura.  
  
    -   Maira Wenzel (MWenzel) non è presente nel reparto finanziario e non è un membro di uno il gruppo FinanceAdmin o FinanceException. Utente non deve avere accesso alla cartella.  
  
    Si noti che l'ultima colonna denominata **limitato l'accesso** nella finestra di accesso valido. In questa colonna indica i gate che compromettono le autorizzazioni di una persona. In questo caso, le autorizzazioni di condivisione e NTFS consentono tutti gli utenti il controllo completo. Tuttavia, i criteri di accesso centrale limitano l'accesso in base alle regole configurate in precedenza.  
  
## <a name="BKMK_1.5"></a>Gestisci: Modificare e gestire i criteri  
  
||||  
|-|-|-|  
|Numero|Passaggio|Esempio|  
|4.1|Configurare le attestazioni dispositivo per i client|Impostazione di criteri di gruppo per abilitare le attestazioni dispositivo|  
|4.2|Abilitare una richiesta diritti per i dispositivi.|Abilitare il tipo di attestazione paese per i dispositivi.|  
|4.3|Aggiungere un criterio di gestione temporanea alla regola di accesso centrale esistente che si desidera modificare.|Modificare Finance Documents Rule per aggiungere un criterio di gestione temporanea.|  
|4.4|Visualizzare i risultati del criterio di gestione temporanea.|La verifica delle autorizzazioni di Ester Velle.|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Per impostare i criteri di gruppo di impostazione per abilitare attestazioni per i dispositivi  
  
1.  Accedere a DC1, aprire Gestione criteri di gruppo, fare clic su **contoso.com**, fare clic su **criterio dominio predefinito**del mouse e scegliere **modifica **.  
  
2.  Nella finestra Editor Gestione criteri di gruppo, passare a **configurazione Computer**, **criteri**, **modelli amministrativi**, **sistema**, **Kerberos **.  
  
3.  Selezionare **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos** e fare clic su **abilitare **.  
  
#### <a name="to-enable-a-claim-for-devices"></a>Per abilitare un'attestazione per i dispositivi  
  
1.  Aprire Server DC1 nella gestione di Hyper-V e accedere come contoso\Administrator, con la password **pass@word1**.  
  
2.  Dal **strumenti** menu, aprire Centro di amministrazione di Active Directory.  
  
3.  Fare clic su **visualizzazione albero**, espandere **controllo dinamico degli accessi**, fare doppio clic su **tipi di attestazione**, fare doppio clic su di **paese** attestazione.  
  
4.  In **attestazioni di questo tipo possono essere emessi per le classi seguenti**, selezionare il **Computer** casella di controllo. Fare clic su **OK**.   
    Entrambi **utente** e **Computer** dovrebbero essere selezionate le caselle di controllo. L'attestazione country può ora essere usato con i dispositivi, oltre agli utenti.  
  
Il passaggio successivo consiste nel creare una regola dei criteri di gestione temporanea. Criteri di gestione temporanea consente di monitorare gli effetti di una nuova voce di criterio prima di abilitarla. Nel passaggio seguente, si crea una voce di criterio di gestione temporanea e monitorare l'effetto sulla cartella condivisa.  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Per creare una regola dei criteri di gestione temporanea e aggiungerla al criterio di accesso centrale  
  
1.  Aprire Server DC1 nella gestione di Hyper-V e accedere come contoso\Administrator, con la password **pass@word1**.  
  
2.  Aprire Centro di amministrazione di Active Directory.  
  
3.  Fare clic su **visualizzazione albero**, espandere **controllo dinamico degli accessi**e seleziona **regole di accesso centrale **.  
  
4.  Fare doppio clic su **Finance Documents Rule**, quindi fare clic su **proprietà **.  
  
5.  Nel **autorizzazioni proposte** selezionare il **autorizzazione Abilita configurazione di gestione temporanea** casella di controllo, fare clic su **modifica**, quindi fare clic su **Aggiungi **. Nel **voce autorizzazione per autorizzazioni proposte** finestra, fare clic sul **seleziona un'entità** collegamento, digitare **Authenticated Users**, quindi fare clic su **OK **.  
  
6.  Fare clic su di **aggiungere una condizione** collegare e aggiungere la condizione seguente:   
     [**User**] [**paese**] [**Any of**] [**Risorse**] [**Paese**].  
  
7.  Fare clic su **aggiungere una condizione** nuovamente e aggiungere la condizione seguente:  
    [**And**]   
     [**Dispositivo**] [**paese**] [**Any of**] [**Risorse**] [**Paese**]  
  
8.  Fare clic su **aggiungere una condizione** nuovamente e aggiungere la condizione seguente.  
    [E]   
     [**User**] [**Group**] [**Membro di qualsiasi**] [**Valore**] \ (**FinanceException**)  
  
9. Per impostare il FinanceException, fare clic su **aggiungere elementi** e il **Seleziona utente, Computer, Account servizio o gruppo** finestra, digitare **FinanceException **.  
  
10. Fare clic su **autorizzazioni**selezionare **controllo completo**e fare clic su **OK **.  
  
11. Nelle impostazioni di sicurezza avanzate per autorizzazioni proposte selezionare **FinanceException** e fare clic su **rimuovere **.  
  
12. Fare clic su **OK** due volte per completare.  
  
![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> Nell'esempio precedente cmdlet, il valore Server riflette il Server nell'ambiente di laboratorio di test. È possibile utilizzare il Visualizzatore della cronologia di Windows PowerShell per cercare i cmdlet di Windows PowerShell per ogni procedura eseguita nel centro di amministrazione di Active Directory. Per ulteriori informazioni, vedere [Visualizzatore della cronologia di Windows PowerShell](https://technet.microsoft.com/library/hh831702)  
  
In questo set di autorizzazioni proposto i membri del gruppo FinanceException avrà accesso completo ai file del proprio paese quando accedono ai loro tramite un dispositivo dallo stesso paese il documento. Le voci di controllo sono disponibili nei server di File di registro di protezione quando un utente del reparto finanziario tenta di accedere ai file. Tuttavia, le impostazioni di sicurezza non vengono applicate fino a quando non la promozione del criterio dalla gestione temporanea.  
  
Nella procedura successiva, verificare i risultati del criterio di gestione temporanea. Accedere alla cartella condivisa con un nome utente che dispone delle autorizzazioni in base alla regola corrente. Esther Valle (EValle) è un membro del gruppo FinanceException e attualmente dispone di diritti di lettura. In base al criterio di gestione temporanea, EValle non deve avere i diritti.  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>Per verificare i risultati del criterio di gestione temporanea  
  
1.  Connettersi al File Server FILE1 nella gestione di Hyper-V e accedere come contoso\administrator, con la password **pass@word1**.  
  
2.  Aprire una finestra del prompt dei comandi e digitare **gpupdate /force **. Ciò garantisce che le modifiche di criteri di gruppo saranno applicate al server.  
  
3.  Nella console di gestione Hyper-V connettersi al server CLIENT1. Disconnettere l'utente attualmente connesso. Riavviare la macchina virtuale, CLIENT1. Accedere quindi al computer usando contoso\EValle pass@word1.  
  
4.  Fare doppio clic sul collegamento sul desktop ai documenti \\\FILE1\Finance. EValle dovrebbe disporre ancora accesso ai file. Tornare a FILE1.  
  
5.  Apri **Visualizzatore eventi** dal collegamento sul desktop. Espandere **registri di Windows**e quindi seleziona **sicurezza **. Aprire le voci con **Event ID 4818**sotto il **gestione temporanea criteri di accesso centrale** categoria di attività. Si noterà che EValle è stato concesso l'accesso. Tuttavia, in base al criterio di gestione temporaneo, l'utente sarebbe stato negato l'accesso.  
  
## <a name="next-steps"></a>Passaggi successivi  
Se si dispone di un sistema di gestione centrale di server, ad esempio System Center Operations Manager, puoi anche configurare il monitoraggio degli eventi. Ciò consente agli amministratori di monitorare gli effetti dei criteri di accesso centrale prima di applicarle.  
  

