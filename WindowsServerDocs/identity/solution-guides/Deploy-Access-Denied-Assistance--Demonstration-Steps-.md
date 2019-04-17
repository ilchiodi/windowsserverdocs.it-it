---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Distribuire l'assistenza per accesso negato (passaggi dimostrativi)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5201441ba884fe4658b917919e60c7d20530341b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Distribuire l'assistenza per accesso negato (passaggi dimostrativi)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come configurare l'assistenza per accesso negato e verificare che funzioni correttamente.  
  
**In questo documento**  
  
-   [Passaggio 1: Configurare l'assistenza per accesso negato](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Passaggio 2: Configurare le impostazioni delle notifiche di posta elettronica](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Passaggio 3: Verificare che l'assistenza per accesso negato sia configurato correttamente](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Questo argomento include cmdlet di Windows PowerShell di esempio che è possibile utilizzare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Passaggio 1: Configurare l'assistenza per accesso negato  
È possibile configurare l'assistenza per accesso negato all'interno di un dominio utilizzando criteri di gruppo, o configurare l'assistenza singolarmente in ogni file server mediante la console di gestione risorse File Server. È inoltre possibile modificare il messaggio di accesso negato per una cartella condivisa specifica in un file server.  
  
È possibile configurare l'assistenza per accesso negato per il dominio utilizzando criteri di gruppo come segue:  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Per configurare l'assistenza per accesso negato tramite criteri di gruppo  
  
1.  Gestione di criteri di gruppo aprire. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo**.  
  
2.  Fare doppio clic su criteri di gruppo appropriato, quindi fare clic su **modifica**.  
  
3.  Fare clic su **configurazione Computer**, fare clic su **criteri**, fare clic su **modelli amministrativi**, fare clic su **sistema**, quindi fare clic su **assistenza per accesso negato**.  
  
4.  Fare doppio clic su **Configura messaggi per errori di accesso negato**, quindi fare clic su **modifica**.  
  
5.  Selezionare il **abilitato** opzione.  
  
6.  Configurare le opzioni seguenti:  
  
    1.  Nel **Visualizza il messaggio seguente agli utenti vengono negati l'accesso**, digitare un messaggio che gli utenti vedranno quando non hanno accesso a un file o cartella.  
  
        È possibile aggiungere macro al messaggio per inserirà testo personalizzato. Le macro includono:  
  
        -   **[Original File Path] **Il percorso del file originale che ha effettuato l'accesso dell'utente.  
  
        -   **[Original File Path Folder] **Cartella padre del percorso del file originale che ha effettuato l'accesso dell'utente.  
  
        -   **[Admin Email] **Dell'elenco di destinatari di posta elettronica di amministratore.  
  
        -   **[Data Owner Email] **L'elenco dei destinatari di posta elettronica dati di proprietario.  
  
    2.  Selezionare il **consentire agli utenti di richiedere assistenza** casella di controllo.  
  
    3.  Lasciare le impostazioni predefinite rimanenti.  
  
![Guide alle soluzioni](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName AllowEmailRequests -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName GenerateLog -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeDeviceClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeUserClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutAdminOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutDataOwnerOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName ErrorMessage -Type MultiString -value "Type the text that the user will see in the error message dialog box."  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName Enabled -Type DWORD -value 1 
  
```  
  
In alternativa, è possibile configurare l'assistenza per accesso negato individualmente in ogni file server mediante la console di gestione risorse File Server.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Per configurare l'assistenza per accesso negato usando Gestione risorse File Server  
  
1.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
2.  Fare doppio clic su **Gestione risorse File Server (locale)**, quindi fare clic su **Configura opzioni**.  
  
3.  Fare clic su di **assistenza per accesso negato** scheda.  
  
4.  Selezionare il **Abilita l'assistenza per accesso negato** casella di controllo.  
  
5.  Nel **Visualizza il messaggio seguente agli utenti vengono negati l'accesso a una cartella o file**, digitare un messaggio che gli utenti vedranno quando non hanno accesso a un file o cartella.  
  
    È possibile aggiungere macro al messaggio per inserirà testo personalizzato. Le macro includono:  
  
    -   **[Original File Path] **Il percorso del file originale che ha effettuato l'accesso dell'utente.  
  
    -   **[Original File Path Folder] **Cartella padre del percorso del file originale che ha effettuato l'accesso dell'utente.  
  
    -   **[Admin Email] **Dell'elenco di destinatari di posta elettronica di amministratore.  
  
    -   **[Data Owner Email] **L'elenco dei destinatari di posta elettronica dati di proprietario.  
  
6.  Fare clic su **richieste tramite posta elettronica configura**, selezionare il **consentire agli utenti di richiedere assistenza** casella di controllo, quindi fare clic su **OK**.  
  
7.  Fare clic su **Preview** se si desidera verificare l'aspetto il messaggio di errore per l'utente.  
  
8.  Fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Dopo aver configurato l'assistenza per accesso negato, è necessario abilitarla per tutti i tipi di file utilizzando criteri di gruppo.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Per configurare l'assistenza per accesso negato per tutti i tipi di file utilizzando criteri di gruppo  
  
1.  Gestione di criteri di gruppo aprire. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione criteri di gruppo**.  
  
2.  Fare doppio clic su criteri di gruppo appropriato, quindi fare clic su **modifica**.  
  
3.  Fare clic su **configurazione Computer**, fare clic su **criteri**, fare clic su **modelli amministrativi**, fare clic su **sistema**, quindi fare clic su **assistenza per accesso negato**.  
  
4.  Fare doppio clic su **Abilita l'assistenza per accesso negato sul client per tutti i tipi di file**, quindi fare clic su **modifica**.  
  
5.  Fare clic su **abilitato**, quindi fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
È anche possibile specificare un messaggio di accesso negato separato per ogni cartella condivisa in un file server mediante la console di gestione risorse File Server.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Per specificare un messaggio di accesso negato separato per una cartella condivisa usando Gestione risorse File Server  
  
1.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
2.  Espandere **Gestione risorse File Server (locale)**, quindi fare clic su **gestione classificazioni**.  
  
3.  Fare doppio clic su **le proprietà di classificazione**, quindi fare clic su **imposta le proprietà di gestione cartelle**.  
  
4.  Nel **proprietà** fare clic su **messaggio di assistenza per accesso negato**, quindi fare clic su **Aggiungi**.  
  
5.  Fare clic su **Sfoglia**e quindi scegliere la cartella che deve avere il messaggio di accesso negato personalizzato.  
  
6.  Nel **valore**, digitare il messaggio da visualizzare agli utenti quando non può accedere a una risorsa all'interno di tale cartella.  
  
    È possibile aggiungere macro al messaggio per inserirà testo personalizzato. Le macro includono:  
  
    -   **[Original File Path] **Il percorso del file originale che ha effettuato l'accesso dell'utente.  
  
    -   **[Original File Path Folder] **Cartella padre del percorso del file originale che ha effettuato l'accesso dell'utente.  
  
    -   **[Admin Email] **Dell'elenco di destinatari di posta elettronica di amministratore.  
  
    -   **[Data Owner Email] **L'elenco dei destinatari di posta elettronica dati di proprietario.  
  
7.  Fare clic su **OK**, quindi fare clic su **Chiudi**.  
  
![Guide alle soluzioni](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Passaggio 2: Configurare le impostazioni delle notifiche di posta elettronica  
È necessario configurare le impostazioni delle notifiche di posta elettronica in ogni file server che invierà i messaggi di assistenza per accesso negato.  
  
[Eseguire questo passaggio usando Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Aprire Gestione risorse File Server. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione risorse File Server**.  
  
2.  Fare doppio clic su **Gestione risorse File Server (locale)**, quindi fare clic su **Configura opzioni**.  
  
3.  Fare clic su di **notifiche di posta elettronica** scheda.  
  
4.  Configurare le impostazioni seguenti:  
  
    -   Nel **nome server SMTP o indirizzo IP**, digitare il nome dell'indirizzo IP del server SMTP all'interno dell'organizzazione.  
  
    -   Nel **destinatari amministratori predefiniti** e **da indirizzo di posta elettronica predefinito** caselle, digitare l'indirizzo di posta elettronica dell'amministratore del file server.  
  
5.  Fare clic su **inviare posta elettronica di prova** per garantire che le notifiche di posta elettronica siano configurate correttamente.  
  
6.  Fare clic su **OK**.  
  
![Guide alle soluzioni](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Passaggio 3: Verificare che l'assistenza per accesso negato sia configurato correttamente  
È possibile verificare che l'assistenza per accesso negato sia configurato correttamente richiedendo a un utente che sta eseguendo prova ad accedere a una condivisione o un file nella condivisione che non hanno accesso a Windows 8. Quando viene visualizzato il messaggio di accesso negato, l'utente dovrebbe vedere un **Richiedi assistenza** pulsante. Dopo aver fatto clic sul pulsante Richiedi assistenza, l'utente può specificare il motivo per l'accesso e quindi inviare un messaggio di posta elettronica al proprietario della cartella o amministratore del file server. Il proprietario della cartella o un amministratore del file server può verificare che il messaggio di posta elettronica è stato ricevuto e contiene i dettagli appropriati.  
  
> [!IMPORTANT]  
> Se si desidera verificare l'assistenza per accesso negato tramite un utente che esegue Windows Server 2012, è necessario installare esperienza Desktop prima della connessione alla condivisione di file.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: Assistenza per accesso negato](Scenario--Access-Denied-Assistance.md)  
  
-   [Piano per l'assistenza per accesso negato](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

