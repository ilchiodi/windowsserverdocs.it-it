---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Distribuire la crittografia dei file di Office (procedura dimostrativa)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Distribuire la crittografia dei file di Office (procedura dimostrativa)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reparto finanziario di Contoso ha un numero di file server che archiviano i propri documenti. Questi documenti possono essere documentazione generale o possono avere un alto impatto aziendale (HBI). Ad esempio, tutti i documenti che contengono informazioni riservate sono classificati da Contoso hanno un alto impatto aziendale. Contoso desidera assicurare che tutta la documentazione abbia un minimo di protezione e che tutta la documentazione HBI sia limitata alle persone appropriate. A tale scopo, Contoso sta prendendo in considerazione utilizzando l'infrastruttura di classificazione File (FCI) e AD RMS, è disponibile in Windows Server 2012. Utilizzando l'infrastruttura di classificazione file, Contoso verrà classificare tutti i documenti archiviati nei file server, in base al contenuto e quindi usare AD RMS per applicare i criteri di diritti appropriati.  
  
In questo scenario si eseguirà la procedura seguente:  
  
|Attività|Descrizione|  
|--------|---------------|  
|[Abilitare le proprietà delle risorse](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Abilitare il **impatto** e **informazioni personali** le proprietà delle risorse.|  
|[Creare regole di classificazione](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Creare le regole di classificazione seguenti: **regola di classificazione HBI** e **regola di classificazione PII **.|  
|[Usare le attività di gestione file per proteggere automaticamente i documenti con AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Creare un'attività di gestione file che usa automaticamente AD RMS per proteggere i documenti che contengono informazioni personali (PII). Solo i membri del gruppo FinanceAdmin avranno accesso ai documenti che contengono high PII.|  
|[Visualizzare i risultati](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Esaminare la classificazione dei documenti e osservare come cambia mentre si modifica il contenuto del documento. Verificare inoltre come il protezione applicata al documento tramite AD RMS.|  
|[Verificare la protezione di AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Verificare che il documento sia protetto con AD RMS.|  
|||  
  
## <a name="BKMK_1.1"></a>Passaggio 1: Abilitare le proprietà delle risorse  
  
#### <a name="to-enable-resource-properties"></a>Per abilitare le proprietà delle risorse  
  
1.  Nella console di gestione Hyper-V connettersi al server ID_AD_DC1. Accedere al server usando Contoso\Administrator con la password **pass@word1**.  
  
2.  Centro di amministrazione aprire Active Directory e fare clic su **visualizzazione albero **.  
  
3.  Espandere **controllo dinamico degli accessi**e seleziona **le proprietà delle risorse **.  
  
4.  Scorrere verso il basso il **impatto** proprietà il **nome visualizzato** colonna. Fare doppio clic su **impatto**, quindi fare clic su **abilitare **.  
  
5.  Scorrere verso il basso il **informazioni personali** proprietà il **nome visualizzato** colonna. Fare doppio clic su **informazioni personali**, quindi fare clic su **abilitare **.  
  
6.  Per pubblicare le proprietà delle risorse nel **elenco risorse globali**, nel riquadro a sinistra, fare clic su **elenchi proprietà risorse**e quindi fare doppio clic su **elenco proprietà risorse globali **.  
  
7.  Fare clic su **Aggiungi**, quindi scorrere verso il basso e fare clic su **impatto** per aggiungerla all'elenco. Ripetere l'operazione per **informazioni personali **. Fare clic su **OK** due volte per terminare.  
  
![Guide alle soluzioni](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>Passaggio 2: Creare regole di classificazione  
Questo passaggio viene illustrato come creare il **High Impact** regola di classificazione. Questa regola cerca nel contenuto dei documenti e se viene trovata la stringa "Contoso Confidential", classifica il documento come ad alto impatto aziendale. Questa classificazione ha la precedenza qualsiasi classificazione di basso impatto aziendale assegnata in precedenza.  
  
Verrà creato anche un **High PII** regola. Questa regola cerca nel contenuto dei documenti e se viene trovato un codice fiscale, classifica il documento come high Pii.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Per creare la regola di classificazione high impact  
  
1.  Nella console di gestione Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password **pass@word1**.  
  
2.  È necessario aggiornare le proprietà delle risorse globali da Active Directory. Aprire Windows PowerShell e digitare: `Update-FSRMClassificationPropertyDefinition`, quindi premere INVIO. Chiudere Windows PowerShell.  
  
3.  Aprire Gestione risorse File Server. Per aprire Gestione risorse File Server, fare clic su **Start**, tipo **Gestione risorse file server**, quindi fare clic su **Gestione risorse File Server**.  
  
4.  Nel riquadro sinistro di gestione risorse File Server, espandere **gestione classificazioni**e quindi seleziona **le regole di classificazione **.  
  
5.  Nel **azioni** riquadro, fare clic su **configura pianificazione classificazione **. Nel **classificazione automatica** selezionare **Abilita pianificazione fissa**, selezionare un **giorno della settimana**e quindi seleziona il **Consenti classificazione continua per i nuovi file** casella di controllo. Fare clic su **OK**.  
  
6.  Nel **azioni** riquadro, fare clic su **Crea regola di classificazione **. Verrà visualizzata la **Crea regola di classificazione** la finestra di dialogo.  
  
7.  Nel **nome regola** digitare **High Business Impact **.  
  
8.  Nel **descrizione** digitare **determina se il documento ha un alto impatto aziendale in base alla presenza della stringa "Contoso Confidential"**  
  
9. Nel **ambito** scheda, fare clic su **imposta le proprietà di gestione cartelle**, selezionare **utilizzo cartella**, fare clic su **Aggiungi**, quindi fare clic su **Sfoglia**, passare a D:\Finance Documents come il percorso, fare clic su **OK**e quindi scegli un valore di proprietà denominato **file gruppo** e fare clic su **Chiudi **. Dopo aver impostate le proprietà di gestione, scegliere il **ambito della regola** scheda selezionare **file gruppo **.  
  
10. Fare clic su di **classificazione** scheda.  In **scegliere un metodo per assegnare la proprietà ai file**selezionare **classificazione del contenuto** dall'elenco a discesa.  
  
11. In **scegliere una proprietà da assegnare ai file**selezionare **impatto** dall'elenco a discesa.  
  
12. In **specificare un valore**selezionare **elevata** dall'elenco a discesa.  
  
13. Fare clic su **configurare** in **parametri **.  Nel **parametri di classificazione** della finestra di dialogo di **tipo espressione** elenco, selezionare **stringa **. Nel **espressione**, digitare: **Contoso Confidential**, quindi fare clic su **OK **.  
  
14. Fare clic su di **tipo valutazione** scheda.  Fare clic su **Rivaluta i valori di proprietà esistenti**, fare clic su **Sovrascrivi**il valore esistente e quindi fare clic su **OK** per terminare.  
  
![Guide alle soluzioni](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Per creare la regola di classificazione high PII  
  
1.  Nella console di gestione Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password **pass@word1**.  
  
2.  Sul desktop, aprire la cartella denominata **espressioni regolari**e quindi aprire il documento di testo denominato **RegEx-SSN **. Evidenziare e copiare la stringa di espressione regolare seguente: **^ (? 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4}$**. Questa stringa verrà usata più avanti in questo passaggio, tenerla negli Appunti.  
  
3.  Aprire Gestione risorse File Server. Per aprire Gestione risorse File Server, fare clic su **Start**, tipo **Gestione risorse file server**, quindi fare clic su **Gestione risorse File Server**.  
  
4.  Nel riquadro sinistro di gestione risorse File Server, espandere **gestione classificazioni**e quindi seleziona **le regole di classificazione **.  
  
5.  Nel **azioni** riquadro, fare clic su **configura pianificazione classificazione **. Nel **classificazione automatica** selezionare **Abilita pianificazione fissa**, selezionare un **giorno della settimana**e quindi seleziona il **Consenti classificazione continua per i nuovi file** casella di controllo. Fare clic su OK.  
  
6.  Nel **nome regola** digitare **High PII **. Nel **descrizione** digitare **determina se il documento ha un elevato informazioni personali in base alla presenza di un codice fiscale.**  
  
7.  Fare clic su di **ambito**, selezionare il **file gruppo** casella di controllo.  
  
8.  Fare clic su di **classificazione** scheda.  In **scegliere un metodo per assegnare la proprietà ai file**selezionare **classificazione del contenuto** dall'elenco a discesa.  
  
9. In **scegliere una proprietà da assegnare ai file**selezionare **informazioni personali** dall'elenco a discesa.  
  
10. In **specificare un valore**selezionare **elevata** dall'elenco a discesa.  
  
11. Fare clic su **configurare** in **parametri **.   
    Nel **parametri di classificazione**finestra, nel **tipo espressione** elenco, selezionare **espressione regolare **. Nel **espressione** incollare il testo dagli Appunti: **^ (? 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4}$**, quindi fare clic su **OK **.  
  
    > [!NOTE]  
    > Questa espressione consente di numeri di previdenza sociale non validi. Questo ci permette di usare codici fiscali fittizi nella dimostrazione.  
  
12. Fare clic su di **tipo valutazione** scheda.  Selezionare **Rivaluta i valori di proprietà esistenti**, **Sovrascrivi**il valore esistente e quindi fare clic su **OK** per terminare.  
  
![Guide alle soluzioni](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
Hai ora due regole di classificazione:  
  
-   Alto impatto aziendale  
  
-   High PII  
  
## <a name="BKMK_3"></a>Passaggio 3: Usare le attività di gestione file per proteggere automaticamente i documenti con AD RMS  
Ora che hai creato le regole per classificare automaticamente i documenti in base al contenuto, il passaggio successivo consiste nel creare un'attività di gestione file che usa AD RMS per proteggere automaticamente determinati documenti in base alla relativa classificazione. In questo passaggio si creerà un'attività di gestione file che protegge automaticamente i documenti classificati come high PII. Solo i membri del gruppo FinanceAdmin avranno accesso ai documenti che contengono high PII.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Per proteggere i documenti con AD RMS  
  
1.  Nella console di gestione Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password **pass@word1**.  
  
2.  Aprire Gestione risorse File Server. Per aprire Gestione risorse File Server, fare clic su **Start**, tipo **Gestione risorse file server**, quindi fare clic su **Gestione risorse File Server**.  
  
3.  Nel riquadro a sinistra, selezionare **attività di gestione File **. Nel **azioni** selezionare **Crea attività di gestione File **.  
  
4.  Nel **nome attività:** digitare **High PII **. Nel **descrizione** digitare **Automatic RMS protection per i documenti personali elevati **.  
  
5.  Fare clic su di **ambito**, selezionare il **file gruppo** casella di controllo.  
  
6.  Fare clic su di **azione** scheda. In **tipo**selezionare **crittografia RMS **. Fare clic su **Sfoglia** per selezionare un modello e quindi seleziona il **Contoso Finance Admin Only** modello.  
  
7.  Fare clic su di **condizione** scheda e quindi fare clic su **Aggiungi **. In **proprietà**selezionare **informazioni personali **. In **operatore**selezionare **uguale **. In **valore**selezionare **elevata **. Fare clic su **OK**.  
  
8.  Fare clic su di **pianificazione** scheda. Nel **pianificazione** fare clic su **settimanale**e quindi seleziona **domenica **. Esecuzione dell'attività una sola volta a settimana verrà assicura l'individuazione di eventuali documenti che potrebbero essere sfuggiti a causa di un'interruzione del servizio o di altri eventi.  
  
9. Nel **funzionamento continuo** selezionare **eseguire attività in modo continuo in nuovi file di**, quindi fare clic su **OK **. È ora un'attività di gestione file denominata High PII.  
  
![Guide alle soluzioni](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>Passaggio 4: Visualizzare i risultati  
È possibile osservare la nuova classificazione automatica e le regole di protezione di AD RMS in azione. In questo passaggio verrà esaminata la classificazione dei documenti e si osserverà come cambia mentre si modifica il contenuto del documento.  
  
#### <a name="to-view-the-results"></a>Per visualizzare i risultati  
  
1.  Nella console di gestione Hyper-V connettersi al server ID_AD_FILE1. Accedere al server usando Contoso\Administrator con la password **pass@word1**.  
  
2.  In Esplora risorse, passare a D:\Finance Documents.  
  
3.  Pulsante destro del mouse sul documento Finance Memo e fare clic su **proprietà**. Fare clic su di **classificazione** scheda e notare che alla proprietà impatto non ha attualmente alcun valore. Fare clic su **Annulla **.  
  
4.  Fare doppio clic su di **Request for Approval to Hire documento**e quindi seleziona **proprietà **.  
  
5.  Fare clic su di **classificazione** scheda e notare che **la proprietà di informazioni personali** attualmente non ha un valore. Fare clic su **Annulla **.  
  
6.  Passare a CLIENT1. Disconnettersi da qualsiasi utente che ha effettuato in e quindi accedere come Contoso\MReid con la password **pass@word1**.  
  
7.  Dal Desktop, aprire il **Finance Documents** cartella condivisa.  
  
8.  Aprire il **Finance Memo** documento. Nella parte inferiore del documento, si noterà la parola **Confidential **. Modificarla in: **Contoso Confidential **. Salvare il documento e chiuderlo.  
  
9. Aprire il **Request for Approval to Hire** documento. Nel **Social Security #:** sezione, digitare: 777-77-7777. Salvare il documento e chiuderlo.  
  
    > [!NOTE]  
    > Potrebbe essere necessario attendere 30 secondi per la classificazione avvenga.  
  
10. Tornare a ID_AD_FILE1. In Esplora risorse, passare a D:\Finance Documents.  
  
11. Pulsante destro del mouse sul documento Finance Memo e scegliere **proprietà **. Fare clic su di **classificazione** scheda. Si noti che il **impatto** proprietà è ora impostata su **elevata **. Fare clic su **Annulla **.  
  
12. Fare doppio clic sulla richiesta di approvazione documento Hire e fare clic su **proprietà **.  
  
13. . Fare clic su di **classificazione** scheda. Si noti che il **informazioni personali** proprietà è ora impostata su **elevata **. Fare clic su **Annulla **.  
  
## <a name="BKMK_5"></a>Passaggio 5: Verificare la protezione con AD RMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Per verificare che il documento sia protetto  
  
1.  Tornare a ID_AD_CLIENT1.  
  
2.  Aprire il **richiesta di approvazione per assunzione** documento.  
  
3.  Fare clic su **OK** per consentire il documento di connettersi al server AD RMS.  
  
4.  È ora possibile vedere che il documento è stato protetto con AD RMS poiché contiene un codice fiscale.  
  

