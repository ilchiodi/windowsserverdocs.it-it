---
title: Passaggi per la risoluzione dei problemi comuni di Windows Admin Center
description: Passaggi per la risoluzione dei problemi comuni di Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: f4e772550aaba6fe9a4f78a6032eaabde4aeb0bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406866"
---
# <a name="troubleshooting-windows-admin-center"></a>Risoluzione dei problemi di Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Important]
> Questa guida ti aiuterà a diagnosticare e risolvere i problemi che ti impediscono di utilizzare Windows Admin Center. Se si verifica un problema con uno strumento specifico, controlla se si tratta di un [problema noto.](http://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-_the-module-microsoftpowershelllocalaccounts-could-not-be-loaded_"></a>Errore del programma di installazione con messaggio: **_Non è stato possibile caricare il modulo ' Microsoft. PowerShell. le LocalAccounts '._**

Questo problema può verificarsi se il percorso predefinito del modulo PowerShell è stato modificato o rimosso. Per risolvere il problema, assicurarsi che ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` sia il **primo** elemento della variabile di ambiente PSModulePath. È possibile ottenere questo risultato con la seguente riga di PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Viene visualizzato l'errore **Impossibile raggiungere il sito o la pagina** nel Web browser in uso

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Se hai installato Windows Admin Center come **app in Windows 10**

* Verifica che Windows Admin Center sia in esecuzione. Cercare l'icona dell'interfaccia di amministrazione di Windows ![](../media/trayIcon.PNG) nella barra delle applicazioni o nel **centro di amministrazione di Windows Desktop/SmeDesktop. exe** in Gestione attività. In alternativa, avvia **Windows Admin Center** dal menu Start.

> [!NOTE] 
> Dopo il riavvio, è necessario avviare Windows Admin Center dal menu Start.  

* [Controllare la versione di Windows](#check-the-windows-version)

* Assicurati di usare Microsoft Edge o Google Chrome come Web browser.

* Hai selezionato il certificato corretto al [primo avvio?](../use/get-started.md#selecting-a-client-certificate)

  * Prova ad aprire il browser in una sessione privata, se funziona è necessario pulire la cache.

* È stato eseguito di recente l'aggiornamento di Windows 10 a una nuova build o versione?

  * È possibile che siano state cancellate le impostazioni degli host attendibili. [Seguire queste istruzioni per aggiornare le impostazioni degli host attendibili.](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Se hai installato Windows Admin Center come **gateway in Windows Server**

* È stato eseguito l'aggiornamento da una versione precedente dell'interfaccia di amministrazione di Windows? Verificare che la regola del firewall non sia stata eliminata a causa di [questo problema noto](known-issues.md#upgrade). Usare il comando di PowerShell seguente per determinare se la regola esiste. In caso contrario, seguire [queste istruzioni](known-issues.md#upgrade) per ricrearlo.
    
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Verifica la versione di Windows](#check-the-windows-version) del client e del server.

* Assicurati di usare Microsoft Edge o Google Chrome come Web browser.

* Nel server aprire Gestione attività > Servizi e verificare che l'interfaccia di **amministrazione di ServerManagementGateway/Windows** sia in esecuzione.
![](../media/Service-TaskMan.PNG)

* Testare la connessione di rete al gateway (sostituire \<values > con le informazioni della distribuzione)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Se l'interfaccia di amministrazione di Windows è stata installata in una macchina virtuale Windows Server di Azure

* [Controllare la versione di Windows](#check-the-windows-version)
* Hai aggiunto una regola della porta in ingresso per HTTPS? 
* [Altre informazioni sull'installazione dell'interfaccia di amministrazione di Windows in una macchina virtuale di Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

### <a name="check-the-windows-version"></a>Verifica la versione di Windows

* Apri la finestra di dialogo per l'esecuzione (tasto WINDOWS + R) e avvia ```winver```.

* Se usi Windows 10 versione 1703 o meno recente, Windows Admin Center non è supportato nella versione di Microsoft Edge. Effettua l'aggiornamento a una versione recente di Windows 10 o utilizza Chrome.

* Se si usa una versione di anteprima di insider di Windows 10 o di un server con una versione di build compresa tra 17134 e 17637, Windows presenta un bug che ha causato l'errore dell'interfaccia di amministrazione di Windows. Usare una versione supportata corrente di Windows.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Verificare che il servizio Gestione remota Windows (WinRM) sia in esecuzione nel computer gateway e nel nodo gestito

* Aprire la finestra di dialogo Esegui con WindowsKey + R
* Digitare ```services.msc``` e premere INVIO
* Nella finestra visualizzata cercare Gestione remota Windows (WinRM), verificare che sia in esecuzione e che sia impostata su avvio automatico

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>È stato eseguito l'aggiornamento del server da 2016 a 2019?

* È possibile che siano state cancellate le impostazioni degli host attendibili. [Seguire queste istruzioni per aggiornare le impostazioni degli host attendibili.](#configure-trustedhosts) 

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>Ricevo il messaggio: "Impossibile connettersi in modo sicuro a questa pagina. Questo potrebbe essere dovuto al fatto che il sito Usa impostazioni di sicurezza TLS obsolete o non sicure.

Il computer è limitato alle connessioni HTTP/2. L'interfaccia di amministrazione di Windows usa l'autenticazione integrata di Windows, che non è supportata in HTTP/2. Aggiungere i seguenti due valori del registro di sistema sotto il tasto ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` nel **computer che esegue il browser** per rimuovere la restrizione http/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Si verificano problemi con il Desktop remoto, gli eventi e gli strumenti di PowerShell.

Questi tre strumenti richiedono il protocollo WebSocket, che in genere è bloccato da firewall e server proxy. Se si usa Google Chrome, si verifica un [problema noto](known-issues.md#google-chrome) con i WebSocket e l'autenticazione NTLM.

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Posso connettermi ad alcuni server, ma non ad altri

* Accedere al computer gateway localmente e provare a ```Enter-PSSession <machine name>``` in PowerShell, sostituendo \<machine nome > con il nome del computer che si sta tentando di gestire nell'interfaccia di amministrazione di Windows. 

* Se l'ambiente utilizza un gruppo di lavoro anziché un dominio, vedi [Utilizzo di Windows Admin Center in un gruppo di lavoro](#using-windows-admin-center-in-a-workgroup).

* **Uso degli account amministratore locale:** Se si usa un account utente locale che non è l'account Administrator predefinito, sarà necessario abilitare il criterio nel computer di destinazione eseguendo il comando seguente in PowerShell o al prompt dei comandi come amministratore nel computer di destinazione:

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>Utilizzo di Windows Admin Center in un gruppo di lavoro

### <a name="what-account-are-you-using"></a>Quale account stai utilizzando?
Assicurati che le credenziali che stai utilizzando siano membri del gruppo Administrators locale del server di destinazione. In alcuni casi, WinRM richiede anche l'appartenenza al gruppo Utenti gestione remota. Se usi un account utente locale che **non è l'account predefinito Administrator**, sarà necessario abilitare il criterio sul computer di destinazione eseguendo il seguente comando in PowerShell o in un prompt dei comandi come amministratore sul computer di destinazione:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>Ti stai connettendo a un computer di un gruppo di lavoro su una subnet diversa?

Per connettersi a un computer del gruppo di lavoro che non si trova nella stessa subnet del gateway, assicurati che la porta del firewall per WinRM (TCP 5985) consenta il traffico in ingresso sul computer di destinazione. Per creare questa regola del firewall puoi eseguire il comando seguente in PowerShell o in un prompt dei comandi come amministratore sul computer di destinazione:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>Configura TrustedHosts

Quando si installa Windows Admin Center, viene data la possibilità di consentire a Windows Admin Center di gestire l'impostazione TrustedHosts del gateway. Ciò è necessario in un ambiente di gruppo di lavoro o quando si utilizzano credenziali di amministratore locali in un dominio. Se scegli di ignorare questa impostazione, dovrai configurare manualmente TrustedHosts.

**Per modificare TrustedHosts usando i comandi di PowerShell:**

1. Apri una sessione di PowerShell come amministratore.
2. Visualizza l'impostazione TrustedHosts corrente:

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > Se l'impostazione TrustedHosts corrente non è vuota, i comandi riportati di seguito sovrascrivono l'impostazione. Si consiglia di salvare l'impostazione corrente in un file di testo con il seguente comando in modo da poterla ripristinare, se necessario:
   > 
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Imposta TrustedHosts su NetBIOS, IP o FQDN delle macchine che intendi gestire:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > Per impostare con semplicità tutti i TrustedHosts contemporaneamente, puoi utilizzare un carattere jolly.
   > 
   >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Una volta terminato il test, puoi eseguire il seguente comando da una sessione PowerShell con privilegi elevati per cancellare l'impostazione TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Se in precedenza hai esportato le impostazioni, apri il file, copia i valori e usa questo comando:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>In precedenza ho installato l'interfaccia di amministrazione di Windows e ora non è possibile usare la stessa porta TCP/IP

Eseguire manualmente questi due comandi in un prompt dei comandi con privilegi elevati:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>Le funzionalità di Azure non funzionano correttamente in Microsoft Edge

Edge presenta [problemi noti](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relativi alle aree di sicurezza che influiscono sull'accesso di Azure nell'interfaccia di amministrazione di Windows. Se si verificano problemi durante l'uso delle funzionalità di Azure quando si usa Edge, provare ad aggiungere https://login.microsoftonline.com, https://login.live.com e l'URL del gateway come siti attendibili e ai siti consentiti per le impostazioni del blocco popup perimetrale sul browser lato client. 

A tale scopo, effettuare le seguenti operazioni:
1. Cerca **Opzioni Internet** nel menu Start di Windows
2. Passare alla scheda **sicurezza**
3. Nell'opzione **siti attendibili** fare clic sul pulsante **siti** e aggiungere gli URL nella finestra di dialogo visualizzata. È necessario aggiungere l'URL del gateway, nonché https://login.microsoftonline.com e https://login.live.com.
4. Vai alla scheda **privacy**
5. Nella sezione **blocco popup** fare clic sul pulsante **Settings (impostazioni** ) e aggiungere gli URL nella finestra di dialogo visualizzata. È necessario aggiungere l'URL del gateway, nonché https://login.microsoftonline.com e https://login.live.com.

## <a name="having-an-issue-with-an-azure-related-feature"></a>Problemi con una funzionalità correlata ad Azure?

Inviare un messaggio di posta elettronica a wacFeedbackAzure@microsoft.com con le informazioni seguenti:
* Informazioni generali sul problema dalle [domande elencate di seguito](#providing-feedback-on-issues).
* Descrivere il problema e i passaggi necessari per riprodurre il problema. 
* In precedenza è stato registrato il gateway in Azure usando lo script scaricabile New-AadApp. ps1, quindi è stato eseguito l'aggiornamento alla versione 1807? Oppure è stato registrato il gateway in Azure usando l'interfaccia utente dalle impostazioni del gateway > Azure?
* L'account di Azure è associato a più directory/tenant?
    * In caso affermativo: Quando si registra l'applicazione Azure AD nell'interfaccia di amministrazione di Windows, è stata usata la directory predefinita in Azure? 
* L'account Azure può accedere a più sottoscrizioni?
* Per la sottoscrizione usata è stata applicata la fatturazione?
* È stato effettuato l'accesso a più account Azure quando si è verificato il problema?
* L'account Azure richiede la funzionalità di autenticazione a più fattori?
* Il computer che si sta tentando di gestire una macchina virtuale di Azure?
* L'interfaccia di amministrazione di Windows è installata in una macchina virtuale di Azure?

## <a name="providing-feedback-on-issues"></a>Invio di commenti e suggerimenti sui problemi

Vai al Visualizzatore eventi > Applicazioni e servizi > Microsoft-ServerManagementExperience e cerca eventuali errori o avvisi.

Segnala un bug al nostro [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) che descrive il problema.

Includi eventuali errori o avvisi che sono presenti nel registro eventi, nonché le seguenti informazioni: 

* Piattaforma di **installazione** di Windows Admin Center (Windows 10 o Windows Server):
    * Se installato nel server, qual è la [versione](#check-the-windows-version) di Windows del **computer che esegue il browser** per accedere all'interfaccia di amministrazione di Windows: 
    * Si sta usando il certificato autofirmato creato dal programma di installazione?
    * Se stai utilizzando un tuo certificato, il nome dell'oggetto corrisponde al computer?
    * Se stai utilizzando un tuo certificato, questo specifica un nome dell'oggetto alternativo?
* Hai installato usando l'impostazione predefinita della porta?
    * In caso contrario, quale porta hai specificato?
* Il computer in cui è **installato** Windows Admin Center fa parte di un dominio?
* La [versione](#check-the-windows-version) di Windows in cui è **installato** Windows Admin Center:
* Il computer che **tenti di gestire** fa parte di un dominio?
* La [versione](#check-the-windows-version) di Windows del computer che **tenti di gestire**:
* Che browser utilizzi?
    * Se stai utilizzando Google Chrome, qual è la versione? (Guida > Informazioni su Google Chrome)

