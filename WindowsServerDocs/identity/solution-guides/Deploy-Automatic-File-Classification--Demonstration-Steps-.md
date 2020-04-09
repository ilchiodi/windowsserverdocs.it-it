---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Deploy Automatic File Classification (Demonstration Steps)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cc89e97aacab3b764df7314beeab701df846048a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861254"
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Deploy Automatic File Classification (Demonstration Steps)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento spiega come abilitare le proprietà delle risorse in Active Directory, creare regole di classificazione sul file server e quindi assegnare valori alle proprietà delle risorse per i file sul file server. Per questo esempio verranno create le regole di classificazione seguenti:  
  
-   Una regola di classificazione del contenuto che cerca un set di file per la stringa "Contoso Confidential". Se la stringa viene trovata in un file, la proprietà della risorsa Impatto viene impostata su Alto nel file.  
  
-   Una regola di classificazione del contenuto che cerca in un gruppo di file un'espressione regolare che corrisponde a un codice fiscale almeno 10 volte in un file. Se l'espressione regolare viene trovata, il file viene classificato come contenente informazioni personali e la proprietà della risorsa Informazioni personali viene impostata su Alto.  
  
**Contenuto del documento**  
  
-   [Passaggio 1: creare definizioni delle proprietà delle risorse](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Passaggio 2: creare una regola di classificazione del contenuto stringa](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Passaggio 3: creare una regola di classificazione del contenuto di un'espressione regolare](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Passaggio 4: verificare che i file siano classificati](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> In questo argomento sono inclusi cmdlet di Windows PowerShell di esempio che possono essere usati per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="step-1-create-resource-property-definitions"></a><a name="BKMK_Step1"></a>Passaggio 1: creare definizioni delle proprietà delle risorse  
Le proprietà delle risorse Impatto e Informazioni personali vengono abilitate in modo che Infrastruttura di classificazione file possa usarle per contrassegnare i file analizzati in una cartella di rete condivisa.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Per creare definizioni delle proprietà delle risorse  
  
1.  Nel controller di dominio accedere al server come membro del gruppo di sicurezza Domain Admins.  
  
2.  Apri Centro di amministrazione di Active Directory. In Server Manager fare clic su **Strumenti** e quindi su **Centro di amministrazione di Active Directory**.  
  
3.  Espandere **Controllo di accesso dinamico** e quindi fare clic su **Proprietà risorse**.  
  
4.  Fare clic con il pulsante destro del mouse su **Impatto** e quindi scegliere **Abilita**.  
  
5.  Fare clic con il pulsante destro del mouse su **Informazioni personali** e quindi scegliere **Abilita**.  
  
![la soluzione guida i](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="step-2-create-a-string-content-classification-rule"></a><a name="BKMK_Step2"></a>Passaggio 2: creare una regola di classificazione del contenuto stringa  
Una regola di classificazione di contenuto di tipo stringa cerca una stringa specifica in un file. Se la stringa viene trovata, il valore di una proprietà della risorsa può essere configurato. In questo esempio, si verrà analizzato ogni file in una cartella condivisa di rete e cercare la stringa "Contoso Confidential". Se la stringa viene trovata, il file associato viene classificato come ad alto impatto aziendale.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Per creare una regola di classificazione di contenuto di tipo stringa  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Al prompt dei comandi di Windows PowerShell digitare **Update-FsrmClassificationPropertyDefinition** e quindi premere INVIO. Questo comando sincronizzerà le definizioni delle proprietà create nel controller di dominio con il file server.  
  
3.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
4.  Espandere **Gestione classificazioni**, fare clic con il pulsante destro del mouse su **Regole di classificazione** e quindi scegliere **Configura pianificazione classificazione**.  
  
5.  Selezionare le caselle di controllo **Abilita pianificazione fissa** e **Consenti classificazione continua per i nuovi file**, scegliere un giorno della settimana in cui eseguire la classificazione e quindi fare clic su **OK**.  
  
6.  Fare clic con il pulsante destro del mouse su **Regole eseguibili**, quindi scegliere **Crea regola di classificazione**.  
  
7.  Nella scheda **Generale** digitare un nome nella casella **Nome regola**, ad esempio **Contoso Confidential**.  
  
8.  Nella scheda **Ambito** fare clic su **Aggiungi**, quindi scegliere le cartelle da includere nella regola, ad esempio D:\Finance Documents.  
  
    > [!NOTE]  
    > Come ambito si può anche scegliere uno spazio dei nomi dinamico. Per ulteriori informazioni sugli spazi dei nomi dinamici per le regole di classificazione, vedere [What's New in Gestione risorse File Server in Windows Server 2012 \[reindirizzato\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Nella scheda **Classificazione** eseguire le configurazioni seguenti:  
  
    -   Nella casella **Scegliere un metodo per l'assegnazione di una proprietà ai file** verificare che l'opzione **Classificazione contenuto** sia selezionata.  
  
    -   Nella casella **Scegliere una proprietà da assegnare ai file** fare clic su **Impatto**.  
  
    -   Nella casella **Specificare un valore** fare clic su **Alto**.  
  
10. In **Parametri** fare clic su **Configura**.  
  
11. Nella colonna **Tipo espressione** selezionare **Stringa**.  
  
12. Nella colonna **Espressione** digitare **Contoso Confidential** e fare clic su **OK**.  
  
13. Nella scheda **Tipo valutazione** selezionare la casella di controllo **Rivaluta i valori di proprietà esistenti**, quindi fare clic su **Sovrascrivi il valore esistente** e infine su **OK**.  
  
![la soluzione guida i](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="step-3-create-a-regular-expression-content-classification-rule"></a><a name="BKMK_Step3"></a>Passaggio 3: creare una regola di classificazione del contenuto di un'espressione regolare  
Una regola di classificazione di contenuto di tipo espressione regolare cerca in un file una stringa corrispondente all'espressione regolare. Se una stringa di questo tipo viene trovata, il valore di una proprietà della risorsa può essere configurato. In questo esempio verrà analizzato ogni file contenuto in una cartella condivisa alla ricerca di una stringa corrispondente allo schema di un codice fiscale (XXX-XX-XXXX). Se lo schema viene trovato, il file associato viene classificato come contenente informazioni personali.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Per creare una regola di classificazione di contenuto di tipo espressione regolare  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Al prompt dei comandi di Windows PowerShell digitare **Update-FsrmClassificationPropertyDefinition** e quindi premere INVIO. Le definizioni delle proprietà create nel controller di dominio saranno sincronizzate con il file server.  
  
3.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
4.  Fare clic con il pulsante destro del mouse su **Regole eseguibili**, quindi scegliere **Crea regola di classificazione**.  
  
5.  Nella scheda **Generale** digitare un nome per la regola di classificazione nella casella **Nome regola**, ad esempio PII Rule.  
  
6.  Nella scheda **Ambito** fare clic su **Aggiungi**, quindi scegliere le cartelle da includere nella regola, ad esempio D:\Finance Documents.  
  
7.  Nella scheda **Classificazione** eseguire le configurazioni seguenti:  
  
    -   Nella casella **Scegliere un metodo per l'assegnazione di una proprietà ai file** verificare che l'opzione **Classificazione contenuto** sia selezionata.  
  
    -   Nella casella **Scegliere una proprietà da assegnare ai file** fare clic su **Informazioni personali**.  
  
    -   Nella casella **Specificare un valore** fare clic su **Alto**.  
  
8.  In **Parametri** fare clic su **Configura**.  
  
9. Nella colonna **Tipo espressione** selezionare **Espressione regolare**.  
  
10. Nella colonna **espressione** Digitare **^ (?! 000) ([0-7] \d{2}| 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d{4}$**  
  
11. Nella colonna **N. minimo occorrenze** digitare **10** e quindi fare clic su **OK**.  
  
12. Nella scheda **Tipo valutazione** selezionare la casella di controllo **Rivaluta i valori di proprietà esistenti**, quindi fare clic su **Sovrascrivi il valore esistente** e infine su **OK**.  
  
![la soluzione guida i](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="step-4-verify-that-the-files-are-classified-correctly"></a><a name="BKMK_Step4"></a>Passaggio 4: verificare che i file siano classificati correttamente  
È possibile verificare che i file siano classificati correttamente visualizzando le proprietà di un file creato nella cartella specificata nelle regole di classificazione.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Per verificare che i file siano classificati correttamente  
  
1.  Nel file server eseguire le regole di classificazione mediante Gestione risorse file server.  
  
    1.  Fare clic su **Gestione classificazioni**, fare clic con il pulsante destro del mouse su **Regole di classificazione** e quindi scegliere **Esegui classificazione con tutte le regole**.  
  
    2.  Fare clic su **Attendi il completamento della classificazione** e quindi su **OK**.  
  
    3.  Chiudere il Rapporto classificazione automatica.  
  
    4.  È possibile farlo tramite Windows PowerShell con il comando seguente: **FSRMClassification inizio ' "valore di RunDuration 0 - confermare: $false**  
  
2.  Passare alla cartella specificata nelle regole di classificazione, ad esempio D:\Finance Documents.  
  
3.  Fare clic con il pulsante destro del mouse su un file in tale cartella e scegliere **Proprietà**.  
  
4.  Fare clic sulla scheda **Classificazione** e verificare che il file sia classificato correttamente.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: ottenere informazioni dettagliate sui dati tramite la classificazione](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Pianificare la classificazione automatica dei file](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/jj574209(v%3dws.11))  

  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

