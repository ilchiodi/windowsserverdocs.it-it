---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Deploy Encryption of Office Files (Demonstration Steps)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 12b0e99651454930ee42a007dd34d1cafdb2e7fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861224"
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Deploy Encryption of Office Files (Demonstration Steps)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reparto finanziario di Contoso ha un numero di file server che archiviano i propri documenti. che spaziano dalla documentazione generale a documenti ad alto impatto aziendale (HBI). Ad esempio, i documenti che contengono informazioni riservate sono classificati da Contoso come documenti HBI. L'azienda vuole che tutta la documentazione abbia un minimo di protezione e che tutta la documentazione HBI sia accessibile solo dalle persone autorizzate. A tale scopo, Contoso sta prendendo in considerazione utilizzando l'infrastruttura di classificazione File (FCI) e AD RMS è disponibile in Windows Server 2012. Infrastruttura di classificazione file consentirà a Contoso di classificare tutti i documenti archiviati nei file server in base al contenuto, mentre AD RMS consentirà di applicare i criteri per i diritti di utilizzo appropriati.  
  
In questo scenario si eseguirà la procedura seguente:  
  
|Attività|Descrizione|  
|--------|---------------|  
|[Abilitare le proprietà delle risorse](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Abilitare le proprietà delle risorse **Impatto** e **Informazioni personali**.|  
|[Creare regole di classificazione](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Creare le regole di classificazione seguenti: **Regola di classificazione HBI** e **Regola di classificazione PII**.|  
|[Usare le attività di gestione file per proteggere automaticamente i documenti con AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Creare un'attività di gestione file che usa automaticamente AD RMS per proteggere i documenti che contengono informazioni personali (PII). Solo i membri del gruppo FinanceAdmin avranno accesso ai documenti classificati come High PII.|  
|[Visualizzare i risultati](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Esaminare la classificazione dei documenti e osservare come cambia in base alla modifica del contenuto. Verificare inoltre la protezione applicata al documento tramite AD RMS.|  
|[Verificare la protezione AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Verificare che il documento sia protetto con AD RMS.|  
|||  
  
## <a name="step-1-enable-resource-properties"></a><a name="BKMK_1.1"></a>Passaggio 1: abilitare le proprietà delle risorse  
  
#### <a name="to-enable-resource-properties"></a>Per abilitare le proprietà delle risorse  
  
1. Nella console di gestione di Hyper-V connettersi al server ID_AD_DC1. Accedere al server usando Contoso\Administrator con la password <strong>pass@word1</strong>.  
  
2. Aprire Centro di amministrazione di Active Directory e fare clic su **Visualizzazione albero**.  
  
3. Espandere **CONTROLLO DI ACCESSO DINAMICO** e selezionare **Proprietà risorse**.  
  
4. Scorrere fino alla proprietà **Impatto** nella colonna **Nome visualizzato**. Fare clic con il pulsante destro del mouse su **Impatto** e quindi scegliere **Abilita**.  
  
5. Scorrere fino alla proprietà **Informazioni personali** nella colonna **Nome visualizzato**. Fare clic con il pulsante destro del mouse su **Informazioni personali** e quindi scegliere **Abilita**.  
  
6. Per pubblicare le proprietà delle risorse in **Elenco risorse globali**, nel riquadro sinistro, fare clic su **Elenchi proprietà risorse** e quindi doppio clic su **Elenco proprietà risorse globali**.  
  
7. Fare clic su **Aggiungi**, scorrere fino alla proprietà **Impatto** e selezionarla per aggiungerla all'elenco. Ripetere l'operazione per **Informazioni personali**. Fare clic su **OK** due volte per completare la procedura.  
  
![la soluzione guida i](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="step-2-create-classification-rules"></a><a name="BKMK_2"></a>Passaggio 2: creare regole di classificazione  
Questo passaggio spiega come creare la regola di classificazione **High Impact**. Questa regola cerca nel contenuto dei documenti e se viene trovata la stringa "Contoso Confidential", classifica il documento come ad alto impatto aziendale. Questa classificazione ha la precedenza su un'eventuale classificazione di basso impatto aziendale assegnata in precedenza.  
  
In questo passaggio verrà creata anche una regola **High PII**. Questa regola cerca nel contenuto dei documenti e, se trova un codice fiscale, classifica il documento che lo contiene come High PII.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Per creare la regola di classificazione High Impact  
  
1. Nella console di gestione di Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password <strong>pass@word1</strong>.  
  
2. È necessario aggiornare le proprietà delle risorse globali da Active Directory. Aprire Windows PowerShell, digitare `Update-FSRMClassificationPropertyDefinition`e quindi premere INVIO. Chiudere Windows PowerShell.  
  
3. Aprire Gestione risorse file server. Per aprire Gestione risorse file server, fare clic su **Start**, digitare **gestione risorse file server** e quindi fare clic su **Gestione risorse file server**.  
  
4. Nel riquadro sinistro di Gestione risorse file server espandere **Gestione classificazioni** e quindi selezionare **Regole di classificazione**.  
  
5. Nel riquadro **Azioni** fare clic su **Configura pianificazione classificazione**. Nella scheda **Classificazione automatica** selezionare **Abilita pianificazione fissa**, selezionare un giorno in **Giorno della settimana**, quindi selezionare la casella di controllo **Consenti classificazione continua per i nuovi file**. Fare clic su **OK**.  
  
6. Nel riquadro **Azioni** fare clic su **Crea regola di classificazione**. Verrà visualizzata la finestra di dialogo **Crea regola di classificazione**.  
  
7. Nella casella **Nome regola** digitare **High Business Impact**.  
  
8. Nel **Descrizione** digitare **determina se il documento ha un alto impatto aziendale in base alla presenza della stringa "Contoso Confidential"**  
  
9. Nella scheda **Ambito** fare clic su **Imposta proprietà di gestione cartelle**, selezionare **Utilizzo cartelle**, fare clic su **Aggiungi** e quindi su **Sfoglia**, individuare il percorso D:\Finance Documents, fare clic su **OK**, scegliere un valore di proprietà denominato **File gruppo** e infine fare clic su **Chiudi**. Una volta impostate le proprietà di gestione, nella scheda **Ambito della regola** selezionare **File gruppo**.  
  
10. Fare clic sulla scheda **classificazione** .  In **scegliere un metodo per assegnare la proprietà ai file**selezionare **classificazione contenuto** nell'elenco a discesa.  
  
11. In **Scegliere una proprietà da assegnare ai file** selezionare **Impatto** nell'elenco a discesa.  
  
12. In **Specificare un valore** selezionare **Alto** nell'elenco a discesa.  
  
13. Fare clic su **Configura** in **Parametri**.  Nella finestra di dialogo **Parametri di classificazione** selezionare **Stringa** nell'elenco **Tipo espressione**. Nella casella **Espressione** digitare: **Contoso Confidential**, quindi fare clic su **OK**.  
  
14. Fare clic sulla scheda **tipo di valutazione** .  Fare clic su **Rivaluta i valori di proprietà esistenti**, fare clic su **Sovrascrivi**il valore esistente, quindi fare clic su **OK** per completare l'installazione.  
  
![la soluzione guida i](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Per creare la regola di classificazione High PII  
  
1. Nella console di gestione di Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password <strong>pass@word1</strong>.  
  
2. Sul desktop aprire la cartella denominata **Espressioni regolari**, quindi aprire il documento di testo denominato **RegEx-SSN**. Evidenziare e copiare la stringa di espressione regolare seguente: **^ (?! 000) ([0-7] \d{2}| 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d{4}$** . Poiché questa stringa verrà usata più avanti in questo passaggio, tenerla negli Appunti.  
  
3. Aprire Gestione risorse file server. Per aprire Gestione risorse file server, fare clic su **Start**, digitare **gestione risorse file server** e quindi fare clic su **Gestione risorse file server**.  
  
4. Nel riquadro sinistro di Gestione risorse file server espandere **Gestione classificazioni** e quindi selezionare **Regole di classificazione**.  
  
5. Nel riquadro **Azioni** fare clic su **Configura pianificazione classificazione**. Nella scheda **Classificazione automatica** selezionare **Abilita pianificazione fissa**, selezionare un giorno in **Giorno della settimana**, quindi selezionare la casella di controllo **Consenti classificazione continua per i nuovi file**. Fai clic su OK.  
  
6. Nella casella **Nome regola** digitare **High PII**. Nella casella **Descrizione** digitare **Determines if the document has a high PII based on the presence of a Social Security Number.**  
  
7. Fare clic sulla scheda **Ambito** e selezionare la casella di controllo **File gruppo**.  
  
8. Fare clic sulla scheda **classificazione** .  In **scegliere un metodo per assegnare la proprietà ai file**selezionare **classificazione contenuto** nell'elenco a discesa.  
  
9. In **Scegliere una proprietà da assegnare ai file** selezionare **Informazioni personali** nell'elenco a discesa.  
  
10. In **Specificare un valore** selezionare **Alto** nell'elenco a discesa.  
  
11. Fare clic su **Configura** in **Parametri**.   
    Nel riquadro **Parametri di classificazione**selezionare **Stringa** nell'elenco **Tipo espressione**. Nella casella **espressione** incollare il testo dagli Appunti: **^ (?! 000) ([0-7] \d{2}| 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d{4}$** , quindi fare clic su **OK**.  
  
    > [!NOTE]  
    > Questa espressione consente di immettere codici fiscali non validi. In questo modo sarà possibile usare codici fiscali fittizi nella dimostrazione.  
  
12. Fare clic sulla scheda **tipo di valutazione** .  Selezionare **Rivaluta i valori di proprietà esistenti**, **sovrascrivere**il valore esistente e quindi fare clic su **OK** per completare l'installazione.  
  
![la soluzione guida i](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
A questo punto dovrebbero essere presenti due regole di classificazione:  
  
-   High Business Impact  
  
-   High PII  
  
## <a name="step-3-use-file-management-tasks-to-automatically-protect-documents-with-ad-rms"></a><a name="BKMK_3"></a>Passaggio 3: usare le attività di gestione file per proteggere automaticamente i documenti con AD RMS  
Ora che avete creato le regole per classificare automaticamente i documenti in base al contenuto, il passaggio successivo consiste nel creare un'attività di gestione di file che usa AD RMS per proteggere automaticamente determinati documenti in base alla relativa classificazione. In questo passaggio verrà creata un'attività di gestione file che protegge automaticamente i documenti classificati come High PII. Solo i membri del gruppo FinanceAdmin avranno accesso ai documenti classificati come High PII.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Per proteggere i documenti con AD RMS  
  
1. Nella console di gestione di Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password <strong>pass@word1</strong>.  
  
2. Aprire Gestione risorse file server. Per aprire Gestione risorse file server, fare clic su **Start**, digitare **gestione risorse file server** e quindi fare clic su **Gestione risorse file server**.  
  
3. Nel riquadro sinistro selezionare **Attività di gestione file**. Nel riquadro **Azioni** selezionare **Crea attività di gestione file**.  
  
4. Nel campo **Nome attività** digitare **High PII**. Nel campo **Descrizione** digitare **Automatic RMS protection for high PII documents**.  
  
5. Fare clic sulla scheda **Ambito** e selezionare la casella di controllo **File gruppo**.  
  
6. Fare clic sulla scheda **azione** . In **tipo**selezionare **crittografia RMS**. Fare clic su **Sfoglia** per selezionare un modello, quindi selezionare il modello **Contoso Finance Admin Only**.  
  
7. Fare clic sulla scheda **Condizione** e quindi su **Aggiungi**. In **Proprietà** selezionare **Informazioni personali**. In **Operatore** selezionare **Uguale**. In **Valore** selezionare **Alto**. Fare clic su **OK**.  
  
8. Fare clic sulla scheda **pianificazione** . Nella sezione **pianificazione** fare clic su **settimanale**e quindi selezionare **domenica**. L'esecuzione settimanale dell'attività assicura l'individuazione di eventuali documenti che potrebbero essere sfuggiti a causa di un'interruzione del servizio o di altri eventi.  
  
9. Nella sezione **Esecuzione continua** selezionare **Esegui in modo continuo sui nuovi file**, quindi fare clic su **OK**. A questo punto dovrebbe essere presente un'attività di gestione file denominata High PII.  
  
![la soluzione guida i](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="step-4-view-the-results"></a><a name="BKMK_4"></a>Passaggio 4: visualizzare i risultati  
È necessario osservare la nuova classificazione automatica e regole di protezione di AD RMS in azione. In questo passaggio verrà esaminata la classificazione dei documenti e si osserverà come cambia in base alla modifica del contenuto.  
  
#### <a name="to-view-the-results"></a>Per visualizzare i risultati  
  
1. Nella console di gestione di Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password <strong>pass@word1</strong>.  
  
2. In Esplora risorse passare a D:\Finance Documents.  
  
3. Fare clic con il pulsante destro del mouse sul documento Finance Memo e scegliere **Proprietà**. Fare clic sulla scheda **Classificazione** e notare che alla proprietà Impatto non è assegnato alcun valore. Fare clic su **Annulla**.  
  
4. Fare clic con il pulsante destro del mouse sul documento **Request for Approval to Hire** e quindi scegliere **Proprietà**.  
  
5. Fare clic sulla scheda **Classificazione** e notare che alla proprietà **Informazioni personali** non è assegnato alcun valore. Fare clic su **Annulla**.  
  
6. Passare a CLIENT1. Disconnettere gli utenti che hanno eseguito l'accesso, quindi accedere come Contoso\MReid con la password <strong>pass@word1</strong>.  
  
7. Sul desktop aprire la cartella condivisa **Finance Documents**.  
  
8. Aprire il documento **Finance Memo**. Quasi in fondo al documento è presente la parola **Confidential**. Modificarla in: **Contoso Confidential**. Salvare il documento e chiuderlo.  
  
9. Aprire il documento **Request for Approval to Hire**. Nella sezione **Social Security#:** digitare: 777-77-7777. Salvare il documento e chiuderlo.  
  
    > [!NOTE]  
    > L'applicazione della classificazione può richiedere circa 30 secondi.  
  
10. Tornare a ID_AD_FILE1. In Esplora risorse passare a D:\Finance Documents.  
  
11. Fare clic con il pulsante destro del mouse sul documento Finance Memo e scegliere **Proprietà**. Fare clic sulla scheda **classificazione** . si noti che la proprietà **Impact** è ora impostata su **High**. Fare clic su **Annulla**.  
  
12. Fare clic con il pulsante destro del mouse sul documento Request for Approval to Hire e scegliere **Proprietà**.  
  
13. . Fare clic sulla scheda **classificazione** . si noti che la proprietà **informazioni** personali è ora impostata su **alto**. Fare clic su **Annulla**.  
  
## <a name="step-5-verify-protection-with-ad-rms"></a><a name="BKMK_5"></a>Passaggio 5: verificare la protezione con AD RMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Per verificare che il documento sia protetto  
  
1.  Tornare a ID_AD_CLIENT1.  
  
2.  Aprire il documento **Request for Approval to Hire**.  
  
3.  Fare clic su **OK** per consentire la connessione del documento al server AD RMS.  
  
4.  È ora possibile vedere che il documento è stato protetto con AD RMS poiché contiene un codice fiscale.  
  

