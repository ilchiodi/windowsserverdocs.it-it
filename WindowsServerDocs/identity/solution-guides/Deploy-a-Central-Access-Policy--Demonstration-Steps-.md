---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: Distribuire i criteri di accesso centrale (procedura dimostrativa)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817142"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>Distribuire i criteri di accesso centrale (procedura dimostrativa)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo scenario, i responsabili della sicurezza del reparto finanziario collaborano con i responsabili della sicurezza centrale delle informazioni per specificare l'esigenza di un criterio di accesso centrale, in modo da permettere di proteggere le informazioni finanziarie archiviate nei file server. Le informazioni finanziarie archiviate per ogni paese sono accessibili in sola lettura dai dipendenti del reparto finanziario dello stesso paese. Un gruppo di amministratori finanziari centrali può accedere alle informazioni finanziarie di tutti i paesi.  
  
La distribuzione di un criterio di accesso centrale include le fasi seguenti:  
  
|Fase|Descrizione  
|---------|---------------  
|[Piano: Identificare la necessità di criterio e la configurazione necessaria per la distribuzione](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|Identificare la necessità di un criterio e la configurazione necessaria per la distribuzione. 
|[Implementazione: Configurare i componenti e dei criteri](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|Configurare i componenti e i criteri.  
|[Distribuire il criterio di accesso centrale](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|Distribuire i criteri.  
|[Effettuare la manutenzione: Modificare e gestire in modo temporaneo il criterio](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|Modifiche ai criteri e gestione temporanea. 
  
## <a name="BKMK_1.1"></a>Configurare un ambiente di test  
Prima di iniziare, è necessario configurare un ambiente di testing per lo scenario. I passaggi per la configurazione del lab sono illustrati in dettaglio in [appendice b: Configurazione dell'ambiente di Test](Appendix-B--Setting-Up-the-Test-Environment.md).  
  
## <a name="BKMK_1.2"></a>Piano: identificare la necessità di un criterio e la configurazione necessaria per la distribuzione  
In questa sezione sono disponibili le procedure generali che semplificano la fase di pianificazione della distribuzione.  
  
||Passaggio|Esempio|  
|-|--------|-----------|  
|1.1|Un'azienda stabilisce che è necessario definire un criterio di accesso centrale|Per proteggere le informazioni finanziarie archiviate nei file server, i responsabili della sicurezza del reparto finanziario collaborano con i responsabili della sicurezza centrale delle informazioni per specificare l'esigenza di un criterio di accesso centrale.|  
|1.2|Esprimere il criterio di accesso|I documenti finanziari devono essere letti solo dai membri del reparto finanziario. I membri del reparto finanziario devono accedere solo ai documenti relativi al proprio paese. L'accesso in scrittura deve essere concesso solo agli amministratori del reparto finanziario. Sarà permessa un'eccezione per i membri del gruppo FinanceException. A questo gruppo sarà assegnato l'accesso in lettura.|  
|1.3|I criteri di accesso in costrutti di Windows Server 2012 Express|Destinazione:<br /><br />-Resource.Department contiene Finance<br /><br />Regole di accesso:<br /><br />-Consenti lettura User.Country=Resource.Country e User. Department = Resource.Department<br />-Consentire il controllo completo User.MemberOf(FinanceAdmin)<br /><br />Eccezione:<br /><br />Allow read memberOf(FinanceException)|  
|1.4|Determinare le proprietà di file necessarie per il criterio|Assegnare tag ai file con:<br /><br />-Department<br />-Paese|  
|1.5|Determinare i tipi di attestazioni e i gruppi necessari per i criteri|Tipi di attestazione:<br /><br />-Paese<br />-Department<br /><br />Gruppi di utenti:<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|Determinare i server a cui applicare il criterio|Applicare il criterio a tutti i file server del reparto finanziario.|  
  
## <a name="BKMK_1.3"></a>Implementazione: Configurare i componenti e i criteri  
In questa sezione è disponibile un esempio di distribuzione di un criterio di accesso centrale per documenti finanziari.  
  
|No|Passaggio|Esempio|  
|------|--------|-----------|  
|2.1|Creare tipi di attestazioni|Creare i tipi di attestazioni seguenti:<br /><br />-Department<br />-Paese|  
|2.2|Creare proprietà delle risorse|Creare e abilitare le proprietà seguenti delle risorse:<br /><br />-Department<br />-Paese|  
|2.3|Configurare una regola di accesso centrale|Creare una regola Finance Documents che include il criterio definito nella sezione precedente.|  
|2.4|Configurare un criterio di accesso centrale|Creare un criterio di accesso centrale denominato Finance Policy e aggiungervi la regola Finance Documents.|  
|2.5|Specificare i file server come destinazione del criterio di accesso centrale|Pubblicare il criterio di accesso centrale Finance Policy nei file server.|  
|2.6|Abilitare il supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos.|Abilitare il supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos per contoso.com.|  
  
Nella procedura seguente saranno creati due tipi di attestazioni: l'attestazione relativa al paese e quella relativa al reparto.  
  
#### <a name="to-create-claim-types"></a>Per creare tipi di attestazioni  
  
1.  Aprire Server DC1 nella console di gestione Hyper-V e accedere come contoso\administrator, con la password **pass@word1**.  
  
2.  Apri Centro di amministrazione di Active Directory.  
  
3.  Fare clic sull'**icona di visualizzazione albero**, espandere **Controllo dinamico degli accessi**, quindi selezionare **Tipi di attestazione**.  
  
    Fare clic con il pulsante destro del mouse su **Tipi di attestazione**, scegliere **Nuovo**,quindi fare clic su **Tipo attestazione**.  
  
    > [!TIP]  
    > È anche possibile aprire una finestra **Crea Tipo attestazione** dal riquadro **Attività**. Nel riquadro **Attività** fare clic su **Nuovo** e quindi su **Tipo attestazione**.  
  
4.  Nell'elenco **Attributo di origine** scorrere verso il basso l'elenco di attributi, quindi fare clic su **department**. Nel campo **Nome visualizzato** sarà inserito il valore **department**. Fare clic su **OK**.  
  
5.  Nel riquadro **Attività** fare clic su **Nuovo** e quindi su **Tipo attestazione**.  
  
6.  Nell'elenco **Attributo di origine** scorrere verso il basso l'elenco di attributi, quindi fare clic sull'attributo **(Country-Name).** Nel campo **Nome visualizzato** digitare **country**.  
  
7.  Nella sezione **Valori suggeriti** selezionare **Valori suggeriti**, quindi fare clic su **Aggiungi**.  
  
8.  Nei campi **Valore** e **Nome visualizzato** digitare **US**, quindi fare clic su **OK**.  
  
9. Ripetere il passaggio precedente. Nella finestra di dialogo **Aggiungi valore suggerito** digitare **JP** nei campi **Valore** e **Nome visualizzato**, quindi fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> È possibile usare il Visualizzatore della cronologia di Windows PowerShell nel Centro di amministrazione di Active Directory per cercare i cmdlet di Windows PowerShell per ogni procedura eseguita nel Centro di amministrazione di Active Directory. Per altre informazioni, vedere [Visualizzatore della cronologia di Windows PowerShell](https://technet.microsoft.com/library/hh831702).  
  
Il passaggio successivo consiste nel creare le proprietà delle risorse. Nella procedura seguente sarà creata una proprietà della risorsa che sarà aggiunta automaticamente all'elenco di proprietà di risorse globali nel controller di dominio, in modo da risultare disponibile per il file server.  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>Per creare e abilitare proprietà delle risorse create in precedenza  
  
1.  Nel riquadro sinistro del Centro di amministrazione di Active Directory fare clic su **Visualizzazione albero**. Espandere **Controllo dinamico degli accessi**, quindi selezionare **Proprietà risorsa**.  
  
2.  Fare clic con il pulsante destro del mouse su **Proprietà risorsa**, scegliere **Nuovo**, quindi fare clic su **Proprietà risorsa di riferimento**.  
  
    > [!TIP]  
    > È possibile scegliere anche una proprietà di risorsa dal riquadro **Attività**. Fare clic su **Nuovo**, quindi su **Proprietà risorsa di riferimento**.  
  
3.  In **Selezionare un tipo di attestazione di cui condividere i valori suggeriti** fare clic su **country**.  
  
4.  Nel campo **Nome visualizzato** digitare **country**, quindi fare clic su **OK**.  
  
5.  Fare doppio clic sull'elenco **Proprietà risorsa**, quindi scorrere verso il basso fino alla proprietà **Department** della risorsa. Fare clic con il pulsante destro del mouse, quindi scegliere **Abilita**. La proprietà predefinita **Department** della risorsa sarà abilitata.  
  
6.  Nell'elenco **Proprietà risorsa** nel pannello di navigazione del Centro di amministrazione di Active Directory saranno disponibili due proprietà abilitate delle risorse:  
  
    -   Country  
  
    -   Reparto  
  
![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
Il passaggio successivo consiste nel creare regole di accesso centrale per definire gli utenti autorizzati ad accedere alle risorse. Di seguito sono riportate le regola di business di questo scenario:  
  
-   I documenti finanziari possono essere letti solo dai membri del reparto finanziario.  
  
-   I membri del reparto finanziario possono accedere solo ai documenti relativi al proprio paese.  
  
-   L'accesso in scrittura può essere concesso solo agli amministratori del reparto finanziario.  
  
-   Sarà permessa un'eccezione per i membri del gruppo FinanceException. A questo gruppo sarà assegnato l'accesso in lettura.  
  
-   L'amministratore e il proprietario del documento disporranno comunque di accesso completo.  
  
O per esprimere le regole con costrutti di Windows Server 2012:  
  
Destinazione: Resource.Department Contains Finance  
  
Regole di accesso:  
  
-   Allow Read User.Country=Resource.Country AND User.department = Resource.Department  
  
-   Allow Full control User.MemberOf(FinanceAdmin)  
  
-   Allow Read User.MemberOf(FinanceException)  
  
#### <a name="to-create-a-central-access-rule"></a>Per creare una regola di accesso centrale  
  
1.  Nel riquadro sinistro del Centro di amministrazione di Active Directory fare clic su **Visualizzazione albero**, selezionare **Controllo dinamico degli accessi**, quindi fare clic su **Regole di accesso centrale**.  
  
2.  Fare clic con il pulsante destro del mouse su **Regole di accesso centrale**, scegliere **Nuovo**, quindi fare clic su **Regola di accesso centrale**.  
  
3.  Nel campo **Nome** digitare **Finance Documents Rule**.  
  
4.  Nella sezione **Risorse di destinazione** fare clic su **Modifica** e nella finestra di dialogo **Regola di accesso centrale** fare clic su **Aggiungi condizione**. Aggiungere la condizione seguente:   
    [**Resource**] [**Department**] [**Equals**] [**Value**] [**Finance**], quindi fare clic su **OK**.  
  
5.  Nella sezione **Autorizzazioni** selezionare **Usa queste autorizzazioni come correnti**, quindi fare clic su **Modifica** e nella finestra di dialogo **Impostazioni di sicurezza avanzate per Autorizzazioni** fare clic su **Aggiungi**.  
  
    > [!NOTE]  
    > **L'opzione** Usa queste autorizzazioni come proposte permette di creare il criterio in gestione temporanea. Per altre informazioni su questa procedura, vedere la sezione Gestione: Modificare e gestire in modo temporaneo il criterio.  
  
6.  Nella finestra di dialogo **Voce autorizzazione per Autorizzazioni** fare clic su **Seleziona un'entità**, digitare **Authenticated Users**, quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Voce autorizzazione per Autorizzazioni** fare clic su **Aggiungi condizione**, quindi aggiungere le condizioni seguenti:   
    [**Utente**] [**country**] [**uno qualsiasi dei**] [**risorsa**] [**paese**]   
     Fai clic su **Aggiungi condizione**.   
     [**And**]   
    Fare clic su [**utente**] [**reparto**] [**uno qualsiasi dei**] [**risorsa**] [**reparto**]. Impostare le **Autorizzazioni** su **Lettura**.  
  
8.  Fare clic su **OK** e quindi su **Aggiungi**. Fare clic su **Seleziona un'entità**, digitare **FinanceAdmin**, quindi fare clic su **OK**.  
  
9. Selezionare le autorizzazioni **Modifica, Lettura/esecuzione, Lettura, Scrittura** e quindi fare clic su **OK**.  
  
10. Fare clic su **Aggiungi**, quindi su **Seleziona un'entità**, digitare **FinanceException** e infine fare clic su **OK**. Selezionare le autorizzazioni da impostare su **Lettura** e **Lettura/esecuzione**.  
  
11. Fare tre volte clic su **OK** per completare la procedura, quindi tornare al Centro di amministrazione di Active Directory.  
  
    ![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
    Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
 
    $countryClaimType = Get-ADClaimType country  
    $departmentClaimType = Get-ADClaimType department  
    $countryResourceProperty = Get-ADResourceProperty Country  
    $departmentResourceProperty = Get-ADResourceProperty Department  
    $currentAcl = "O:SYG:SYD:AR(A;; FA;; O W) (A; FA;; BA) (A; 0 x1200a9;; S-1-5-21-1787166779-1215870801-2157059049-1113) (A; 0 x1301bf;; S-1-5-21-1787166779-1215870801-2157059049-1112) (A; FA;; SY) (XA; 0 x1200a9;; AU ;((@USER. " + $countryClaimType.Name + " Any_of @RESOURCE." + $countryResourceProperty.Name + ") && (@USER." + $departmentClaimType.Name + " Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
    $resourceCondition = "(@RESOURCE." $departmentResourceProperty.Name + + "contiene {`"Finance`"}) "  
    Nuovo ADCentralAccessRule "Finance Documents Rule" - CurrentAcl $currentAcl - ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> Nel cmdlet di esempio precedente gli ID di sicurezza (SID, Security Identifier) per il gruppo FinanceAdmin e gli utenti sono determinati al momento della creazione e saranno diversi nell'esempio eseguito in computer diversi. Ad esempio, il valore SID specificato (S-1-5-21-1787166779-1215870801-2157059049-1113) per FinanceAdmins deve essere sostituito con il valore SID effettivo per il gruppo FinanceAdmin da creare nella distribuzione in uso. È possibile utilizzare Windows PowerShell per cercare il valore di SID del gruppo, assegnare tale valore a una variabile e quindi usare la variabile di seguito. Per altre informazioni, vedere [suggerimento per Windows PowerShell: Uso dei SID](https://go.microsoft.com/fwlink/?LinkId=253545).  
  
Dovrebbe essere ora disponibile una regola di accesso centrale che permette agli utenti di accedere a documenti dello stesso paese e dello stesso reparto. La regola permette al gruppo FinanceAdmin di modificare i documenti e al gruppo FinanceException di leggere i documenti. Questa regola è relativa solo ai documenti classificati come Finance.  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>Per aggiungere una regola di accesso centrale a un criterio di accesso centrale  
  
1.  Nel riquadro sinistro del Centro di amministrazione di Active Directory fare clic su **Controllo dinamico degli accessi**, quindi su **Criteri di accesso centrale**.  
  
2.  Nel riquadro **Attività** fare clic su **Nuovo**, quindi su **Criterio di accesso centrale**.  
  
3.  In **Crea criteri di accesso centrale** digitare **Finance Policy** nella casella **Nome**.  
  
4.  In **Regole di accesso centrale membri** fare clic su **Aggiungi**.  
  
5.  Fare doppio clic su **Finance Documents Rule** per aggiungerla all'elenco **Aggiungere le regole di accesso centrale seguenti**, quindi fare clic su **OK**.  
  
6.  Fare clic su **OK** per completare la procedura. Dovrebbe essere disponibile un criterio di accesso centrale denominato Finance Policy.  
  
    ![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
    Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>Per applicare il criterio di accesso centrale nei file server usando Criteri di gruppo  
  
1.  Nella schermata **Start** digitare **Gestione criteri di gruppo** nella casella **Cerca**. Fare doppio clic su **Gestione Criteri di gruppo**.  
  
    > [!TIP]  
    > Se l'impostazione **Mostra strumenti di amministrazione** è disabilitata, la cartella **Strumenti di amministrazione** e il relativo contenuto non verranno visualizzati nei risultati di **Impostazioni**.  
  
    > [!TIP]  
    > Nell'ambiente di produzione creare un'unità organizzativa File Server e aggiungervi tutti i file server a cui applicare il criterio. È quindi possibile creare un criterio di gruppo e aggiungervi questa unità organizzativa.  
  
2.  In questo passaggio si modifica l'oggetto Criteri di gruppo creato nella sezione relativa alla [creazione del controller di dominio](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build) nell'ambiente di testing per includere il criterio di accesso centrale creato. Nell'Editor Gestione Criteri di gruppo passare all'unità organizzativa nel dominio e selezionarla (in questo esempio, contoso.com): **Gestione Criteri di gruppo**, **Foresta: contoso.com**, **Domini**, **contoso.com**, **Contoso**, **FileServerOU**.  
  
3.  Fare clic con il pulsante destro del mouse su **FlexibleAccessGPO**, quindi scegliere **Modifica**.  
  
4.  Nella finestra dell'Editor Gestione Criteri di gruppo passare a **Configurazione computer**, espandere **Criteri**, espandere **Impostazioni di Windows**, quindi fare clic su **Impostazioni sicurezza**.  
  
5.  Espandere **File system**, fare clic con il pulsante destro del mouse su **Central Access Policy** e quindi scegliere **Gestisci criteri di accesso centrale**.  
  
6.  Nella finestra di dialogo **Configurazione criteri di accesso centrale** aggiungere **Finance Policy**, quindi fare clic su **OK**.  
  
7.  Scorrere verso il basso fino a **Configurazione avanzata dei criteri di controllo** ed espanderlo.  
  
8.  Espandere **Criteri di controllo** e selezionare **Accesso agli oggetti**.  
  
9. Fare doppio clic su **Controlla Gestione temporanea Criteri di accesso centrale**. Selezionare tutte e tre le caselle di controllo, quindi fare clic su **OK**. Questo passaggio permette al sistema di ricevere eventi di controllo correlati a Gestione temporanea Criteri di accesso.  
  
10. Fare doppio clic su **Controllo Proprietà File system**. Selezionare tutte e tre le caselle di controllo, quindi fare clic su **OK**.  
  
11. Chiudi l'Editor Gestione Criteri di gruppo. Il criterio di accesso centrale è stato incluso in Criteri di gruppo.  
  
Per i controller di dominio di un dominio forniscano attestazioni o dati di autorizzazione dispositivo, i controller di dominio devono essere configurata per supportare controllo dinamico degli accessi.  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>Per abilitare il supporto per le attestazioni e l'autenticazione composta per contoso.com  
  
1.  Aprire Gestione criteri di gruppo, fare clic su **contoso.com**, quindi su **Controller di dominio**.  
  
2.  Fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefiniti**, quindi scegliere **Modifica**.  
  
3.  Nella finestra Editor Gestione Criteri di gruppo fare doppio clic su **Configurazione computer**, quindi su **Criteri**, **Modelli amministrativi**, **Sistema** e infine su **KDC**.  
  
4.  Fare doppio clic su **Supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos**. Nella finestra di dialogo **Supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos** fare clic su **Abilitato**, quindi scegliere **Supportato** dall'elenco a discesa **Opzioni**. È necessario abilitare questa impostazione per usare le attestazioni utente nei criteri di accesso centrale.  
  
5.  Chiudere **Gestione Criteri di gruppo**.  
  
6.  Aprire un prompt dei comandi e digitare `gpupdate /force`.  
  
## <a name="BKMK_1.4"></a>Distribuire il criterio di accesso centrale  
  
||Passaggio|Esempio|  
|-|--------|-----------|  
|3.1|Assegnare il criterio di accesso centrale alle cartelle condivise appropriate sul file server.|Assegnare il criterio di accesso centrale alla cartella condivisa appropriata sul file server.|  
|3.2|Verificare che l'accesso sia stato configurato correttamente.|Verificare l'accesso per utenti di diversi paesi e reparti.|  
  
In questo passaggio il criterio di accesso centrale sarà assegnato a un file server. Si eseguirà l'accesso a un file server che riceve il criterio di accesso centrale creato nei passaggi precedenti e si assegnerà il criterio a una cartella condivisa.  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>Per assegnare un criterio di accesso centrale a un file server  
  
1.  Nella Console di gestione di Hyper-V connettersi al server FILE1. Accedere al server usando contoso\administrator con la password: **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati e digitare: **gpupdate /force**. In questo modo, le modifiche a Criteri di gruppo saranno applicate al server.  
  
3.  È anche necessario aggiornare le proprietà delle risorse globali da Active Directory. Aprire una finestra di Windows PowerShell con privilegi elevati e digitare `Update-FSRMClassificationpropertyDefinition`. Fare clic su INVIO, quindi chiudere Windows PowerShell.  
  
    > [!TIP]  
    > È anche possibile aggiornare le proprietà delle risorse globali accedendo al file server. Per aggiornare le proprietà delle risorse globali dal file server, eseguire la procedura seguente:  
    >   
    > 1.  Accedere al File Server FILE1 come contoso\administrator, usando la password **pass@word1**.  
    > 2.  Aprire Gestione risorse file server. Per aprire Gestione risorse file server, fare clic su **Start**, digitare **gestione risorse file server**e quindi fare clic su **Gestione risorse file server**.  
    > 3.  In Gestione risorse file server fare clic su **Gestione classificazione file** , fare clic con il pulsante destro del mouse su **Proprietà classificazione** e quindi scegliere **Aggiorna**.  
  
4.  Aprire Esplora risorse e nel riquadro sinistro fare clic sull'unità D. Fare clic con il pulsante destro del mouse sulla cartella **Finance Documents**, quindi scegliere **Proprietà**.  
  
5.  Fare clic sulla scheda **Classificazione**, quindi su **Country**, e infine selezionare **US** nel campo **Valore**.  
  
6.  Fare clic su **Department**, quindi selezionare **Finance** nel campo **Valore** e infine fare clic su **Applica**.  
  
    > [!NOTE]  
    > Come si potrà ricordare, il criterio di accesso centrale è stato configurato in modo da interessare i file per il reparto finanziario. I passaggi precedenti permettono di contrassegnare tutti i documenti nella cartella con gli attributi Country e Department.  
  
7.  Fare clic sulla scheda **Sicurezza** e quindi su **Avanzate**. Fare clic sulla scheda **Criteri centrali**.  
  
8.  Fare clic su **Modifica**, selezionare **Finance Policy** dal menu a discesa, quindi fare clic su **Applica**. Nel criterio è disponibile la regola **Finance Documents Rule**. Espandere l'elemento per visualizzare tutte le autorizzazioni impostate durante la creazione della regola in Active Directory.  
  
9. Fare clic su **OK** per tornare a Esplora risorse.  
  
Nel passaggio successivo, assicurarsi che l'acceso sia configurato correttamente.  L'attributo Department appropriato deve essere impostato negli account utente. Eseguire questa operazione nel Centro di amministrazione di Active Directory. Il modo più semplice per verificare i risultati effettivi del nuovo criterio consiste nell'usare la scheda **Accesso valido** in Esplora risorse. Nella scheda **Accesso valido** sono mostrati i diritti di accesso per un determinato account utente.  
  
#### <a name="to-examine-the-access-for-various-users"></a>Per esaminare l'accesso per i diversi utenti  
  
1.  Nella Console di gestione di Hyper-V connettersi al server FILE1. Accedere al server usando contoso\administrator. Passare a D:\ in Esplora risorse. Fare clic con il pulsante destro del mouse sulla cartella **Finance Documents**, quindi scegliere **Proprietà**.  
  
2.  Fare clic sulla scheda **Sicurezza**, quindi su **Avanzate** e infine su **Accesso valido**.  
  
3.  Per esaminare le autorizzazioni per un utente, fare clic su **selezionare un utente**, digitare il nome dell'utente e quindi fare clic su **Visualizza accesso valido** per visualizzare i diritti di accesso valido. Ad esempio:  
  
    -   Myriam Delesalle (MDelesalle) lavora nel reparto finanziario e deve disporre di accesso in lettura alla cartella.  
  
    -   Miles Reid (MReid) è un membro del gruppo FinanceAdmin e deve disporre di accesso di tipo Modifica alla cartella.  
  
    -   Esther Valle (EValle) non lavora nel reparto finanziario, ma è un membro del gruppo FinanceException e deve disporre di accesso in lettura.  
  
    -   Maira Wenzel (MWenzel) non lavora nel reparto finanziario e non è un membro dei gruppi FinanceAdmin o FinanceException. Non deve disporre di accesso alla cartella.  
  
    Si noti l'ultima colonna denominata **Accesso limitato da** nella finestra relativa all'accesso valido. Questa colonna indica gate che compromettono le autorizzazioni dell'utente. In questo caso, le autorizzazioni Condivisione e NTFS permettono agli utenti di disporre di controllo completo. Il criterio di accesso centrale, tuttavia, limita l'accesso in base alle regole configurate in precedenza.  
  
## <a name="BKMK_1.5"></a>Effettuare la manutenzione: Modificare e gestire in modo temporaneo il criterio  
  
||||  
|-|-|-|  
|Numero|Passaggio|Esempio|  
|4.1|Configurare le richieste diritti da dispositivo per i client|Configurare le impostazioni di Criteri di gruppo per abilitare le richieste diritti da dispositivo|  
|4.2|Abilitare una richiesta diritti per i dispositivi.|Abilitare la richiesta diritti di tipo country per i dispositivi.|  
|4.3|Aggiungere un criterio di gestione temporanea alla regola di accesso centrale esistente da modificare.|Modificare la regola Finance Documents Rule per aggiungere un criterio di gestione temporanea.|  
|4.4|Visualizzare i risultati del criterio di gestione temporanea.|Controllare le autorizzazioni di Ester Velle.|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>Per configurare le impostazioni di Criteri di gruppo per abilitare le richieste diritti per i dispositivi  
  
1.  Accedere a DC1, aprire Gestione Criteri di gruppo, fare clic su **contoso.com**, quindi su **Criterio dominio predefinito**, infine fare clic con il pulsante destro del mouse e scegliere **Modifica**.  
  
2.  Nella finestra Editor Gestione Criteri di gruppo passare a **Configurazione computer**, **Criteri**, **Modelli amministrativi**, **Sistema**, **Kerberos**.  
  
3.  Selezionare **Supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos**, quindi fare clic su **Abilita**.  
  
#### <a name="to-enable-a-claim-for-devices"></a>Per abilitare una richiesta diritti per i dispositivi  
  
1.  Aprire Server DC1 nella console di gestione Hyper-V e accedere come contoso\Administrator, con la password **pass@word1**.  
  
2.  Dal menu **Strumenti** aprire il Centro di amministrazione di Active Directory.  
  
3.  Fare clic su **Visualizzazione albero**, espandere **Controllo dinamico degli accessi**, fare doppio clic su **Tipi di attestazione** e infine fare doppio clic sull'attestazione **country**.  
  
4.  In **Le attestazioni di questo tipo possono essere emesse per le classi seguenti** selezionare la casella di controllo **Computer**. Fare clic su **OK**.   
    Entrambe le caselle di controllo **User** e **Computer** dovrebbero essere selezionate. È ora possibile usare l'attestazione country con i dispositivi, oltre che con gli utenti.  
  
Il passaggio successivo consiste nel creare un criterio di gestione temporanea. I criteri di gestione temporanea possono essere usati per monitorare gli effetti di una nuova voce di criterio prima di abilitarla. Nel passaggio successivo sarà creata una voce di criterio di gestione temporanea e ne saranno monitorati gli effetti sulla cartella condivisa.  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>Per creare una regola di criterio di gestione temporanea e aggiungerla al criterio di accesso centrale  
  
1.  Aprire Server DC1 nella console di gestione Hyper-V e accedere come contoso\Administrator, con la password **pass@word1**.  
  
2.  Apri Centro di amministrazione di Active Directory.  
  
3.  Fare clic su **Visualizzazione albero**, espandere **Controllo dinamico degli accessi**, quindi selezionare **Regole di accesso centrale**.  
  
4.  Fare clic con il pulsante destro del mouse su **Finance Documents Rule**, quindi scegliere **Proprietà**.  
  
5.  Nella sezione **Autorizzazioni proposte** selezionare la casella di controllo **Abilita configurazione di gestione temporanea delle autorizzazioni**, quindi fare clic su **Modifica** e infine su **Aggiungi**. Nella finestra di dialogo **Voce autorizzazione per Autorizzazioni proposte** fare clic sul collegamento **Seleziona un'entità**, digitare **Authenticated Users**, quindi fare clic su **OK**.  
  
6.  Fare clic sul collegamento **Aggiungi condizione** e aggiungere la condizione seguente:   
     [**User**] [**country**] [**Any of**] [**Resource**] [**Country**].  
  
7.  Fare di nuovo clic su **Aggiungi condizione** e aggiungere la condizione seguente:  
    [**And**]   
     [**Device**] [**country**] [**Any of**] [**Resource**] [**Country**]  
  
8.  Fare di nuovo clic su **Aggiungi condizione** e aggiungere la condizione seguente.  
    [E]   
     [**Utente**] [**gruppo**] [**membro di alcun**] [**valore**]\(**FinanceException**)  
  
9. Per configurare il gruppo FinanceException, fare clic su **Aggiungi elementi** e nella finestra **Seleziona utente, computer, account del servizio o gruppo** digitare **FinanceException**.  
  
10. Fare clic su **Autorizzazioni**, selezionare **Controllo completo** e infine fare clic su **OK**.  
  
11. Nella finestra Impostazioni di sicurezza avanzate per Autorizzazioni proposte selezionare **FinanceException** e fare clic su **Rimuovi**.  
  
12. Fare due volte clic su **OK** per completare la procedura.  
  
![Guide alle soluzioni](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> Nell'esempio di cmdlet precedente il valore Server riflette il Server nell'ambiente di testing. È possibile usare il Visualizzatore della cronologia di Windows PowerShell per cercare i cmdlet di Windows PowerShell per ogni procedura eseguita nel Centro di amministrazione di Active Directory. Per altre informazioni, vedere [Visualizzatore della cronologia di Windows PowerShell](https://technet.microsoft.com/library/hh831702).  
  
Nel set di autorizzazioni proposto i membri del gruppo FinanceException disporranno di Accesso completo ai file del proprio paese quando vi accedono tramite un dispositivo dallo stesso paese in cui si trova il documento. Le voci di controllo sono disponibili nel registro di protezione dei file server quando un utente del reparto finanziario prova ad accedere ai file. Le impostazioni di sicurezza, tuttavia, saranno applicate solo dopo la promozione del criterio dalla gestione temporanea.  
  
Nella procedura seguente saranno verificati i risultati del criterio di gestione temporanea. Accedere alla cartella condivisa con un nome utente che dispone di autorizzazioni in base alla regola corrente. Esther Valle (EValle) è un membro del gruppo FinanceException e attualmente dispone di diritti di lettura. In base al criterio di gestione temporanea, EValle non deve disporre di alcun diritto.  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>Per verificare i risultati del criterio di gestione temporanea  
  
1.  Connettersi al File Server FILE1 nella console di gestione Hyper-V e accedere come contoso\administrator, con la password **pass@word1**.  
  
2.  Aprire una finestra del prompt dei comandi e digitare **gpupdate /force**. In questo modo, le modifiche a Criteri di gruppo saranno applicate al server.  
  
3.  Nella Console di gestione di Hyper-V connettersi al server CLIENT1. Disconnettere l'utente attualmente connesso. Riavviare la macchina virtuale, CLIENT1. Accedere quindi al computer usando contoso\EValle pass@word1.  
  
4.  Fare doppio clic sul collegamento sul desktop a \\\FILE1\Finance documenti. EValle dovrebbe disporre ancora di accesso ai file. Tornare a FILE1.  
  
5.  Aprire il **Visualizzatore eventi** dal collegamento sul desktop. Espandere **Registri di Windows**, quindi selezionare **Sicurezza**. Aprire le voci con **Event ID 4818**sotto il **gestione temporanea criteri di accesso centrale** categoria attività. Come si può notare, a EValle è stato concesso l'accesso. In base al criterio di gestione temporanea, tuttavia, questo utente non dovrebbe essere autorizzato all'accesso.  
  
## <a name="next-steps"></a>Passaggi successivi  
Se si usa un sistema di gestione centrale dei server, ad esempio System Center Operations Manager, sarà anche possibile configurare il monitoraggio degli eventi. Ciò permette agli amministratori di monitorare gli effetti dei criteri di accesso centrale prima di applicarli.  
  

