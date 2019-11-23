---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Deploy Access-Denied Assistance (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: afc05f395753e5c5614e92d109d71e05980d5d92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407170"
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Deploy Access-Denied Assistance (Demonstration Steps)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento è spiegato come configurare l'assistenza per accesso negato e come verificarne il corretto funzionamento.  
  
**Contenuto del documento**  
  
-   [Passaggio 1: configurare l'assistenza per accesso negato](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Passaggio 2: configurare le impostazioni di notifica tramite posta elettronica](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Passaggio 3: verificare che l'assistenza per accesso negato sia configurata correttamente](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Passaggio 1: configurare l'assistenza per accesso negato  
È possibile configurare l'assistenza per accesso negato in un dominio usando Criteri di gruppo oppure individualmente in ogni file server tramite la console di Gestione risorse file server. È anche possibile cambiare il messaggio relativo all'accesso negato per una cartella condivisa specifica in un file server.  
  
Per configurare l'assistenza per accesso negato per il dominio usando Criteri di gruppo, eseguire la procedura seguente:  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Per configurare l'assistenza per accesso negato usando Criteri di gruppo  
  
1.  Aprire Gestione Criteri di gruppo. In Server Manager fare clic su **Strumenti** e quindi su **Gestione Criteri di gruppo**.  
  
2.  Fare clic con il pulsante destro del mouse sui Criteri di gruppo appropriati, quindi scegliere **Modifica**.  
  
3.  Fare clic su **Configurazione computer**, quindi su **Criteri**, **Modelli amministrativi**, **Sistema** e infine su **Assistenza per accesso negato**.  
  
4.  Fare clic con il pulsante destro del mouse su **Configura messaggi per errori di accesso negato**e quindi scegliere **Modifica**.  
  
5.  Selezionare l'opzione **Abilitato**.  
  
6.  Configurare le opzioni seguenti:  
  
    1.  Nella casella **Visualizza il messaggio seguente agli utenti a cui è negato l'accesso a una cartella o un file** immettere il messaggio da visualizzare agli utenti in caso di accesso negato a un file o a una cartella.  
  
        È possibile aggiungere macro al messaggio per inserire testo personalizzato. Sono disponibili le macro seguenti:  
  
        -   **[Original File Path]** Percorso file originale a cui l'utente ha effettuato l'accesso.  
  
        -   **[Original File Path Folder]** Cartella padre del percorso file originale a cui l'utente ha effettuato l'accesso.  
  
        -   **[Admin Email]** Elenco di destinatari di messaggi di posta elettronica dell'amministratore.  
  
        -   **[Data Owner Email]** Elenco di proprietari di dati destinatari di posta elettronica.  
  
    2.  Selezionare la casella di controllo **Consenti a utenti di richiedere assistenza** .  
  
    3.  Non modificare le impostazioni predefinite rimanenti.  
  
![la soluzione guida i](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
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
  
In alternativa, è possibile configurare l'assistenza per accesso negato individualmente in ogni file server usando la console di Gestione risorse file server.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Per configurare l'assistenza per accesso negato usando Gestione risorse file server  
  
1.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
2.  Fare clic con il pulsante destro del mouse su **Gestione risorse file server (locale)** , quindi scegliere **Configura opzioni**.  
  
3.  Fare clic sulla scheda **Assistenza per accesso negato** .  
  
4.  Selezionare la casella di controllo **Abilita assistenza per accesso negato** .  
  
5.  Nella casella **Visualizza il messaggio seguente agli utenti a cui è negato l'accesso a una cartella o un file** immettere il messaggio da visualizzare agli utenti in caso di accesso negato a un file o a una cartella.  
  
    È possibile aggiungere macro al messaggio per inserire testo personalizzato. Sono disponibili le macro seguenti:  
  
    -   **[Original File Path]** Percorso file originale a cui l'utente ha effettuato l'accesso.  
  
    -   **[Original File Path Folder]** Cartella padre del percorso file originale a cui l'utente ha effettuato l'accesso.  
  
    -   **[Admin Email]** Elenco di destinatari di messaggi di posta elettronica dell'amministratore.  
  
    -   **[Data Owner Email]** Elenco di proprietari di dati destinatari di posta elettronica.  
  
6.  Fare clic su **Configura richieste tramite posta elettronica**, selezionare la casella di controllo **Consenti a utenti di richiedere assistenza** e infine fare clic su **OK**.  
  
7.  Fare clic su **Anteprima** per visualizzare un'anteprima del messaggio di errore per l'utente.  
  
8.  Fai clic su **OK**.  
  
![la soluzione guida i](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Dopo avere configurato l'assistenza per accesso negato, è necessario abilitarla per tutti i tipi di file usando Criteri di gruppo.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Per configurare l'assistenza per accesso negato per tutti i tipi di file usando Criteri di gruppo  
  
1.  Aprire Gestione Criteri di gruppo. In Server Manager fare clic su **Strumenti** e quindi su **Gestione Criteri di gruppo**.  
  
2.  Fare clic con il pulsante destro del mouse sui Criteri di gruppo appropriati, quindi scegliere **Modifica**.  
  
3.  Fare clic su **Configurazione computer**, quindi su **Criteri**, **Modelli amministrativi**, **Sistema** e infine su **Assistenza per accesso negato**.  
  
4.  Fare clic con il pulsante destro del mouse su **Abilita assistenza per accesso negato sul client per tutti i tipi di file**, quindi scegliere **Modifica**.  
  
5.  Fare clic su **Abilitato**e quindi su **OK**.  
  
![la soluzione guida i](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
È anche possibile specificare un messaggio separato per l'accesso negato per ogni cartella condivisa in un file server usando la console di Gestione risorse file server.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Per specificare un messaggio separato per l'accesso negato a una cartella condivisa usando Gestione risorse file server  
  
1.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
2.  Espandere **Gestione risorse file server (locale)** , quindi fare clic su **Gestione classificazioni**.  
  
3.  Fare clic con il pulsante destro del mouse su **Proprietà classificazione**, quindi scegliere **Imposta proprietà di gestione cartelle**.  
  
4.  Nella casella **Proprietà** fare clic su **Messaggio di assistenza per accesso negato** e quindi su **Aggiungi**.  
  
5.  Fare clic su **Sfoglia**, quindi scegliere la cartella a cui associare il messaggio personalizzato per l'accesso negato.  
  
6.  Nella casella **Valore** digitare il messaggio da visualizzare agli utenti in caso di accesso negato a una risorsa nella cartella.  
  
    È possibile aggiungere macro al messaggio per inserire testo personalizzato. Sono disponibili le macro seguenti:  
  
    -   **[Original File Path]** Percorso file originale a cui l'utente ha effettuato l'accesso.  
  
    -   **[Original File Path Folder]** Cartella padre del percorso file originale a cui l'utente ha effettuato l'accesso.  
  
    -   **[Admin Email]** Elenco di destinatari di messaggi di posta elettronica dell'amministratore.  
  
    -   **[Data Owner Email]** Elenco di proprietari di dati destinatari di posta elettronica.  
  
7.  Fare clic su **OK** e quindi fare clic su **Chiudi**.  
  
![la soluzione guida i](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Passaggio 2: configurare le impostazioni di notifica tramite posta elettronica  
È necessario configurare le impostazioni per le notifiche tramite posta elettronica in ogni file server che invierà i messaggi di assistenza per accesso negato.  
  
[Eseguire questo passaggio con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Aprire Gestione risorse file server. In Server Manager fare clic su **Strumenti** e quindi su **Gestione risorse file server**.  
  
2.  Fare clic con il pulsante destro del mouse su **Gestione risorse file server (locale)** , quindi scegliere **Configura opzioni**.  
  
3.  Fare clic sulla scheda **Notifiche posta elettronica**.  
  
4.  Configurare le impostazioni seguenti:  
  
    -   Nella casella **Nome server SMTP o indirizzo IP** digitare il nome dell'indirizzo IP del server SMTP dell'organizzazione.  
  
    -   Nelle caselle **destinatari amministratori predefiniti** e **indirizzo di posta elettronica predefinito** , digitare l'indirizzo di posta elettronica dell'amministratore file server.  
  
5.  Fare clic su **Invio posta elettronica di prova** per verificare al corretta configurazione delle notifiche tramite posta elettronica.  
  
6.  Fai clic su **OK**.  
  
![la soluzione guida i](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Passaggio 3: verificare che l'assistenza per accesso negato sia configurata correttamente  
È possibile verificare che l'assistenza per accesso negato sia configurata correttamente in modo che un utente che esegue Windows 8 provi ad accedere a una condivisione o a un file in tale condivisione a cui non ha accesso. Quando viene visualizzato il messaggio di accesso negato, l'utente dovrebbe vedere un pulsante **Richiedi assistenza** . Dopo la selezione del pulsante Richiedi assistenza, l'utente può specificare un motivo per l'accesso e quindi inviare un messaggio di posta elettronica al proprietario della cartella o all'amministratore del file server. Il proprietario della cartella o l'amministratore del file server può verificare che il messaggio di posta elettronica è stato ricevuto e che include i dettagli appropriati.  
  
> [!IMPORTANT]  
> Se si desidera verificare l'assistenza per accesso negato tramite un utente che esegue Windows Server 2012, è necessario installare l'esperienza desktop prima di connettersi alla condivisione file.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Scenario: assistenza per accesso negato](Scenario--Access-Denied-Assistance.md)  
  
-   [Pianificare l'assistenza per accesso negato](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

