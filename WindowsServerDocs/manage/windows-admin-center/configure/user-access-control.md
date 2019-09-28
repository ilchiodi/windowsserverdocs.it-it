---
title: Configurazione del controllo di accesso utente e delle autorizzazioni
description: Informazioni su come configurare il controllo di accesso utente e le autorizzazioni usando Active Directory o Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 20b311e9330880c2b26e2494aabe27bb04891868
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407031"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurare il controllo di accesso utente e le autorizzazioni

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Se non è già stato fatto, acquisire familiarità con le [Opzioni di controllo di accesso utente nell'interfaccia di amministrazione di Windows](../plan/user-access-options.md)

> [!NOTE]
> L'accesso basato sui gruppi nell'interfaccia di amministrazione di Windows non è supportato negli ambienti del gruppo di lavoro o in domini non trusted.

## <a name="gateway-access-role-definitions"></a>Definizioni del ruolo accesso gateway

Sono disponibili due ruoli per l'accesso al servizio gateway dell'interfaccia di amministrazione di Windows:

**Gli utenti del gateway** possono connettersi al servizio gateway dell'interfaccia di amministrazione di Windows per gestire i server tramite tale gateway, ma non possono modificare le autorizzazioni di accesso né il meccanismo di autenticazione usato per autenticare il gateway.

Gli **amministratori del gateway** possono configurare chi ottiene l'accesso e il modo in cui gli utenti eseguono l'autenticazione al gateway. Solo gli amministratori del gateway possono visualizzare e configurare le impostazioni di accesso nell'interfaccia di amministrazione di Windows. Gli amministratori locali del computer gateway sono sempre amministratori del servizio gateway dell'interfaccia di amministrazione di Windows.

> [!NOTE]
> L'accesso al gateway non implica l'accesso ai server gestiti visibili dal gateway. Per gestire un server di destinazione, l'utente che esegue la connessione deve usare le credenziali (tramite le credenziali di Windows passate o tramite le credenziali fornite nella sessione dell'interfaccia di amministrazione di Windows usando l'azione **Gestisci come** ) con accesso amministrativo al server di destinazione.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory o gruppi di computer locali

Per impostazione predefinita, Active Directory o i gruppi di computer locali vengono usati per controllare l'accesso al gateway. Se si dispone di un dominio di Active Directory, è possibile gestire l'accesso degli utenti e degli amministratori del gateway dall'interfaccia di amministrazione di Windows.

Nella scheda **utenti** è possibile controllare chi può accedere a centro di amministrazione di Windows come utente gateway. Per impostazione predefinita, e se non si specifica un gruppo di sicurezza, tutti gli utenti che accedono all'URL del gateway hanno accesso. Quando si aggiungono uno o più gruppi di sicurezza all'elenco utenti, l'accesso è limitato ai membri di tali gruppi.

Se non si usa un dominio Active Directory nell'ambiente, l'accesso viene controllato dai gruppi locali `Users` e `Administrators` nel computer gateway dell'interfaccia di amministrazione di Windows.

### <a name="smartcard-authentication"></a>Autenticazione smart card

È possibile applicare **l'autenticazione con smart card** specificando un gruppo aggiuntivo _obbligatorio_ per i gruppi di sicurezza basati su smart card. Una volta aggiunto un gruppo di sicurezza basato su smart card, un utente può accedere al servizio Windows Admin Center solo se è membro di un gruppo di sicurezza e di un gruppo di Smart Card incluso nell'elenco degli utenti.

Nella scheda **amministratori** è possibile controllare chi può accedere all'interfaccia di amministrazione di Windows come amministratore del gateway. Il gruppo Administrators locale nel computer disporrà sempre dell'accesso amministratore completo e non potrà essere rimosso dall'elenco. Aggiungendo gruppi di sicurezza, si assegnano i privilegi ai membri dei gruppi per modificare le impostazioni del gateway dell'interfaccia di amministrazione di Windows. L'elenco Administrators supporta l'autenticazione con smart card in modo analogo all'elenco utenti: con la condizione AND per un gruppo di sicurezza e un gruppo smartcard.

## <a name="azure-active-directory"></a>Azure Active Directory

Se l'organizzazione USA Azure Active Directory (Azure AD), è possibile scegliere di aggiungere un **ulteriore** livello di sicurezza al centro di amministrazione di Windows richiedendo Azure ad autenticazione per accedere al gateway. Per accedere all'interfaccia di amministrazione di Windows, l' **account di Windows** dell'utente deve avere anche accesso al server gateway (anche se viene usata l'autenticazione Azure ad). Quando si usa Azure AD, si gestiranno le autorizzazioni di accesso degli utenti e degli amministratori di Windows Admin Center dal portale di Azure anziché dall'interfaccia utente di Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Accesso all'interfaccia di amministrazione di Windows quando è abilitata l'autenticazione Azure AD

A seconda del browser utilizzato, alcuni utenti che accedono all'interfaccia di amministrazione di Windows con Azure AD autenticazione configurata riceveranno un messaggio di richiesta aggiuntivo **dal browser** in cui devono fornire le credenziali dell'account di Windows per il computer in cui Windows Admin Center è installato. Dopo aver immesso queste informazioni, gli utenti otterranno la richiesta di autenticazione aggiuntiva Azure Active Directory, che richiede le credenziali di un account Azure a cui è stato concesso l'accesso nell'applicazione Azure AD in Azure.

> [!NOTE]
> Agli utenti che dispongono di **diritti di amministratore** nel computer del gateway non verrà richiesto di eseguire l'autenticazione Azure ad.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configurazione dell'autenticazione Azure Active Directory per l'anteprima dell'interfaccia di amministrazione di Windows

Passare a **Impostazioni**di amministrazione di Windows  > **accesso** e usare l'interruttore attiva/disattiva per attivare "USA Azure Active Directory per aggiungere un livello di sicurezza al gateway". Se il gateway non è stato registrato in Azure, sarà necessario eseguire questa operazione in questo momento.

Per impostazione predefinita, tutti i membri del tenant Azure AD hanno accesso utente al servizio gateway di Windows Admin Center. Solo gli amministratori locali del computer gateway hanno accesso come amministratore al gateway dell'interfaccia di amministrazione di Windows. Si noti che i diritti degli amministratori locali nel computer del gateway non possono essere limitati. gli amministratori locali possono eseguire qualsiasi operazione indipendentemente dal fatto che Azure AD venga usato per l'autenticazione.

Se si vuole assegnare a utenti specifici Azure AD o gruppi l'accesso dell'utente gateway o dell'amministratore del gateway al servizio centro di amministrazione di Windows, è necessario eseguire le operazioni seguenti:

1.  Passare al centro di amministrazione di Windows Azure AD applicazione nell'portale di Azure usando il collegamento ipertestuale fornito in impostazioni di accesso. Nota Questo collegamento ipertestuale è disponibile solo quando è abilitata l'autenticazione Azure Active Directory. 
    -   È anche possibile trovare l'applicazione nel portale di Azure accedendo a **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni** e cercando **WindowsAdminCenter** (l'app Azure ad verrà denominata WindowsAdminCenter-<gateway name>). Se non vengono visualizzati risultati di ricerca, verificare che **Mostra** sia impostato su **tutte le applicazioni**, che **lo stato dell'applicazione** sia impostato su **qualsiasi** , quindi fare clic su Applica, quindi provare la ricerca. Dopo aver trovato l'applicazione, passare a **utenti e gruppi**
2.  Nella scheda Proprietà impostare **assegnazione utente obbligatoria** su Sì.
    Una volta completata questa operazione, solo i membri elencati nella scheda **utenti e gruppi** saranno in grado di accedere al gateway dell'interfaccia di amministrazione di Windows.
3.  Nella scheda utenti e gruppi selezionare **Aggiungi utente**. È necessario assegnare un ruolo utente gateway o amministratore gateway per ogni utente/gruppo aggiunto.

Dopo aver acceso Azure AD autenticazione, il servizio gateway viene riavviato ed è necessario aggiornare il browser. È possibile aggiornare l'accesso utente per l'applicazione PMI Azure AD nel portale di Azure in qualsiasi momento.

Agli utenti verrà richiesto di accedere con l'identità del Azure Active Directory quando tenteranno di accedere all'URL del gateway dell'interfaccia di amministrazione di Windows. Tenere presente che gli utenti devono essere anche membri degli utenti locali nel server gateway per accedere all'interfaccia di amministrazione di Windows.

Gli utenti e gli amministratori possono visualizzare il proprio account attualmente connesso e la disconnessione di questo account Azure AD dalla scheda **account** delle impostazioni dell'interfaccia di amministrazione di Windows.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configurazione dell'autenticazione Azure Active Directory per l'interfaccia di amministrazione di Windows

[Per configurare Azure ad autenticazione, è necessario prima registrare il gateway con Azure](azure-integration.md) . è necessario eseguire questa operazione una sola volta per il gateway dell'interfaccia di amministrazione di Windows. Questo passaggio consente di creare un'applicazione Azure AD dalla quale è possibile gestire l'accesso dell'utente del gateway e dell'amministratore del gateway.

Se si vuole assegnare a utenti specifici Azure AD o gruppi l'accesso dell'utente gateway o dell'amministratore del gateway al servizio centro di amministrazione di Windows, è necessario eseguire le operazioni seguenti:

1.  Passare all'applicazione PMI Azure AD nel portale di Azure. 
    -   Quando si fa clic su **modifica controllo di accesso** e quindi si seleziona **Azure Active Directory** dalle impostazioni di accesso dell'interfaccia di amministrazione di Windows, è possibile usare il collegamento ipertestuale disponibile nell'interfaccia utente per accedere all'applicazione Azure ad nel portale di Azure. Questo collegamento ipertestuale è disponibile anche nelle impostazioni di accesso dopo aver fatto clic su Salva ed è stato selezionato Azure AD come provider di identità del controllo di accesso.
    -   È anche possibile trovare l'applicazione nel portale di Azure accedendo a **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni** e cercando le **PMI** (l'app Azure ad verrà denominata SME-<gateway>). Se non vengono visualizzati risultati di ricerca, verificare che **Mostra** sia impostato su **tutte le applicazioni**, che **lo stato dell'applicazione** sia impostato su **qualsiasi** , quindi fare clic su Applica, quindi provare la ricerca. Dopo aver trovato l'applicazione, passare a **utenti e gruppi**
2.  Nella scheda Proprietà impostare **assegnazione utente obbligatoria** su Sì.
    Una volta completata questa operazione, solo i membri elencati nella scheda **utenti e gruppi** saranno in grado di accedere al gateway dell'interfaccia di amministrazione di Windows.
3.  Nella scheda utenti e gruppi selezionare **Aggiungi utente**. È necessario assegnare un ruolo utente gateway o amministratore gateway per ogni utente/gruppo aggiunto.

Dopo aver salvato il controllo di accesso Azure AD nel riquadro **Change Access Control** , il servizio gateway viene riavviato ed è necessario aggiornare il browser. È possibile aggiornare l'accesso utente per il centro di amministrazione di Windows Azure AD applicazione nel portale di Azure in qualsiasi momento. 

Agli utenti verrà richiesto di accedere con l'identità del Azure Active Directory quando tenteranno di accedere all'URL del gateway dell'interfaccia di amministrazione di Windows. Tenere presente che gli utenti devono essere anche membri degli utenti locali nel server gateway per accedere all'interfaccia di amministrazione di Windows. 

Usando la scheda **Azure** delle impostazioni generali dell'interfaccia di amministrazione di Windows, gli utenti e gli amministratori possono visualizzare il proprio account attualmente connesso e la disconnessione di questo account Azure ad.

### <a name="conditional-access-and-multi-factor-authentication"></a>Accesso condizionale e autenticazione a più fattori

Uno dei vantaggi dell'uso di Azure AD come un ulteriore livello di sicurezza per controllare l'accesso al gateway dell'interfaccia di amministrazione di Windows è la possibilità di sfruttare le potenti funzionalità di sicurezza di Azure AD, ad esempio l'accesso condizionale e l'autenticazione a più fattori. 

[Altre informazioni sulla configurazione dell'accesso condizionale con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configura l'accesso Single Sign-On

**Single Sign-on quando distribuito come servizio in Windows Server**

Quando si installa l'interfaccia di amministrazione di Windows in Windows 10, è possibile usare Single Sign-On. Se si intende usare l'interfaccia di amministrazione di Windows in Windows Server, è tuttavia necessario configurare una qualche forma di delega Kerberos nell'ambiente prima di poter usare Single Sign-On. La delega configura il computer gateway come attendibile per la delega al nodo di destinazione. 

Per configurare la [delega vincolata basata sulle risorse](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) nell'ambiente in uso, eseguire i cmdlet di PowerShell seguenti. Tenere presente che è necessario un controller di dominio che esegue Windows Server 2012 o versione successiva.

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

In questo esempio il gateway dell'interfaccia di amministrazione di Windows è installato nel server **WindowsAdminCenterGW**e il nome del nodo di destinazione è **ManagedNode**.

Per rimuovere questa relazione, eseguire il cmdlet seguente:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

Il controllo degli accessi in base al ruolo consente di fornire agli utenti un accesso limitato al computer invece di renderli completi per gli amministratori locali.
[Scopri di più sul controllo degli accessi in base al ruolo e sui ruoli disponibili.](../plan/user-access-options.md#role-based-access-control)

La configurazione del controllo degli accessi in base al ruolo è costituita da 2 passaggi: abilitazione del supporto nei computer di destinazione e assegnazione di utenti ai ruoli pertinenti.

> [!TIP]
> Assicurarsi di disporre di privilegi di amministratore locale nei computer in cui si sta configurando il supporto per il controllo degli accessi in base al ruolo.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Applicare il controllo degli accessi in base al ruolo a un singolo computer

Il modello di distribuzione a computer singolo è ideale per ambienti semplici con pochi computer da gestire.
La configurazione di un computer con supporto per il controllo degli accessi in base al ruolo comporterà le seguenti modifiche:

-   I moduli di PowerShell con funzioni richieste dall'interfaccia di amministrazione di Windows verranno installati nell'unità di sistema, in `C:\Program Files\WindowsPowerShell\Modules`. Tutti i moduli inizieranno con **Microsoft. SME**
-   Desired state Configuration eseguirà una configurazione unica per configurare un endpoint di amministrazione sufficiente nel computer, denominato **Microsoft. SME. PowerShell**. Questo endpoint definisce i 3 ruoli usati dall'interfaccia di amministrazione di Windows e viene eseguito come amministratore locale temporaneo quando un utente si connette a esso.
-   verranno creati 3 nuovi gruppi locali per controllare a quali utenti viene assegnato l'accesso ai ruoli:
    -   Amministratori di Windows Admin Center
    -   Amministratori di Hyper-V in Windows Admin Center
    -   Lettori del centro di amministrazione di Windows

Per abilitare il supporto per il controllo degli accessi in base al ruolo in un singolo computer, seguire questa procedura:

1.  Aprire l'interfaccia di amministrazione di Windows e connettersi alla macchina che si vuole configurare con il controllo degli accessi in base al ruolo usando un account con privilegi di amministratore locale nel computer di destinazione.
2.  Nello strumento **Panoramica** fare clic su **Impostazioni** > **controllo degli accessi in base al ruolo**.
3.  Fare clic su **applica** nella parte inferiore della pagina per abilitare il supporto per il controllo degli accessi in base al ruolo nel computer di destinazione. Il processo dell'applicazione comporta la copia di script di PowerShell e la chiamata di una configurazione (usando PowerShell DSC (Desired state Configuration) nel computer di destinazione. Il completamento dell'operazione potrebbe richiedere fino a 10 minuti e verrà riavviato WinRM. L'interfaccia di amministrazione di Windows, PowerShell e gli utenti WMI verrà temporaneamente disconnessa.
4.  Aggiornare la pagina per verificare lo stato del controllo degli accessi in base al ruolo. Quando è pronto per l'uso, lo stato viene modificato in **applicato**.

Una volta applicata la configurazione, è possibile assegnare gli utenti ai ruoli:

1.  Aprire lo strumento **utenti e gruppi locali** e passare alla scheda **gruppi** .
2.  Selezionare il gruppo **Readers** dell'interfaccia di amministrazione di Windows.
3.  Nel riquadro dei *Dettagli* nella parte inferiore fare clic su **Aggiungi utente** e immettere il nome di un utente o di un gruppo di sicurezza che deve avere accesso in sola lettura al server tramite l'interfaccia di amministrazione di Windows. Gli utenti e i gruppi possono provenire dal computer locale o dal dominio Active Directory.
4.  Ripetere i passaggi 2-3 per **i gruppi amministratori** **Hyper-V** amministratori di Windows e amministratori di Windows Admin Center.

È anche possibile compilare questi gruppi in modo coerente nel dominio configurando un oggetto Criteri di gruppo con l' [impostazione dei criteri gruppi limitati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Applicare il controllo degli accessi in base al ruolo a più computer

In una distribuzione aziendale di grandi dimensioni, è possibile usare gli strumenti di automazione esistenti per eseguire il push della funzionalità di controllo degli accessi in base al ruolo nei computer scaricando il pacchetto di configurazione dal gateway dell'interfaccia di amministrazione di Windows.
Il pacchetto di configurazione è progettato per essere usato con la configurazione dello stato desiderato di PowerShell, ma è possibile adattarlo per funzionare con la soluzione di automazione preferita.

#### <a name="download-the-role-based-access-control-configuration"></a>Scaricare la configurazione del controllo degli accessi in base al ruolo

Per scaricare il pacchetto di configurazione del controllo degli accessi in base al ruolo, è necessario avere accesso all'interfaccia di amministrazione di Windows e a un prompt di PowerShell.

Se si sta eseguendo il gateway dell'interfaccia di amministrazione di Windows in modalità servizio in Windows Server, usare il comando seguente per scaricare il pacchetto di configurazione.
Assicurarsi di aggiornare l'indirizzo del gateway con quello corretto per l'ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se si esegue il gateway dell'interfaccia di amministrazione di Windows nel computer Windows 10, eseguire invece il comando seguente:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Quando si espande l'archivio zip, viene visualizzata la struttura di cartelle seguente:

- InstallJeaFeatures. ps1
- JustEnoughAdministration (directory)
- Moduli (directory)
    - Microsoft. SME. \* (directory)
    - WindowsAdminCenter. Jea (directory)

Per configurare il supporto per il controllo degli accessi in base al ruolo in un nodo, è necessario eseguire le azioni seguenti:

1.  Copiare i moduli JustEnoughAdministration, Microsoft. SME. \* e WindowsAdminCenter. Jea nella directory del modulo di PowerShell nel computer di destinazione. Si trova in genere in `C:\Program Files\WindowsPowerShell\Modules`.
2.  Aggiornare il file **InstallJeaFeature. ps1** in modo che corrisponda alla configurazione desiderata per l'endpoint RBAC.
3.  Eseguire InstallJeaFeature. ps1 per compilare la risorsa DSC.
4.  Distribuire la configurazione DSC in tutti i computer per applicare la configurazione.

La sezione seguente illustra come eseguire questa operazione usando la comunicazione remota di PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Distribuisci in più computer

Per distribuire la configurazione scaricata su più computer, è necessario aggiornare lo script **InstallJeaFeatures. ps1** in modo da includere i gruppi di sicurezza appropriati per l'ambiente, copiare i file in ogni computer e richiamare il cmdlet script di configurazione.
Per ottenere questo risultato, è possibile usare gli strumenti di automazione preferiti, tuttavia questo articolo si concentra su un approccio basato su PowerShell.

Per impostazione predefinita, lo script di configurazione creerà gruppi di sicurezza locali nel computer per controllare l'accesso a ogni ruolo.
Questo è adatto per i computer del gruppo di lavoro e del dominio, ma se si esegue la distribuzione in un ambiente di solo dominio, è possibile associare direttamente un gruppo di sicurezza di dominio a ogni ruolo.
Per aggiornare la configurazione per l'utilizzo dei gruppi di sicurezza di dominio, aprire **InstallJeaFeatures. ps1** e apportare le modifiche seguenti:

1.  Rimuovere le 3 risorse del **gruppo** dal file:
    1.  "Gruppo MS-Readers-Group"
    2.  "Gruppo MS-Hyper-V-Administrators-gruppo"
    3.  "Gruppo MS-Administrators-gruppo"
2.  Rimuovere le 3 risorse del gruppo dalla proprietà **DependsOn** di JeaEndpoint
    1.  "[Gruppo] MS-Readers-Group"
    2.  "[Gruppo] MS-Hyper-V-Administrators-gruppo"
    3.  "[Gruppo] MS-Administrators-gruppo"
3.  Modificare i nomi dei gruppi nella proprietà JeaEndpoint **RoleDefinitions** nei gruppi di sicurezza desiderati. Ad esempio, se si dispone di un gruppo di sicurezza *CONTOSO\MyTrustedAdmins* a cui deve essere assegnato l'accesso al ruolo di amministratore di Windows Admin Center, modificare `'$env:COMPUTERNAME\Windows Admin Center Administrators'` in `'CONTOSO\MyTrustedAdmins'`. Le tre stringhe che è necessario aggiornare sono:
    1.  ' $env: amministratori di COMPUTERNAME\Windows Admin Center '
    2.  ' $env: amministratori di Hyper-V di COMPUTERNAME\Windows Admin Center
    3.  ' $env: lettori dell'interfaccia di amministrazione di COMPUTERNAME\Windows '

> [!NOTE]
> Assicurarsi di usare gruppi di sicurezza univoci per ogni ruolo. La configurazione avrà esito negativo se lo stesso gruppo di sicurezza viene assegnato a più ruoli.

Alla fine del file **InstallJeaFeatures. ps1** aggiungere quindi le righe seguenti di PowerShell alla fine dello script:

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

Infine, è possibile copiare la cartella che contiene i moduli, la risorsa DSC e la configurazione in ogni nodo di destinazione ed eseguire lo script **InstallJeaFeature. ps1** .
Per eseguire questa operazione in modalità remota dalla workstation di amministrazione, è possibile eseguire i comandi seguenti:

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
