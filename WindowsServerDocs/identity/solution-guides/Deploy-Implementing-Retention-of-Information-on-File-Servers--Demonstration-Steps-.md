---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Deploy Implementing Retention of Information on File Servers (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 994eadfa205b62c5a512ab130c71fa6c22d1cff6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357537"
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Deploy Implementing Retention of Information on File Servers (Demonstration Steps)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile impostare periodi di memorizzazione per le cartelle e applicare ai file il blocco a fini giudiziari usando Infrastruttura di classificazione file e Gestione risorse file server.  
  
**Contenuto del documento**  
  
-   [Prerequisiti](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Passaggio 1: Creare definizioni di proprietà delle risorse @ no__t-0  
  
-   [Passaggio 2: Configurare le notifiche @ no__t-0  
  
-   [Passaggio 3: Creare un'attività di gestione file @ no__t-0  
  
-   [Passaggio 4: Classificare manualmente un file @ no__t-0  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Prerequisiti  
Per i passaggi in questo argomento si presuppone che sia disponibile un server SMTP configurato per notifiche relative alla scadenza di file.  
  
## <a name="BKMK_Step1"></a>Passaggio 1: Creare definizioni delle proprietà delle risorse  
In questo passaggio si abilita il periodo di memorizzazione e le proprietà di esposizione al rilevamento delle risorse, in modo che Infrastruttura di classificazione file possa usare queste proprietà delle risorse per assegnare tag ai file analizzati in una cartella di rete condivisa.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Per creare definizioni delle proprietà delle risorse  
  
1.  Nel controller di dominio accedere al server come membro del gruppo di sicurezza Domain Admins.  
  
2.  Apri Centro di amministrazione di Active Directory. In Server Manager fare clic su **Strumenti** e quindi su **Centro di amministrazione di Active Directory**.  
  
3.  Espandere **Controllo di accesso dinamico** e quindi fare clic su **Proprietà risorse**.  
  
4.  Fare clic con il pulsante destro del mouse su **Periodo di conservazione**, quindi scegliere **Abilita**.  
  
5.  Fare clic con il pulsante destro del mouse su **Esposizione al rilevamento**, quindi scegliere **Abilita**.  
  
![solution guide i](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Passaggio 2: Configurare le notifiche  
In questo passaggio si usa Gestione risorse file server per configurare il server SMTP, l'indirizzo di posta elettronica predefinito dell'amministratore e l'indirizzo di posta elettronica predefinito da cui inviare i rapporti.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Per configurare le notifiche  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Al prompt dei comandi di Windows PowerShell digitare **Update-FsrmClassificationPropertyDefinition** e quindi premere INVIO. Le definizioni delle proprietà create nel controller di dominio saranno sincronizzate con il file server.  
  
3.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
4.  Fare clic con il pulsante destro del mouse su **Gestione risorse file server (locale)** , quindi scegliere **Configura opzioni**.  
  
5.  Nella scheda **Notifiche posta elettronica** configurare gli elementi seguenti:  
  
    -   Nella casella **Nome server SMTP o indirizzo IP** digitare il nome del server SMTP della rete.  
  
    -   Nella casella **Destinatari amministratori predefiniti** digitare l'indirizzo di posta elettronica dell'amministratore che deve ricevere le notifiche.  
  
    -   Nel **"Da" indirizzo di posta elettronica predefinito** digitare l'indirizzo di posta elettronica che deve essere utilizzato per inviare le notifiche.  
  
6.  Fare clic su **OK**.  
  
![solution guide i](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Passaggio 3: Creare un'attività di gestione dei file  
In questo passaggio si usa la console di Gestione risorse file server per creare un'attività di gestione file che sarà eseguita l'ultimo giorno del mese e farà scadere tutti i file che soddisfano i criteri seguenti:  
  
-   Il file non è classificato come sottoposto a blocco a fini giudiziari.  
  
-   Il file è classificato come associato a un periodo di memorizzazione a lungo termine.  
  
-   Il file non è stato modificato negli ultimi 10 anni.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Per creare un'attività di gestione dei file  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
3.  Fare clic con il pulsante destro del mouse su **Attività di gestione file**, quindi scegliere **Crea attività di gestione file**.  
  
4.  Nella scheda **Generale** digitare nella casella **Nome attività** un nome per l'attività di gestione dei file, ad esempio Attività di memorizzazione.  
  
5.  Nella scheda **Ambito** fare clic su **Aggiungi**, quindi scegliere le cartelle da includere nella regola, ad esempio D:\Finance Documents.  
  
6.  Nella scheda **Azione** fare clic su **Scadenza file** nella casella **Tipo**. Nella casella **Directory file in scadenza** digitare un percorso per una cartella nel file server locale in cui saranno spostati i file scaduti. La cartella deve disporre di un elenco di controllo di accesso che concede l'accesso ai soli amministratori del file server.  
  
7.  Nella scheda **Notifica** fare clic su **Aggiungi**.  
  
    -   Selezionare la casella di controllo **Invia posta elettronica agli amministratori seguenti**.  
  
    -   Selezionare la casella di controllo **Invia messaggio di posta elettronica all'utente i cui file sono interessati** , quindi fare clic su **OK**.  
  
8.  Nella scheda **Condizione** fare clic su **Aggiungi**, quindi aggiungere le proprietà seguenti:  
  
    -   Nell'elenco **Proprietà** fare clic su **Esposizione al rilevamento**. Nell'elenco **Operatore** fare clic su **Diverso da**. Nell'elenco **Valore** fare clic su **Blocco**.  
  
    -   Nell'elenco **Proprietà** fare clic su **Periodo di conservazione**. Nell'elenco **Operatore** fare clic su **Uguale a**. Nell'elenco **Valore** fare clic su **Lungo termine**.  
  
9. Nella scheda **Condizione** selezionare la casella di controllo **N. di giorni dall'ultima modifica al file** , quindi impostare il valore su **3650**.  
  
10. Nella scheda **Pianificazione** fare clic sull'opzione **Mensile** , quindi selezionare la casella di controllo **Ultimo** .  
  
11. Fare clic su **OK**.  
  
![solution guide i](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
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
In questo passaggio si classifica manualmente un file da sottoporre a blocco a fini giudiziari. La cartella padre di questo file sarà classificata con un periodo di memorizzazione a lungo termine.  
  
#### <a name="to-manually-classify-a-file"></a>Per classificare manualmente un file  
  
1.  Accedere al file server come membro del gruppo di sicurezza Administrators.  
  
2.  Passare alla cartella configurata nell'ambito dell'attività di gestione file creata nel Passaggio 3.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella e scegliere **Proprietà**.  
  
4.  Nella scheda **Classificazione** fare clic su **Periodo di conservazione**, quindi su **Lungo termine**e infine su **OK**.  
  
5.  Fare clic con il pulsante destro del mouse su un file in tale cartella e scegliere **Proprietà**.  
  
6.  Nella scheda **Classificazione** fare clic su **Esposizione al rilevamento**, quindi su **Blocco**, **Applica**e infine su **OK**.  
  
7.  Nel file server eseguire l'attività di gestione file usando la console di Gestione risorse file server. Al termine dell'attività di gestione file, controllare la cartella e assicurarsi che il file non sia stato spostato nella directory di scadenza.  
  
8.  Fare clic con il pulsante destro del mouse sullo stesso file nella cartella e scegliere **Proprietà**.  
  
9. Nella scheda **Classificazione** fare clic su **Esposizione al rilevamento**, quindi su **Non applicabile**, **Applica**e infine su **OK**.  
  
10. Nel file server eseguire di nuovo l'attività di gestione file usando la console di Gestione risorse file server. Al termine dell'attività di gestione file, controllare la cartella e assicurarsi che il file sia stato spostato nella directory di scadenza.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: Implementare la conservazione delle informazioni nei file server](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Pianificare la conservazione delle informazioni nei file server](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

