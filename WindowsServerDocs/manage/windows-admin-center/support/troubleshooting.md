---
title: Passaggi per la risoluzione dei problemi comuni di Windows Admin Center
description: Passaggi per la risoluzione dei problemi comuni di Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/12/2019
ms.openlocfilehash: 53c943ee3eddbe8f67bec125961eb3d36ead17a3
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034473"
---
# <a name="troubleshooting-windows-admin-center"></a>Risoluzione dei problemi di Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

> [!Important]
> Questa guida ti aiuterà a diagnosticare e risolvere i problemi che ti impediscono di utilizzare Windows Admin Center. Se si verifica un problema con uno strumento specifico, controlla se si tratta di un [problema noto.](http://aka.ms/wacknownissues)

<a id="toc"></a>

## <a name="quick-links"></a>Collegamenti rapidi

* [Il programma di installazione ha esito negativo con messaggio: **_Non è stato possibile caricare il modulo 'Microsoft.PowerShell.LocalAccounts'._** ](#psmodulepath)

* Viene visualizzato l'errore **Impossibile raggiungere il sito o la pagina** nel Web browser in uso (seleziona il tipo di distribuzione)
    * [Ho Windows Admin Center installato come un'App in Windows 10](#whitescreenw10)
    * [Ho Windows Admin Center installato come Gateway in Windows Server](#whitescreenws)
    * [Ho Windows Admin Center installato come Gateway in una macchina virtuale di Azure](#if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm)

* [Caricamenti di pagina iniziale di Windows Admin Center, ma viene bloccata nel riquadro Aggiungi connessione o non è possibile connettersi a qualsiasi macchina.](#winvercompat)

* [Viene visualizzato il messaggio: "Errore durante il caricamento del modulo. RPC: Scaduto tentativi "Ping"."](#winvercompat)

* [Viene visualizzato il messaggio: "Impossibile connettersi in modo sicuro a questa pagina. È possibile che il sito Usa le impostazioni di sicurezza TLS obsolete o non sicure."](#tls)

* [Si riscontrano problemi con gli strumenti Desktop remoto, eventi e PowerShell.](#websockets)

* [Si stanno verificando problemi usando le funzionalità di Azure in Microsoft Edge](#azlogin)

* [È possibile connettersi a alcuni server, ma non per altri](#connectionissues)

* [Utilizzo di Windows Admin Center in un **workgroup**](#workgroup)

* [Avevo già Windows Admin Center installato e nient'altro possono ora usare la stessa porta TCP/IP](#urlacl)

* [Problema non elencato qui, o i passaggi in questa pagina non è stato risolto il problema.](#filebug)

<a id="psmodulepath"></a>

## <a name="installer-fails-with-message-the-module-microsoftpowershelllocalaccounts-could-not-be-loaded"></a>Programma di installazione ha esito negativo con messaggio: **_Non è stato possibile caricare il modulo 'Microsoft.PowerShell.LocalAccounts'._** 

Questa situazione può verificarsi se il percorso del modulo di PowerShell predefinito è stato modificato o rimosso. Per risolvere il problema, assicurarsi che ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` è il **primo** elemento nella variabile di ambiente PSModulePath. È possibile ottenere questo risultato con la riga seguente di PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Viene visualizzato l'errore **Impossibile raggiungere il sito o la pagina** nel Web browser in uso

<a id="whitescreenw10"></a>

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Se hai installato Windows Admin Center come **app in Windows 10**

* Verifica che Windows Admin Center sia in esecuzione. Cercare l'icona di Windows Admin Center ![](../media/trayIcon.PNG) nella barra delle applicazioni oppure **Desktop di Windows Admin Center / SmeDesktop.exe** in Gestione attività. In alternativa, avvia **Windows Admin Center** dal menu Start.

> [!NOTE] 
> Dopo il riavvio, è necessario avviare Windows Admin Center dal menu Start.  

* [Controllare la versione di Windows](#winvercompat)

* Assicurati di usare Microsoft Edge o Google Chrome come Web browser.

* Hai selezionato il certificato corretto al [primo avvio?](../use/get-started.md#selecting-a-client-certificate)

  * Prova ad aprire il browser in una sessione privata, se funziona è necessario pulire la cache.

* Controllare recentemente è stato aggiornato Windows 10 a una nuova build o versione?

  * Ciò che sia stata deselezionata le impostazioni di host attendibili. [Seguire queste istruzioni per aggiornare le impostazioni di host attendibili.](#configure-trustedhosts) 

[[Torna all'inizio]](#toc)

<a id="whitescreenws"></a>

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Se hai installato Windows Admin Center come **gateway in Windows Server**

* È stata l'aggiornamento da una versione precedente di Windows Admin Center? Verificare che la regola del firewall non è stata eliminata a causa dell'errore [questo problema noto](known-issues.md#upgrade). Usare il comando PowerShell seguente per determinare se la regola esiste. In caso contrario, seguire [queste istruzioni](known-issues.md#upgrade) per ricrearla.
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Verifica la versione di Windows](#winvercompat) del client e del server.

* Assicurati di usare Microsoft Edge o Google Chrome come Web browser.

* Nel server, aprire Gestione attività > servizi e verificare che **ServerManagementGateway / Windows Admin Center** è in esecuzione.
![](../media/Service-TaskMan.PNG)

* Testare la connessione di rete al Gateway (sostituire \<valori > con le informazioni di distribuzione)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[Torna all'inizio]](#toc)

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Se è stato installato Windows Admin Center in una macchina virtuale Server di Windows Azure

* [Controllare la versione di Windows](#winvercompat)
* Hai aggiunto una regola della porta in ingresso per HTTPS? 
* [Altre informazioni sull'installazione di Windows Admin Center in una VM di Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[Torna all'inizio]](#toc)

<a id="winvercompat"></a>

### <a name="check-the-windows-version"></a>Verifica la versione di Windows

* Apri la finestra di dialogo per l'esecuzione (tasto WINDOWS + R) e avvia ```winver```.

* Se usi Windows 10 versione 1703 o meno recente, Windows Admin Center non è supportato nella versione di Microsoft Edge. Effettua l'aggiornamento a una versione recente di Windows 10 o utilizza Chrome.

* Se si usa una versione di anteprima insider di Windows 10 o Server con una versione di build 17134 quella 17637, Windows presenta un bug che causava Windows Admin Center esito negativo. Usare una versione supportata corrente di Windows.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Assicurarsi che il servizio Gestione remota Windows (WinRM) è in esecuzione nel computer gateway sia nodo gestito

* Aprire la finestra di dialogo eseguire con tasto Windows + R
* Tipo ```services.msc``` e premere INVIO
* Nella finestra visualizzata, cercare la gestione remota Windows (WinRM), assicurarsi che sia in esecuzione e impostato per avviarsi automaticamente

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>Sono stati si aggiorna il server da 2016 a 2019?

* Ciò che sia stata deselezionata le impostazioni di host attendibili. [Seguire queste istruzioni per aggiornare le impostazioni di host attendibili.](#configure-trustedhosts) 

[[Torna all'inizio]](#toc)

<a id="tls"></a>

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>Viene visualizzato il messaggio: "Impossibile connettersi in modo sicuro a questa pagina. È possibile che il sito Usa le impostazioni di sicurezza TLS obsolete o non sicure.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
Computer in uso è limitato per le connessioni HTTP/2. Windows Admin Center Usa l'autenticazione Windows integrata, che non è supportato in HTTP/2. Aggiungere i seguenti valori del Registro di sistema nel ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` tasto **il computer che esegue il browser** rimozione della restrizione HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[Torna all'inizio]](#toc)

<a id="websockets"></a> 

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Si riscontrano problemi con gli strumenti Desktop remoto, eventi e PowerShell.

Queste tre strumenti richiedono il protocollo websocket, che in genere è bloccato da server proxy e firewall. Se si usa Google Chrome, è presente una [problema noto](known-issues.md#google-chrome) con WebSocket e l'autenticazione NTLM.

[[Torna all'inizio]](#toc)


<a id="connectionissues"></a> 

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Posso connettermi ad alcuni server, ma non ad altri
* Accedere al computer del gateway in locale e tentare ```Enter-PSSession <machine name>``` in PowerShell, sostituendo \<nome computer > con il nome del computer di cui si sta tentando di gestire in Windows Admin Center. 

* Se l'ambiente utilizza un gruppo di lavoro anziché un dominio, vedi [Utilizzo di Windows Admin Center in un gruppo di lavoro](#workgroup).

* **Utilizzo di account di amministratore locale:** Se si usa un account utente locale che non è l'account amministratore predefinito, è necessario abilitare i criteri nel computer di destinazione eseguendo il comando seguente in PowerShell o un prompt dei comandi come amministratore nel computer di destinazione:

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[Torna all'inizio]](#toc)

<a id="workgroup"></a>

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

    > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Imposta TrustedHosts su NetBIOS, IP o FQDN delle macchine che intendi gestire:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

    > [!TIP] 
    >Per impostare con semplicità tutti i TrustedHosts contemporaneamente, puoi utilizzare un carattere jolly.

    >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Una volta terminato il test, puoi eseguire il seguente comando da una sessione PowerShell con privilegi elevati per cancellare l'impostazione TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Se in precedenza hai esportato le impostazioni, apri il file, copia i valori e usa questo comando:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

[[Torna all'inizio]](#toc)

<a id="urlacl"></a>

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>Avevo già Windows Admin Center installato e nient'altro possono ora usare la stessa porta TCP/IP

Eseguire manualmente i due comandi seguenti in un prompt dei comandi con privilegi elevati:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[Torna all'inizio]](#toc)

<a id="azlogin"></a>

## <a name="im-having-issues-using-azure-features-in-edge"></a>Si stanno verificando problemi usando le funzionalità di Azure in Microsoft Edge

Edge ha [problemi noti](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) correlati alle aree di sicurezza che interessano l'accesso ad Azure in Windows Admin Center. Se si sono verificati problemi nell'uso di funzionalità di Azure quando si usa Edge, provare ad aggiungere https://login.microsoftonline.com, https://login.live.com e l'URL del gateway come siti attendibili e a siti consentiti per le impostazioni di blocco popup in Edge nel browser sul lato client. 

A tale scopo, effettuare le seguenti operazioni:
1. Cercare **Opzioni Internet** in Windows il Menu Start
2. Andare alla **sicurezza** scheda
3. Sotto il **dei siti attendibili** opzione, fare clic sul **siti** pulsante e aggiungere gli URL nella finestra di dialogo visualizzata. È necessario aggiungere l'URL del gateway, nonché https://login.microsoftonline.com e https://login.live.com.
4. Andare alla **Privacy** scheda
5. Sotto il **blocco popup** sezione, fare clic sui **impostazioni** pulsante e aggiungere gli URL nella finestra di dialogo visualizzata. È necessario aggiungere l'URL del gateway, nonché https://login.microsoftonline.com e https://login.live.com.


[[Torna all'inizio]](#toc)

<a id="azissue"></a>

## <a name="having-an-issue-with-an-azure-related-feature"></a>Verifica un problema con una funzionalità correlate ad Azure?

Inviare un'e-mail all'indirizzo wacFeedbackAzure@microsoft.com con le informazioni seguenti:
* Informazioni sui problemi generali dal [domande elencate di seguito](#filebug). 
* Descrivere il problema e i passaggi eseguiti per riprodurre il problema. 
* È stato in precedenza registrare il gateway in Azure usando lo script scaricabile di New-AadApp.ps1 e quindi eseguire l'aggiornamento alla versione 1807? Oppure è stata registrata il gateway in Azure usando l'interfaccia utente dal gateway Impostazioni > Azure?
* È l'account Azure associato a più tenant di directory /?
    * In caso affermativo: Quando si registra l'applicazione di Azure AD per Windows Admin Center, è stata la directory è usata la directory predefinita in Azure? 
* L'account di Azure ha accesso a più sottoscrizioni?
* La sottoscrizione che si usava ha collegata la fatturazione?
* Sono stati si è connessi a più account Azure quando si è verificato il problema?
* L'account di Azure richiede l'autenticazione a più fattori?
* È il computer in cui che si sta tentando di gestire una macchina virtuale di Azure?
* Windows Admin Center è installato in una macchina virtuale di Azure?

[[Torna all'inizio]](#toc)

<a id="filebug"></a>

## <a name="still-not-working-or-is-your-issue-not-captured-here-troubleshooting-common-questions"></a>Ancora non funziona o è il problema non acquisito qui? [domande frequenti sulla risoluzione dei problemi]

Vai al Visualizzatore eventi > Applicazioni e servizi > Microsoft-ServerManagementExperience e cerca eventuali errori o avvisi.

Segnala un bug al nostro [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) che descrive il problema.

Includi eventuali errori o avvisi che sono presenti nel registro eventi, nonché le seguenti informazioni: 

* Piattaforma di **installazione** di Windows Admin Center (Windows 10 o Windows Server):
    * Se installato nel Server, qual è il Windows [versione](#winvercompat) dei **computer in esecuzione il browser** per accedere a Windows Admin Center: 
    * Si sta utilizzando il certificato autofirmato creato dal programma di installazione?
    * Se stai utilizzando un tuo certificato, il nome dell'oggetto corrisponde al computer?
    * Se stai utilizzando un tuo certificato, questo specifica un nome dell'oggetto alternativo?
* Hai installato usando l'impostazione predefinita della porta?
    * In caso contrario, quale porta hai specificato?
* Il computer in cui è **installato** Windows Admin Center fa parte di un dominio?
* La [versione](#winvercompat) di Windows in cui è **installato** Windows Admin Center:
* Il computer che **tenti di gestire** fa parte di un dominio?
* La [versione](#winvercompat) di Windows del computer che **tenti di gestire**:
* Che browser utilizzi?
    * Se stai utilizzando Google Chrome, qual è la versione? (Guida > Informazioni su Google Chrome)

[[Torna all'inizio]](#toc)