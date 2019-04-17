---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Distribuire la classificazione File automatica (passaggi dimostrativi)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c5c0fa221e0d7375216426f838ba37bee852984
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Distribuire la classificazione File automatica (passaggi dimostrativi)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come abilitare le proprietà delle risorse in Active Directory, creare regole di classificazione del file server e quindi assegnare valori alle proprietà delle risorse per i file nel file server. Per questo esempio, vengono create le regole di classificazione seguenti:  
  
-   Una regola di classificazione di contenuto che cerca un set di file per la stringa "Contoso Confidential". Se la stringa viene trovata in un file, la proprietà di risorsa impatto viene impostata su alto nel file.  
  
-   Una regola di classificazione di contenuto che cerca un set di file per un'espressione regolare che corrisponde a un codice fiscale almeno 10 volte in un unico file. Se viene trovato, il file viene classificato come contenente informazioni personali e la proprietà di risorsa informazioni personali è impostata su High.  
  
**In questo documento**  
  
-   [Passaggio 1: Creare risorse definizioni delle proprietà](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Passaggio 2: Creare una regola di classificazione del contenuto di stringa](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Passaggio 3: Creare una regola di classificazione del contenuto di espressione regolare](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Passaggio 4: Verificare che i file siano classificati](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Questo argomento include cmdlet di Windows PowerShell di esempio che è possibile utilizzare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Passaggio 1: Creare risorse definizioni delle proprietà  
Le proprietà delle risorse impatto e informazioni personali sono abilitate in modo che infrastruttura di classificazione File possa usare queste proprietà delle risorse per contrassegnare i file analizzati in una cartella condivisa di rete.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Per creare definizioni delle proprietà di risorsa  
  
1.  Nel controller di dominio, accedere al server come membro del gruppo di sicurezza Domain Admins.  
  
2.  Aprire Centro di amministrazione di Active Directory. In Server Manager, fare clic su **strumenti**, quindi fare clic su **centro di amministrazione di Active Directory**.  
  
3.  Espandere **controllo dinamico degli accessi**, quindi fare clic su **le proprietà delle risorse**.  
  
4.  Fare doppio clic su **impatto**, quindi fare clic su **abilitare **.  
  
5.  Fare doppio clic su **informazioni personali**, quindi fare clic su **abilitare **.  
  
![Guide alle soluzioni](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Passaggio 2: Creare una regola di classificazione del contenuto di stringa  
Una regola di classificazione del contenuto stringa Cerca un file per una stringa specifica. Se la stringa viene trovata, il valore di una proprietà di risorsa può essere configurato. In questo esempio, si verrà analizzato ogni file in una cartella condivisa di rete e cercare la stringa "Contoso Confidential". Se la stringa viene trovata, il file associato viene classificato come ad alto impatto aziendale.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Per creare una regola di classificazione del contenuto della stringa  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Dal prompt dei comandi di Windows PowerShell, digitare **Update-FsrmClassificationPropertyDefinition** e quindi premere INVIO. Questo comando sincronizzerà le definizioni delle proprietà create nel controller di dominio per il file server.  
  
3.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
4.  Espandere **gestione classificazioni**, fare doppio clic su **le regole di classificazione**, quindi fare clic su **configura pianificazione classificazione**.  
  
5.  Selezionare il **Abilita pianificazione fissa** casella di controllo, seleziona il **Consenti classificazione continua per i nuovi file** casella di controllo, scegliere un giorno della settimana per eseguire la classificazione e quindi fare clic su **OK**.  
  
6.  Fare doppio clic su **le regole di classificazione**, quindi fare clic su **Crea regola di classificazione**.  
  
7.  Nel **generale** nella scheda il **nome regola**, digitare un nome di regola, ad esempio **Contoso Confidential**.  
  
8.  Nel **ambito** scheda, fare clic su **Aggiungi**, quindi scegliere le cartelle da includere nella regola, ad esempio D:\Finance Documents.  
  
    > [!NOTE]  
    > È anche possibile scegliere uno spazio dei nomi dinamico per l'ambito. Per ulteriori informazioni sugli spazi dei nomi dinamico per le regole di classificazione, vedere [What's New in Gestione risorse File Server in Windows Server 2012 \[redirected\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Nel **classificazione**, configurare quanto segue:  
  
    -   Nel **scegliere un metodo per assegnare una proprietà ai file**, verificare che **classificazione del contenuto** sia selezionata.  
  
    -   Nel **scegliere una proprietà da assegnare ai file** fare clic su **impatto**.  
  
    -   Nel **specificare un valore** fare clic su **elevata**.  
  
10. Nel **parametri** fare clic su **configura**.  
  
11. Nel **tipo espressione** colonna, seleziona **stringa**.  
  
12. Nel **espressione** colonna, tipo **Contoso Confidential**, quindi fare clic su **OK**.  
  
13. Nel **tipo valutazione**, selezionare il **Rivaluta i valori di proprietà esistenti** casella di controllo, fare clic su **sovrascrivere il valore esistente**, quindi fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Passaggio 3: Creare una regola di classificazione del contenuto di espressione regolare  
Una regola di classificazione di espressione regolare Cerca un file per un modello che corrisponde all'espressione regolare. Se una stringa che corrisponde all'espressione regolare viene trovata, il valore di una proprietà di risorsa può essere configurato. In questo esempio verrà analizzato ogni file in una cartella condivisa di rete e cercare una stringa che corrisponde al modello di un codice fiscale (XXX-XX-XXXX). Se viene trovato, il file associato viene classificato come contenente informazioni personali.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Per creare una regola di classificazione del contenuto di espressione regolare  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Dal prompt dei comandi di Windows PowerShell, digitare **Update-FsrmClassificationPropertyDefinition**, quindi premere INVIO. Questo comando sincronizzerà le definizioni delle proprietà create nel controller di dominio per il file server.  
  
3.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
4.  Fare doppio clic su **le regole di classificazione**, quindi fare clic su **Crea regola di classificazione**.  
  
5.  Nel **generale** nella scheda il **nome regola**, digitare un nome per la regola di classificazione, ad esempio PII Rule.  
  
6.  Nel **ambito** scheda, fare clic su **Aggiungi**e quindi scegli le cartelle da includere nella regola, ad esempio D:\Finance Documents.  
  
7.  Nel **classificazione**, configurare quanto segue:  
  
    -   Nel **scegliere un metodo per assegnare una proprietà ai file**, verificare che **classificazione del contenuto** sia selezionata.  
  
    -   Nel **scegliere una proprietà da assegnare ai file** fare clic su **informazioni personali**.  
  
    -   Nel **specificare un valore** fare clic su **elevata**.  
  
8.  Nel **parametri** fare clic su **configura**.  
  
9. Nel **tipo espressione** colonna, seleziona **espressione regolare**.  
  
10. Nel **espressione** colonna, tipo **^ (? 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4}$**  
  
11. Nel **n. minimo occorrenze** colonna, tipo **10**, quindi fare clic su **OK**.  
  
12. Nel **tipo valutazione**, selezionare il **Rivaluta i valori di proprietà esistenti** casella di controllo, fare clic su **sovrascrivere il valore esistente**, quindi fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Passaggio 4: Verificare che i file siano classificati correttamente  
È possibile verificare che i file siano classificati correttamente visualizzando le proprietà di un file che è stato creato nella cartella specificata nelle regole di classificazione.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Per verificare che i file siano classificati correttamente  
  
1.  Nel file server, eseguire le regole di classificazione mediante Gestione risorse File Server.  
  
    1.  Fare clic su **gestione classificazioni**, fare doppio clic su **le regole di classificazione**, quindi fare clic su **Esegui classificazione con tutte le regole ora**.  
  
    2.  Fare clic su di **attendere completamento della classificazione** opzione, quindi fare clic su **OK**.  
  
    3.  Chiudere il rapporto classificazione automatica.  
  
    4.  È possibile farlo tramite Windows PowerShell con il comando seguente: **-FSRMClassification inizio ' "valore di RunDuration 0 - confermare: $false**  
  
2.  Passare alla cartella che è stata specificata nelle regole di classificazione, ad esempio D:\Finance Documents.  
  
3.  Fare doppio clic su un file in tale cartella e quindi fare clic su **proprietà**.  
  
4.  Fare clic su di **classificazione** scheda e verificare che il file sia classificato correttamente.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: Ottenere comprendere i dati mediante la classificazione](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Piano per la classificazione automatica dei File](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

