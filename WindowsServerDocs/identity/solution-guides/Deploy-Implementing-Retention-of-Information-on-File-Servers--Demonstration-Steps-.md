---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Distribuire l'implementazione della conservazione delle informazioni nei File server (passaggi dimostrativi)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Distribuire l'implementazione della conservazione delle informazioni nei File server (passaggi dimostrativi)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile impostare periodi di memorizzazione per le cartelle e file giudiziari usando infrastruttura di classificazione File e Gestione risorse File Server.  
  
**In questo documento**  
  
-   [Prerequisiti](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Passaggio 1: Creare risorse definizioni delle proprietà](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Passaggio 2: Configurare le notifiche](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [Passaggio 3: Creare un'attività di gestione file](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Passaggio 4: Classificare manualmente un file](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Questo argomento include cmdlet di Windows PowerShell di esempio che è possibile utilizzare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Prerequisiti  
I passaggi descritti in questo argomento si presuppongono che dispone di un server SMTP configurato per le notifiche di scadenza file.  
  
## <a name="BKMK_Step1"></a>Passaggio 1: Creare risorse definizioni delle proprietà  
In questo passaggio, abbiamo abilitare le proprietà della risorsa periodo di conservazione e l'esposizione al rilevamento, in modo che infrastruttura di classificazione File possa usare queste proprietà delle risorse per contrassegnare i file analizzati in una cartella condivisa di rete.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Per creare definizioni delle proprietà di risorsa  
  
1.  Nel controller di dominio, accedere al server come membro del gruppo di sicurezza Domain Admins.  
  
2.  Aprire Centro di amministrazione di Active Directory. In Server Manager, fare clic su **strumenti**, quindi fare clic su **centro di amministrazione di Active Directory**.  
  
3.  Espandere **controllo dinamico degli accessi**, quindi fare clic su **le proprietà delle risorse**.  
  
4.  Fare doppio clic su **periodo di memorizzazione**, quindi fare clic su **abilitare**.  
  
5.  Fare doppio clic su **esposizione al rilevamento**, quindi fare clic su **abilitare**.  
  
![Guide alle soluzioni](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Passaggio 2: Configurare le notifiche  
In questo passaggio, usiamo la console di gestione risorse File Server per configurare il server SMTP, l'indirizzo di posta elettronica amministratore predefinito e l'indirizzo di posta elettronica predefinito che i report vengono inviati dal.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Per configurare le notifiche  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Dal prompt dei comandi di Windows PowerShell, digitare **Update-FsrmClassificationPropertyDefinition**, quindi premere INVIO. Questo comando sincronizzerà le definizioni delle proprietà create nel controller di dominio per il file server.  
  
3.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
4.  Fare doppio clic su **Gestione risorse File Server (locale)**, quindi fare clic su **Configura opzioni**.  
  
5.  Nel **notifiche di posta elettronica**, configurare quanto segue:  
  
    -   Nel **nome server SMTP o indirizzo IP**, digitare il nome del server SMTP della rete.  
  
    -   Nel **destinatari amministratori predefiniti** casella, digitare l'indirizzo di posta elettronica dell'amministratore che deve ricevere la notifica.  
  
    -   Nel **"indirizzo di posta elettronica da" predefinito**, digitare l'indirizzo di posta elettronica che deve essere utilizzato per inviare le notifiche.  
  
6.  Fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Passaggio 3: Creare un'attività di gestione file  
In questo passaggio è utilizzare la console di gestione risorse File Server per creare un'attività di gestione file che verrà eseguito l'ultimo giorno del mese e scadenza qualsiasi file con i criteri seguenti:  
  
-   Il file non viene classificato come sottoposto a giudiziari.  
  
-   Il file viene classificato come contenente un periodo di memorizzazione a lungo termine.  
  
-   Il file non è stato modificato negli ultimi 10 anni.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Per creare un'attività di gestione file  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
3.  Fare doppio clic su **attività di gestione File**, quindi fare clic su **Crea attività di gestione File**.  
  
4.  Nel **generale** nella scheda il **nome attività**, digitare un nome per l'attività di gestione file, ad esempio attività di memorizzazione.  
  
5.  Nel **ambito** scheda, fare clic su **Aggiungi**, quindi scegliere le cartelle da includere nella regola, ad esempio D:\Finance Documents.  
  
6.  Nel **azione** nella scheda il **tipo** fare clic su **File scadenza**. Nel **directory di scadenza**, digitare un percorso a una cartella nel server di file locale in cui verranno spostati i file scaduti. Tale cartella deve presentare un elenco di controllo di accesso che concede l'accesso solo agli amministratori di file server.  
  
7.  Nel **notifica** scheda, fare clic su **Aggiungi**.  
  
    -   Selezionare il **invia posta elettronica agli amministratori seguenti** casella di controllo.  
  
    -   Selezionare il **inviare un messaggio di posta elettronica agli utenti con file interessati** casella di controllo, quindi fare clic su **OK**.  
  
8.  Nel **condizione** scheda, fare clic su **Aggiungi**e aggiungere le seguenti proprietà:  
  
    -   Nel **proprietà** elenco, fare clic su **esposizione al rilevamento**. Nel **operatore** elenco, fare clic su **non uguale**. Nel **valore** elenco, fare clic su **premuto**.  
  
    -   Nel **proprietà** elenco, fare clic su **periodo di memorizzazione**. Nel **operatore** elenco, fare clic su **uguale**. Nel **valore** elenco, fare clic su **a lungo termine**.  
  
9. Nel **condizione**, selezionare il **giorni dall'ultima modifica file** casella di controllo e quindi impostare il valore su **3650**.  
  
10. Nel **pianificazione** scheda, fare clic su di **mensile** opzione e quindi seleziona il **ultimo** casella di controllo.  
  
11. Fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
$fmjexpiration = New-FSRMFmjAction -Type 'Expiration' -ExpirationFolder folder  
$fmjNotificationAction = New-FsrmFmjNotificationAction -Type Email -MailTo "[FileOwner],[AdminEmail]"  
$fmjNotification = New-FsrmFMJNotification -Days 10 -Action @($fmjNotificationAction)  
$fmjCondition1 = New-FSRMFmjCondition -Property 'Discoverability_MS' -Condition 'NotEqual' -Value "Hold" 
$fmjCondition2 = New-FSRMFmjCondition -Property 'RetentionPeriod_MS' -Condition 'Equal' -Value "Long-term"  
$fmjCondition3 = New-FSRMFmjCondition -Property 'File.DateLastAccessed' -Condition 'Equal' -Value 3650  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Monthly @(-1)    
$fmj1=New-FSRMFileManagementJob -Name "Retention Task" -Namespace @('D:\Finance Documents') -Action $fmjexpiration -Schedule $schedule -Notification @($fmjNotification) -Condition @( $fmjCondition1, $fmjCondition2, $fmjCondition3)  
```  
  
## <a name="BKMK_Step4"></a>Passaggio 4: Classificare manualmente un file  
In questo passaggio si classifica manualmente un file da sottoporre giudiziari. La cartella padre di questo file sarà classificata con un periodo di memorizzazione a lungo termine.  
  
#### <a name="to-manually-classify-a-file"></a>Per classificare manualmente un file  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Passare alla cartella in cui è stata configurata nell'ambito dell'attività di gestione file creata nel passaggio 3.  
  
3.  Fare clic sulla cartella e quindi fare clic su **proprietà**.  
  
4.  Nel **classificazione** scheda, fare clic su **periodo di memorizzazione**, fare clic su **a lungo termine**, quindi fare clic su **OK**.  
  
5.  Fare doppio clic su un file in tale cartella e quindi fare clic su **proprietà**.  
  
6.  Nel **classificazione** scheda, fare clic su **esposizione al rilevamento**, fare clic su **premuto**, fare clic su **applica**, quindi fare clic su **OK**.  
  
7.  Nel file server, eseguire l'attività di gestione di file tramite la console di gestione risorse File Server. Al termine dell'attività di gestione file, controllare la cartella e assicurarsi che il file non è stato spostato nella directory di scadenza.  
  
8.  Fare doppio clic sullo stesso file all'interno di tale cartella e quindi fare clic su **proprietà**.  
  
9. Nel **classificazione** scheda, fare clic su **esposizione al rilevamento**, fare clic su **non applicabile**, fare clic su **applica**, quindi fare clic su **OK**.  
  
10. Nel file server, eseguire di nuovo l'attività di gestione di file tramite la console di gestione risorse File Server. Al termine dell'attività di gestione file, controllare la cartella e assicurarsi di che tale file è stato spostato nella directory di scadenza.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: Implementare la conservazione delle informazioni nei File server](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Pianificare la conservazione delle informazioni nei File server](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

