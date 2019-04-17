---
title: Configurazione delle autorizzazioni e controllo dell'accesso utente
description: Scopri come configurare il controllo di accesso utente e le autorizzazioni con Active Directory o Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152046"
---
# Configurare le autorizzazioni e controllo dell'accesso utente

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Se hai già fatto, acquisire familiarità con le [Opzioni di controllo di accesso utente in Windows Admin Center](../plan/user-access-options.md)

>[!NOTE]
> Accesso dei gruppi basati su Windows Admin Center non è supportato in ambienti di gruppo di lavoro o in domini non attendibili.

## Definizioni dei ruoli di accesso gateway

Esistono due ruoli per l'accesso al servizio gateway Windows Admin Center:

**Gli utenti di gateway** possono connettersi al servizio gateway Windows Admin Center per gestire i server tramite il gateway, ma non possono modificare le autorizzazioni di accesso né il meccanismo di autenticazione usato per autenticare al gateway.

**Gli amministratori di gateway** possono configurare chi ha accesso anche il modo in cui gli utenti autenticati al gateway. Solo gli amministratori di gateway possono visualizzare e configurare le impostazioni di accesso in Windows Admin Center. Gli amministratori locali nel computer gateway sono sempre gli amministratori del servizio gateway Windows Admin Center.

> [!NOTE]
> Accesso al gateway non implica l'accesso ai server gestiti visibile dal gateway. Per gestire un server di destinazione, utente connesso deve usare le credenziali (tramite le proprie credenziali di Windows passato-through o tramite le credenziali fornite nella sessione di Windows Admin Center con l'azione **Gestisci come** ) che hanno accesso amministrativo a tale server di destinazione.

## Active Directory o gruppi di computer locale

Per impostazione predefinita, Active Directory o gruppi di computer locale vengono usati per controllare l'accesso gateway. Se hai un dominio Active Directory, è possibile gestire utente gateway e amministratore accedere da all'interno dell'interfaccia di Windows Admin Center.

Nella scheda **utenti** puoi controllare quali utenti possono accedere a Windows Admin Center come utente gateway. Per impostazione predefinita, e se non specifichi un gruppo di sicurezza, qualsiasi utente che accede all'URL di gateway ha accesso. Dopo aver aggiunto uno o più gruppi di sicurezza all'elenco degli utenti, l'accesso è limitato ai membri di questi gruppi.

Se non usi un dominio Active Directory nel tuo ambiente, l'accesso è controllato dal ```Users``` e ```Administrators``` gruppi locali sul computer gateway Windows Admin Center.

### Autenticazione della smart card

È possibile applicare **l'autenticazione con smart card** , specificando un gruppo aggiuntive di tipo _obbligatorio_ per i gruppi di sicurezza basata su smart card. Dopo avere aggiunto un gruppo di sicurezza basata su smart card, un utente può accedere solo il servizio Windows Admin Center se sono un membro di qualsiasi gruppo di sicurezza e un gruppo di smart card incluso nell'elenco degli utenti.

Nella scheda **amministratori** puoi controllare quali utenti possono accedere a Windows Admin Center come amministratore di gateway. Gruppo administrators locale nel computer avrà sempre accesso amministratore completo e non può essere rimossi dall'elenco. Mediante l'aggiunta di gruppi di sicurezza, consentire ai membri di tali privilegi di gruppi di modificare le impostazioni di gateway Windows Admin Center. L'elenco di amministratori supporta l'autenticazione con smart card nello stesso modo come elenco degli utenti: con la condizione AND per un gruppo di sicurezza e di un gruppo di smart card.

## Azure Active Directory

Se l'organizzazione Usa Azure Active Directory (Azure AD), puoi scegliere di aggiungere un **ulteriore** livello di sicurezza di Windows Admin Center richiedendo l'autenticazione di Azure AD per accedere al gateway. Per accedere a Windows Admin Center, dell'utente **account di Windows** anche deve avere accesso al server gateway (anche se si utilizza l'autenticazione di Azure AD). Quando si utilizza Azure AD, è possibile gestire le autorizzazioni di accesso utente e amministratore Windows Admin Center dal portale di Azure, anziché dall'interno dell'interfaccia utente di Windows Admin Center.

### Accesso a Windows Admin Center quando è abilitata l'autenticazione di Azure AD

A seconda il browser usato, alcuni utenti accesso a Windows Admin Center con l'autenticazione di Azure AD configurato verrà visualizzato un prompt aggiuntive **dal browser** in cui devono fornire le finestre di account le credenziali per il computer in cui Viene installato Windows Admin Center. Dopo aver immesso le informazioni, gli utenti riceveranno la richiesta di autenticazione aggiuntiva Azure Active Directory, che richiede le credenziali di un account Azure che sia stata concessa l'accesso nell'applicazione Azure AD in Azure.

> [!NOTE]
> Utenti che hanno account di Windows dispone di **diritti di amministratore** sul computer gateway verrà non richiesto per l'autenticazione di Azure AD.

### Configurazione dell'autenticazione di Azure Active Directory per Windows Admin Center Preview

Vai a **Impostazioni**di Windows Admin Center > **accesso** e usare l'interruttore Attiva/Disattiva per attivare "usare Azure Active Directory per aggiungere un livello di sicurezza per il gateway". Se non è stato registrato il gateway di Azure, riceverai a tale scopo, in questo momento.

Per impostazione predefinita, tutti i membri del tenant di Azure AD abbiano accesso utente per il servizio gateway Windows Admin Center. Solo gli amministratori locali nel computer gateway hanno accesso come amministratore al gateway Windows Admin Center. Si noti che i diritti del gruppo administrators locale sul computer gateway non possono essere limitati, gli amministratori locali possono eseguire alcuna operazione indipendentemente dal fatto che venga usato Azure AD per l'autenticazione.

Se si desidera assegnare ad Azure AD specifici utenti o gruppi gateway utente o accesso come amministratore gateway al servizio Windows Admin Center, è necessario eseguire le operazioni seguenti:

1.  Vai all'applicazione di Windows Admin Center ad Azure AD nel portale di Azure tramite il collegamento ipertestuale fornito nelle impostazioni di accesso. Tieni presente che il collegamento ipertestuale è disponibile solo quando è abilitata l'autenticazione di Azure Active Directory. 
    -   Puoi anche trovare l'applicazione nel portale di Azure passando ad **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni** e la ricerca **WindowsAdminCenter** (l'app di Azure AD sarà denominato WindowsAdminCenter-<gateway name>). Se non ottieni tutti i risultati di ricerca, assicurarsi di **mostrare** è impostato su **tutte le applicazioni**, **lo stato dell'applicazione** è impostato su **qualsiasi** e fare clic su Applica, prova la ricerca. Dopo aver trovato l'applicazione, passa a **utenti e gruppi**
2.  Nella scheda proprietà, impostare **l'assegnazione di utente necessaria** su Sì.
    Dopo aver eseguito questo, solo i membri elencati nella scheda **utenti e gruppi** sarà in grado di accedere al gateway Windows Admin Center.
3.  Utenti e di scheda gruppi, selezionare **Aggiungi utente**. Devi assegnare un utente di gateway o un ruolo di amministratore di gateway per ogni utente o gruppo aggiunto.

Una volta che si attiva l'autenticazione di Azure AD, il riavvio del servizio gateway e si deve aggiornare il browser. È possibile aggiornare l'accesso utente per l'applicazione di Azure AD SME nel portale di Azure in qualsiasi momento.

Gli utenti verranno richiesto di accedere con la propria identità di Azure Active Directory durante il tentativo di accedere all'URL di gateway Windows Admin Center. Ricorda che gli utenti devono essere anche un membro degli utenti locali nel server gateway per accedere a Windows Admin Center.

Gli utenti e gli amministratori di visualizzare i loro account attualmente connesso e anche come disconnessione di questo Azure AD dalla scheda **Account** di Windows Admin Center impostazioni account.

### Configurazione dell'autenticazione di Azure Active Directory per Windows Admin Center

[Per configurare l'autenticazione di Azure AD, è prima necessario registrare il gateway con Azure](azure-integration.md) (è sufficiente eseguire questa operazione una sola volta per il gateway Windows Admin Center). Questo passaggio Crea un'applicazione Azure AD da cui è possibile gestire utente gateway e accesso come amministratore gateway.

Se si desidera assegnare ad Azure AD specifici utenti o gruppi gateway utente o accesso come amministratore gateway al servizio Windows Admin Center, è necessario eseguire le operazioni seguenti:

1.  Vai alla tua applicazione Azure AD SME nel portale di Azure. 
    -   Quando fai clic sul **controllo di accesso di modifica** e quindi seleziona **Azure Active Directory** dalle impostazioni di accesso a Windows Admin Center, è possibile utilizzare il collegamento ipertestuale fornito nell'interfaccia utente per accedere all'applicazione di Azure AD nel portale di Azure. Questo collegamento ipertestuale è disponibile nelle impostazioni di accesso anche dopo che fare clic su Salva e aver selezionato ad Azure AD come il provider di identità di controllo di accesso.
    -   Puoi anche trovare l'applicazione nel portale di Azure passando ad **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni** e la ricerca **SME** (l'app di Azure AD sarà denominato SME -<gateway>). Se non ottieni tutti i risultati di ricerca, assicurarsi di **mostrare** è impostato su **tutte le applicazioni**, **lo stato dell'applicazione** è impostato su **qualsiasi** e fare clic su Applica, prova la ricerca. Dopo aver trovato l'applicazione, passa a **utenti e gruppi**
2.  Nella scheda proprietà, impostare **l'assegnazione di utente necessaria** su Sì.
    Dopo aver eseguito questo, solo i membri elencati nella scheda **utenti e gruppi** sarà in grado di accedere al gateway Windows Admin Center.
3.  Utenti e di scheda gruppi, selezionare **Aggiungi utente**. Devi assegnare un utente di gateway o un ruolo di amministratore di gateway per ogni utente o gruppo aggiunto.

Dopo aver salvato il controllo di accesso di Azure AD nel riquadro di **controllo di accesso di modifica** , il riavvio del servizio gateway e si deve aggiornare il browser. È possibile aggiornare l'accesso utente per l'applicazione di Windows Admin Center ad Azure AD nel portale di Azure in qualsiasi momento. 

Gli utenti verranno richiesto di accedere con la propria identità di Azure Active Directory durante il tentativo di accedere all'URL di gateway Windows Admin Center. Ricorda che gli utenti devono essere anche un membro degli utenti locali nel server gateway per accedere a Windows Admin Center. 

Usando la scheda **Azure** di impostazioni generali di Windows Admin Center, gli utenti e gli amministratori possono visualizzare loro account attualmente connesso e anche come disconnessione di questo account Azure AD.

### Accesso condizionale e l'autenticazione a più fattori

Uno dei vantaggi dell'utilizzo di Azure AD come un ulteriore livello di sicurezza per controllare l'accesso al gateway Windows Admin Center è che puoi sfruttare funzionalità di sicurezza potente dell'annuncio Azure come accesso condizionale e l'autenticazione a più fattori. 

[Altre informazioni sulla configurazione di accesso condizionale in Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Configurare single sign-on

**Accesso Single sign-on quando distribuito come servizio in Windows Server**

Quando si installa Windows Admin Center in Windows 10, è pronto per utilizzare single sign-on. Se intende utilizzare Windows Admin Center in Windows Server, tuttavia, devi impostare qualche forma di delega Kerberos nell'ambiente prima di poter usare accesso single sign-on. La delega configura il computer gateway come attendibile delegare al nodo di destinazione. 

Per configurare [in base alle risorse della delega vincolata](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1) nel tuo ambiente, eseguire i cmdlet di PowerShell seguenti. (Essere presente che questo richiede un controller di dominio che esegue Windows Server 2012 o versione successiva).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

In questo esempio, il gateway Windows Admin Center è installato nel server **WindowsAdminCenterGW**e il nome del nodo di destinazione è **ManagedNode**.

Per rimuovere la relazione, eseguire il cmdlet seguente:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## Controllo dell'accesso basato sui ruoli

Controllo dell'accesso basato sui ruoli ti consente di fornire agli utenti con accesso limitato alla macchina invece di rendere inseriscono degli amministratori locali completo.
[Per ulteriori informazioni su controllo dell'accesso basato sui ruoli e i ruoli disponibili.](../plan/user-access-options.md#role-based-access-control)

Configurazione di accessi costituita 2 passaggi: abilitare il supporto per i computer di destinazione e assegnazione di utenti ai ruoli pertinenti.

> [!TIP]
> Assicurati di che disporre dei privilegi di amministratore locale per i computer in cui si sta configurando il supporto per il controllo dell'accesso basato sui ruoli.

### Applicare il controllo dell'accesso basato sui ruoli a un singolo computer

Il modello di distribuzione di un computer singolo è ideale per gli ambienti semplice con solo pochi computer per la gestione.
Configurazione di un computer con il supporto per il controllo dell'accesso basato sui ruoli genererà le modifiche seguenti:
-   I moduli di PowerShell con le funzioni richiesto da Windows Admin Center verranno installati nell'unità di sistema, in `C:\Program Files\WindowsPowerShell\Modules`. Tutti i moduli verranno avviato con **Microsoft.Sme**
-   Configurazione dello stato desiderato verrà eseguita una configurazione per configurare un endpoint (Just Enough Administration) nel computer, denominato **Microsoft.Sme.PowerShell**una tantum. Questo endpoint definisce i 3 ruoli utilizzati da Windows Admin Center e verrà eseguito come amministratore locale temporaneo quando un utente connette a esso.
-   3 nuovi gruppi locali verranno creati per controllare quali utenti sono l'accesso assegnati per i ruoli:
    -   Amministratori di Windows Admin Center
    -   Amministratori di Hyper-V di Windows Admin Center
    -   Lettori di Windows Admin Center

Per abilitare il supporto per controllo dell'accesso basato sui ruoli in un singolo computer, segui questi passaggi:

1.  Apri Windows Admin Center e connettersi alla macchina che si desidera configurare con controllo dell'accesso basato sui ruoli con un account con privilegi di amministratore locale sul computer di destinazione.
2.  Scegliere **le impostazioni**dello strumento di **Panoramica** , > **controllo dell'accesso basato sui ruoli**.
3.  Fai clic su **Applica** nella parte inferiore della pagina per abilitare il supporto per controllo dell'accesso basato sui ruoli nel computer di destinazione. Il processo di applicazione prevede la copia di script di PowerShell e richiamare una configurazione (utilizzando PowerShell Desired State Configuration) sul computer di destinazione. Potrebbero richiedere fino a 10 minuti per completare, determinerà il riavvio WinRM. Ciò comporterà la disconnessione temporanea di utenti di Windows Admin Center, PowerShell e WMI.
4.  Aggiornare la pagina per controllare lo stato del controllo dell'accesso basato sui ruoli. Quando è pronto per l'uso, lo stato verrà modificato in **applicata**.

Una volta che viene applicata la configurazione, è possibile assegnare gli utenti ai ruoli:

1.  Apri lo strumento di **utenti e gruppi locali** e passa alla scheda **gruppi** .
2.  Seleziona il gruppo di **Windows Admin Center lettori** .
3.  Nel riquadro dei *Dettagli* nella parte inferiore, fai clic su **Aggiungi utente** e immetti il nome di un utente o gruppo che deve avere accesso in sola lettura al server tramite Windows Admin Center. Gli utenti e gruppi possono essere eseguite dal computer locale o del dominio di Active Directory.
4.  Ripetere i passaggi 2 e 3 per i gruppi di **Amministratori di Hyper-V di Windows Admin Center** e **Agli amministratori di Windows Admin Center** .

È inoltre possibile compilare questi gruppi in modo coerente tra il dominio tramite la configurazione di un oggetto Criteri di gruppo con l' [Impostazione di criteri gruppi con restrizioni](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### Applicare il controllo dell'accesso basato sui ruoli in più computer

In una distribuzione aziendale di grandi dimensioni, è possibile utilizzare gli strumenti di automazione esistenti push le funzionalità di controllo dell'accesso basato sui ruoli per i computer scaricando il pacchetto di configurazione dal gateway Windows Admin Center.
Il pacchetto di configurazione è progettato per essere usato con PowerShell Desired State Configuration, ma puoi adattare il funzionamento con la soluzione di automazione preferita.

#### Scarica la configurazione di controllo di accesso basato sui ruoli

Per scaricare il pacchetto di configurazione di controllo di accesso basato sui ruoli, è necessario avere accesso a Windows Admin Center e un prompt di PowerShell.

Se Esegui il gateway Windows Admin Center in modalità di servizio in Windows Server, Usa il comando seguente per scaricare il pacchetto di configurazione.
Assicurati di aggiornare l'indirizzo del gateway con quello corretto per il tuo ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se stai usando il gateway Windows Admin Center nel tuo computer Windows 10, Esegui invece il comando seguente:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Quando si espande l'archivio zip, vedrai la struttura di cartelle seguente:
- InstallJeaFeatures.ps1
- JustEnoughAdministration (directory)
- Moduli (directory)
    - Microsoft.SME.\* (Directory)
    - WindowsAdminCenter.Jea (directory)

Per configurare il supporto per controllo dell'accesso basato sui ruoli in un nodo, è necessario eseguire le seguenti azioni:
1.  Copia i moduli JustEnoughAdministration, Microsoft.SME.\* e WindowsAdminCenter.Jea nella directory di modulo di PowerShell nel computer di destinazione. In genere, questo si trova in `C:\Program Files\WindowsPowerShell\Modules`.
2.  Aggiorna il file **InstallJeaFeature.ps1** in modo che corrisponda la configurazione desiderata per l'endpoint RBAC.
3.  Eseguire InstallJeaFeature.ps1 per compilare la risorsa DSC.
4.  Distribuire la configurazione DSC a tutti i computer ad applicare la configurazione.

Nella sezione seguente illustra come eseguire questa operazione usando la comunicazione remota di PowerShell.

#### Distribuire su più computer

Per distribuire la configurazione è stato scaricato in più computer, scoprirai necessario per aggiornare lo script **InstallJeaFeatures.ps1** per includere i gruppi di sicurezza appropriate per il tuo ambiente, copiare i file in ogni computer e richiamare il script di configurazione.
È possibile utilizzare strumenti l'automazione preferita a tale scopo, tuttavia, questo articolo è incentrato su un approccio basato su PowerShell puro.

Per impostazione predefinita, lo script di configurazione verrà creare gruppi di sicurezza locale sul computer per controllare l'accesso a ognuno dei ruoli.
Questo è adatto per gruppo di lavoro e i computer appartenenti dominio, ma se si esegue la distribuzione in un ambiente di dominio di sola lettura, puoi decidere di direttamente associare un gruppo di sicurezza del dominio a ogni ruolo.
Per aggiornare la configurazione per l'uso di gruppi di sicurezza di dominio, Apri **InstallJeaFeatures.ps1** e apportare le modifiche seguenti:

1.  Rimuovere le risorse di **gruppo** 3 dal file:
    1.  Lettori "gruppo MS-gruppo"
    2.  "Gruppo MS-Hyper-V--gruppo Administrators"
    3.  Gli amministratori "gruppo MS-gruppo"
2.  Rimuovere le risorse di gruppo 3 dalla proprietà JeaEndpoint **DependsOn**
    1.  "[Gruppo] MS-lettori-gruppo"
    2.  "[Gruppo] MS-Hyper-V--gruppo Administrators"
    3.  "[Gruppo] gruppo di amministratori MS"
3.  Modificare i nomi dei gruppi nella proprietà JeaEndpoint **RoleDefinitions** a gruppi di sicurezza desiderato. Ad esempio, se hai un gruppo di sicurezza *CONTOSO\MyTrustedAdmins* che deve essere l'accesso assegnato il ruolo di amministratori di Windows Admin Center, cambiare `'$env:COMPUTERNAME\Windows Admin Center Administrators'` a `'CONTOSO\MyTrustedAdmins'`. Le tre stringhe che è necessario aggiornare sono:
    1.  ' $env: degli amministratori di COMPUTERNAME\Windows Admin Center
    2.  ' $env: degli amministratori di COMPUTERNAME\Windows Admin Center Hyper-V
    3.  ' $env: dei lettori di COMPUTERNAME\Windows Admin Center

> [!NOTE]
> Assicurati di utilizzare i gruppi di sicurezza univoco per ogni ruolo. Configurazione non riuscirà se stesso gruppo di sicurezza viene assegnato a più ruoli.

Successivamente, alla fine del file **InstallJeaFeatures.ps1** , Aggiungi le seguenti righe di PowerShell nella parte inferiore dello script:

```powershell
Copy-Item "$PSScriptRoot\JustEnoughAdministration" "$env:ProgramFiles\WindowsPowerShell\Modules" -Recurse -Force
$ConfigData = @{
    AllNodes = @()
    ModuleBasePath = @{
        Source = "$PSScriptRoot\Modules"
        Destination = "$env:ProgramFiles\WindowsPowerShell\Modules"
    }
}
InstallJeaFeature -ConfigurationData $ConfigData | Out-Null
Start-DscConfiguration -Path "$PSScriptRoot\InstallJeaFeature" -JobName "Installing JEA for Windows Admin Center" -Force
```

Infine, puoi copiare la cartella contenente i moduli, risorsa DSC e configurazione a ogni nodo di destinazione ed Esegui lo script **InstallJeaFeature.ps1** .
A tale scopo in remoto da workstation di amministrazione, è possibile eseguire i comandi seguenti:

```powershell
$ComputersToConfigure = 'MyServer01', 'MyServer02'

$ComputersToConfigure | ForEach-Object {
    $session = New-PSSession -ComputerName $_ -ErrorAction Stop
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC\JustEnoughAdministration\" -Destination "$env:ProgramFiles\WindowsPowerShell\Modules\" -ToSession $session -Recurse -Force
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC" -Destination "$env:TEMP\WindowsAdminCenter_RBAC" -ToSession $session -Recurse -Force
    Invoke-Command -Session $session -ScriptBlock { Import-Module JustEnoughAdministration; & "$env:TEMP\WindowsAdminCenter_RBAC\InstallJeaFeature.ps1" } -AsJob
    Disconnect-PSSession $session
}
```
