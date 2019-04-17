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
ms.openlocfilehash: a91a8dcf6f05ef0ef66dee603851150b2145d559
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297080"
---
# Risoluzione dei problemi di Windows Admin Center

>Si applica a: Windows Admin Center, Anteprima di Windows Admin Center

> [!Important]
> Questa guida ti aiuterà a diagnosticare e risolvere i problemi che ti impediscono di utilizzare Windows Admin Center. Se si verifica un problema con uno strumento specifico, controlla se si tratta di un [problema noto.](http://aka.ms/wacknownissues)

<a id="toc"></a>

## Collegamenti rapidi

* [Il programma di installazione ha esito negativo con messaggio: ** _Impossibile caricare il modulo di 'Microsoft.PowerShell.LocalAccounts'._**](#psmodulepath)

* Viene visualizzato l'errore **Impossibile raggiungere il sito o la pagina** nel Web browser in uso (seleziona il tipo di distribuzione)
    * [Ho installato Windows Admin Center come app su Windows 10](#whitescreenw10)
    * [Ho installato Windows Admin Center come gateway su Windows Server](#whitescreenws)
    * [Ho installato Windows Admin Center come gateway su una macchina virtuale di Azure](#whitescreenazvm)

* [Windows Admin Center home pagina viene caricata, ma sono bloccato nel riquadro Aggiungi connessione o non riesco a connettermi alla qualsiasi computer.](#winvercompat)

* [Viene visualizzato il messaggio: "Errore durante il caricamento del modulo. RPC: tentativi di 'Ping' scaduti."](#winvercompat)

* [Viene visualizzato il messaggio: "Impossibile connettere in modo sicuro a questa pagina. Questo potrebbe essere perché il sito Usa le impostazioni di protezione TLS non aggiornate o non sicure."](#tls)

* [Sto avendo un problema con gli strumenti di Desktop remoto, eventi e PowerShell.](#websockets)

* [Sto avendo problemi con le funzionalità di Azure in Edge](#azlogin)

* [Posso connettermi ad alcuni server, ma non ad altri](#connectionissues)

* [Sto utilizzando Windows Admin Center in un **gruppo di lavoro**](#workgroup)

* [Era stato installato Windows Admin Center e nient'altro ora puoi usare la stessa porta TCP/IP](#urlacl)

* [Il mio problema non è elencato qui o i passaggi in questa pagina non hanno risolto il problema.](#filebug)

<a id="psmodulepath"></a>

## Programma di installazione ha esito negativo con messaggio: ** _Impossibile caricare il modulo di 'Microsoft.PowerShell.LocalAccounts'._**

Questa situazione può verificarsi se il percorso del modulo di PowerShell predefinito è stato modificato o rimosso. Per risolvere il problema, assicurarsi che ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` è il **primo** elemento nella variabile di ambiente PSModulePath. È possibile ottenere questo risultato con la riga di PowerShell seguente:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## Viene visualizzato l'errore **Impossibile raggiungere il sito o la pagina** nel Web browser in uso

<a id="whitescreenw10"></a>

### Se hai installato Windows Admin Center come **app in Windows 10**

* Verifica che Windows Admin Center sia in esecuzione. Cerca l'icona di Windows Admin Center ![](../media/trayIcon.PNG) nell'area di notifica o **Desktop di Windows Admin Center / SmeDesktop.exe** in Gestione attività. In alternativa, avvia **Windows Admin Center** dal menu Start.

> [!NOTE] 
> Dopo il riavvio, è necessario avviare Windows Admin Center dal menu Start.  

* [Verifica la versione di Windows](#winvercompat)

* Assicurati di usare Microsoft Edge o Google Chrome come Web browser.

* Hai selezionato il certificato corretto al [primo avvio?](../use/get-started.md#selecting-a-client-certificate)

  * Prova ad aprire il browser in una sessione privata, se funziona è necessario pulire la cache.

* Aggiornamento recente Windows 10 a una nuova build o una versione?

  * Questo potrebbe avere cancellato le impostazioni di host attendibili. [Segui queste istruzioni per aggiornare le impostazioni di host attendibili.](#configure-trustedhosts) 

[[torna all'inizio]](#toc)

<a id="whitescreenws"></a>

### Se hai installato Windows Admin Center come **gateway in Windows Server**

* Aggiornamento da una versione precedente di Windows Admin Center? Verifica che la regola del firewall non è stata eliminata a causa di [questo problema noto](known-issues.md#upgrade). Usa il comando PowerShell seguente per determinare se è presente la regola. In caso contrario, segui [queste istruzioni](known-issues.md#upgrade) per ricrearlo.
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Verifica la versione di Windows](#winvercompat) del client e del server.

* Assicurati di usare Microsoft Edge o Google Chrome come Web browser.

* Nel server, Apri Gestione attività gt _ servizi e assicurati che **ServerManagementGateway / Windows Admin Center** è in esecuzione.
![](../media/Service-TaskMan.PNG)

* Verifica la connessione di rete al gateway (sostituisci \<valori> con le informazioni della distribuzione in uso)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[torna all'inizio]](#toc)

<a id="whitescreenazvm"></a>  

### Se hai installato Windows Admin Center in una macchina virtuale server di Windows Azure

* [Verifica la versione di Windows](#winvercompat)
* Hai aggiunto una regola della porta in ingresso per HTTPS? 
* [Ulteriori informazioni sull'installazione di Windows Admin Center in una macchina virtuale di Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[torna all'inizio]](#toc)

<a id="winvercompat"></a>

### Verifica la versione di Windows

* Apri la finestra di dialogo per l'esecuzione (tasto WINDOWS + R) e avvia ```winver```.

* Se usi Windows 10 versione 1703 o meno recente, Windows Admin Center non è supportato nella versione di Microsoft Edge. Effettua l'aggiornamento a una versione recente di Windows 10 o utilizza Chrome.

* Se usi una versione di anteprima di Windows 10 o Windows Server con una versione di build tra 17134 e 17637, Windows avuto un bug che ha causato Windows Admin Center avere esito negativo. Utilizzare una versione supportata corrente di Windows.

### Il servizio di gestione remota Windows (WinRM) sia in esecuzione su computer gateway e nodo gestito

* Aprire la finestra di dialogo eseguire con tasto Windows + R
* Tipo di ```services.msc``` e premi INVIO
* Nella finestra che si apre, visuale per la gestione remota Windows (WinRM), assicurati è in esecuzione e imposta per l'avvio automatico

### Aggiornare il server da 2016 a 2019?

* Questo potrebbe avere cancellato le impostazioni di host attendibili. [Segui queste istruzioni per aggiornare le impostazioni di host attendibili.](#configure-trustedhosts) 

[[torna all'inizio]](#toc)

<a id="tls"></a>

## Viene visualizzato il messaggio: "Impossibile connettere in modo sicuro a questa pagina. È possibile che il sito Usa le impostazioni di protezione TLS non aggiornate o non sicure.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
Il computer è limitato alle connessioni HTTP/2. Windows Admin Center utilizza l'autenticazione integrata di Windows, che non è supportato in HTTP/2. Aggiungere i seguenti valori del Registro di due sistema sotto il ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` chiave in **computer che esegue il browser** per rimuovere la limitazione di HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[torna all'inizio]](#toc)

<a id="websockets"></a> 

## Sto avendo un problema con gli strumenti di Desktop remoto, eventi e PowerShell.

Questi tre strumenti richiedono il protocollo websocket, comunemente bloccato da server proxy e firewall. Se stai utilizzando Google Chrome, esiste un [problema noto](known-issues.md#google-chrome) con WebSocket e l'autenticazione NTLM.

[[torna all'inizio]](#toc)


<a id="connectionissues"></a> 

## Posso connettermi ad alcuni server, ma non ad altri
* Accedi localmente al computer gateway e prova ```Enter-PSSession <machine name>``` in PowerShell, sostituendo \<nome computer> con il nome del computer che vuoi gestire in Windows Admin Center. 

* Se l'ambiente utilizza un gruppo di lavoro anziché un dominio, vedi [Utilizzo di Windows Admin Center in un gruppo di lavoro](#workgroup).

* **Utilizzo degli account Administrator locali:** se usi un account utente locale che non è l'account predefinito Administrator, sarà necessario abilitare il criterio sul computer di destinazione eseguendo il seguente comando in PowerShell o in un prompt dei comandi come amministratore sul computer di destinazione:

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[torna all'inizio]](#toc)

<a id="workgroup"></a>

## Utilizzo di Windows Admin Center in un gruppo di lavoro 

### Quale account stai utilizzando?
Assicurati che le credenziali che stai utilizzando siano membri del gruppo Administrators locale del server di destinazione. In alcuni casi, WinRM richiede anche l'appartenenza al gruppo Utenti gestione remota. Se usi un account utente locale che **non è l'account predefinito Administrator**, sarà necessario abilitare il criterio sul computer di destinazione eseguendo il seguente comando in PowerShell o in un prompt dei comandi come amministratore sul computer di destinazione:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### Ti stai connettendo a un computer di un gruppo di lavoro su una subnet diversa?

Per connettersi a un computer del gruppo di lavoro che non si trova nella stessa subnet del gateway, assicurati che la porta del firewall per WinRM (TCP 5985) consenta il traffico in ingresso sul computer di destinazione. Per creare questa regola del firewall puoi eseguire il comando seguente in PowerShell o in un prompt dei comandi come amministratore sul computer di destinazione:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### Configura TrustedHosts

Quando si installa Windows Admin Center, viene data la possibilità di consentire a Windows Admin Center di gestire l'impostazione TrustedHosts del gateway. Ciò è necessario in un ambiente di gruppo di lavoro o quando si utilizzano credenziali di amministratore locali in un dominio. Se scegli di ignorare questa impostazione, dovrai configurare manualmente TrustedHosts.

**Per modificare TrustedHosts utilizzando i comandi PowerShell:**

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

[[torna all'inizio]](#toc)

<a id="urlacl"></a>

## Era stato installato Windows Admin Center e nient'altro ora puoi usare la stessa porta TCP/IP

Eseguire manualmente questi due comandi in un prompt dei comandi con privilegi elevati:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[torna all'inizio]](#toc)

<a id="azlogin"></a>

## Sto avendo problemi con le funzionalità di Azure in Edge

Edge ha [problemi noti](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relativi a aree di sicurezza che influiscono sull'accesso di Azure in Windows Admin Center. Se si verificano problemi con le funzionalità di Azure quando si utilizza Edge, prova ad aggiungere https://login.microsoftonline.com, https://login.live.com e l'URL del gateway come siti attendibili e per i siti consentiti per le impostazioni di blocco popup Edge nel browser sul lato client. 

A tale scopo, effettua le seguenti operazioni:
1. Ricerca di **Opzioni Internet** nel Menu Start di Windows
2. Vai alla scheda **sicurezza**
3. Sotto l'opzione di **Siti attendibili** , fare clic sul pulsante **siti** e aggiungere gli URL nella finestra di dialogo visualizzata. È necessario aggiungere l'URL del gateway così come https://login.microsoftonline.com e https://login.live.com.
4. Vai alla scheda **Privacy**
5. Nella sezione **Blocco popup** , fare clic sul pulsante **Impostazioni** e aggiungere gli URL nella finestra di dialogo visualizzata. È necessario aggiungere l'URL del gateway così come https://login.microsoftonline.com e https://login.live.com.


[[torna all'inizio]](#toc)

<a id="azissue"></a>

## Avendo un problema con una funzionalità di Azure?

Inviaci un messaggio e-mail al wacFeedbackAzure@microsoft.com con le informazioni seguenti:
* Informazioni sui problemi generali di [domande elencate di seguito](#filebug). 
* Descrivere il problema e i passaggi per riprodurre il problema. 
* È stato precedentemente registrare il gateway di Azure tramite lo script di New-AadApp.ps1 scaricabile e quindi Esegui l'aggiornamento alla versione 1807? O è registrato il gateway di Azure tramite l'interfaccia utente da gt _ impostazioni gateway Azure?
* È l'account Azure associata a più directory/tenant?
    * Se Sì: durante la registrazione di un'applicazione Azure AD per Windows Admin Center, è stato alla directory è utilizzata la directory predefinita in Azure? 
* L'account Azure ha accesso a più sottoscrizioni?
* La sottoscrizione che si usasse dispone di fatturazione associata?
* Sono state si è connessi a più account di Azure quando si è verificato il problema?
* L'account Azure richiede l'autenticazione a più fattori?
* È il computer che si sta tentando di gestire macchine Virtuali di Azure?
* Windows Admin Center è installato in una macchina virtuale di Azure?

[[torna all'inizio]](#toc)

<a id="filebug"></a>

## Ancora non funziona, oppure è il problema non presente qui? [domande comuni sulla risoluzione dei problemi]

Vai al Visualizzatore eventi > Applicazioni e servizi > Microsoft-ServerManagementExperience e cerca eventuali errori o avvisi.

Segnala un bug al nostro [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) che descrive il problema.

Includi eventuali errori o avvisi che sono presenti nel registro eventi, nonché le seguenti informazioni: 

* Piattaforma di **installazione** di Windows Admin Center (Windows 10 o Windows Server):
    * Se è installato nel Server, che cos'è la [versione](#winvercompat) di Windows di **computer che esegue il browser** per accedere a Windows Admin Center: 
    * Utilizzi il certificato autofirmato creato dal programma di installazione?
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

[[torna all'inizio]](#toc)