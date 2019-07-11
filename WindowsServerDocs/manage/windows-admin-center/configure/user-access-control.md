---
title: Configurazione di controllo di accesso utente e autorizzazioni
description: Informazioni su come configurare controllo di accesso utente e autorizzazioni usando Active Directory o Azure AD (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ef87a3bcc5bd0b924a938f055307a0a87cb60d0b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792323"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurare le autorizzazioni e controllo di accesso utente

> Si applica a Windows Admin Center, Windows Admin Center Preview

Se hai già fatto, acquisire familiarità con la [le opzioni di controllo di accesso utente in Windows Admin Center](../plan/user-access-options.md)

> [!NOTE]
> Accesso basato su gruppo in Windows Admin Center non è supportato in ambienti gruppi di lavoro o tra domini non trusted.

## <a name="gateway-access-role-definitions"></a>Definizioni di ruolo di accesso di gateway

Sono disponibili due ruoli per l'accesso al servizio del gateway Windows Admin Center:

**Gli utenti di gateway** può connettersi al servizio del gateway Windows Admin Center per gestire i server tramite tale gateway, ma non possono modificare le autorizzazioni di accesso né il meccanismo di autenticazione usato per l'autenticazione del gateway.

**Gli amministratori di gateway** configurabili chi ottiene anche l'accesso come autenticare gli utenti al gateway. Solo gli amministratori di gateway possono visualizzare e configurare le impostazioni di accesso di Windows Admin Center. Gli amministratori locali nel computer del gateway sono sempre gli amministratori del servizio del gateway Windows Admin Center.

> [!NOTE]
> Accesso al gateway non implica l'accesso ai server gestiti visibile dal gateway. Per gestire un server di destinazione, l'utente che esegue la connessione deve usare le credenziali (tramite le proprie credenziali di Windows pass-through o le credenziali specificate nella sessione di Windows Admin Center utilizzando la **gestire come** azione) che hanno accesso amministrativo al server di destinazione.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory o i gruppi di computer locale

Per impostazione predefinita, Active Directory o i gruppi di computer locale vengono usati per controllare l'accesso al gateway. Se si dispone di un dominio Active Directory, è possibile gestire gateway utenti e amministratori accesso dall'interno dell'interfaccia di Windows Admin Center.

Nel **utenti** scheda è possibile controllare chi può accedere a Windows Admin Center come utente di gateway. Per impostazione predefinita, e se non si specifica un gruppo di sicurezza, qualsiasi utente che accede l'URL del gateway può accedere. Dopo aver aggiunto uno o più gruppi di sicurezza all'elenco di utenti, l'accesso è limitato ai membri di tali gruppi.

Se non si usa un dominio Active Directory nell'ambiente in uso, l'accesso viene controllato per il `Users` e `Administrators` gruppi locali nel computer del gateway Windows Admin Center.

### <a name="smartcard-authentication"></a>Autenticazione tramite smart card

È possibile imporre **autenticazione tramite smart card** specificando un ulteriore _obbligatorio_ gruppo per i gruppi di sicurezza basata su smart card. Dopo aver aggiunto un gruppo di sicurezza basata su smart card, un utente può accedere solo il servizio Windows Admin Center se sono membri di gruppi di sicurezza di un gruppo di smart card incluso nell'elenco di utenti.

Nel **gli amministratori** scheda è possibile controllare chi può accedere a Windows Admin Center come un amministratore del gateway. Il gruppo administrators locale nel computer avrà sempre accesso amministratore completo e non può essere rimosso dall'elenco. Quando si aggiungono gruppi di sicurezza, assegnare i membri di tali privilegi di gruppi per modificare le impostazioni del gateway Windows Admin Center. L'elenco di amministratori supporta l'autenticazione della smart card nello stesso modo come elenco di utenti: con la condizione AND per un gruppo di sicurezza e un gruppo di smart card.

## <a name="azure-active-directory"></a>Azure Active Directory

Se l'organizzazione Usa Azure Active Directory (Azure AD), è possibile scegliere di aggiungere un **aggiuntive** livello di sicurezza per Windows Admin Center richiedendo l'autenticazione di Azure AD per accedere al gateway. Per poter accedere a Windows Admin Center, l'utente **account di Windows** deve anche avere accesso al server gateway (anche se viene utilizzata l'autenticazione di Azure AD). Quando si usa Azure AD, si gestiranno le autorizzazioni di accesso utente e amministratore Windows Admin Center dal portale di Azure, piuttosto che dall'interno dell'interfaccia utente di Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>L'accesso a Windows Admin Center quando è abilitata l'autenticazione di Azure AD

In base al browser utilizzato, alcuni utenti che accedono a Windows Admin Center con l'autenticazione di Azure AD configurato verranno visualizzato un prompt aggiuntivo **dal browser** in cui è necessario fornire le credenziali di account di Windows per la macchina in cui è installato Windows Admin Center. Dopo aver immesso le informazioni, gli utenti otterranno la richiesta di autenticazione Azure Active Directory aggiuntiva, che richiede le credenziali di un account Azure a cui è stato concesso l'accesso nell'applicazione Azure AD in Azure.

> [!NOTE]
> Gli utenti in cui Windows account abbia **diritti di amministratore** nel gateway macchina non verrà richiesto per l'autenticazione di Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configurazione dell'autenticazione di Azure Active Directory per Windows Admin Center anteprima

Passare a Windows Admin Center **le impostazioni** > **accesso** e utilizzare l'opzione di attivazione/disattivazione per attivare "usare Azure Active Directory per aggiungere un livello di sicurezza per il gateway". Se non è stato registrato il gateway in Azure, si passerà a tale scopo in questo momento.

Per impostazione predefinita, tutti i membri del tenant di Azure AD hanno accesso utente al servizio del gateway Windows Admin Center. Solo gli amministratori locali nel computer del gateway hanno accesso di amministratore per il gateway di Windows Admin Center. Si noti che i diritti del gruppo administrators locale nel computer del gateway non possono essere limitati agli amministratori locali possono eseguire qualsiasi operazione indipendentemente dal fatto che Azure AD viene usato per l'autenticazione.

Se si desidera specifico di Azure AD concede agli utenti o gruppi gateway utente o l'accesso amministratore gateway al servizio Windows Admin Center, è necessario eseguire quanto segue:

1.  Passare all'applicazione Windows Admin Center di Azure AD nel portale di Azure usando il collegamento ipertestuale fornito nelle impostazioni di accesso. Si noti che il collegamento ipertestuale è disponibile solo quando è abilitata l'autenticazione di Azure Active Directory. 
    -   È anche possibile trovare l'applicazione nel portale di Azure visitando **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni** ed eseguendo ricerche **WindowsAdminCenter** (l'app Azure AD verrà denominato WindowsAdminCenter -<gateway name>). Se non si ottengono risultati di ricerca, assicurarsi **mostrare** è impostata su **tutte le applicazioni**, **lo stato dell'applicazione** è impostata su **qualsiasi** e fare clic su Applica, Provare la ricerca. Dopo aver trovato l'applicazione, passare a **utenti e gruppi**
2.  Nella scheda proprietà, impostare **assegnazione utente obbligatoria** su Sì.
    Dopo aver eseguito questo, solo i membri elencati nella **utenti e gruppi** scheda sarà in grado di accedere al gateway di Windows Admin Center.
3.  Nella scheda gruppi e utenti, selezionare **Add user**. È necessario assegnare un utente di gateway o un ruolo di amministratore di gateway per ogni utente o gruppo aggiunto.

Una volta che si attiva l'autenticazione di Azure AD, il riavvio del servizio gateway e si deve aggiornare il browser. È possibile aggiornare l'accesso utente per l'applicazione di esperti di dominio Azure AD nel portale di Azure in qualsiasi momento.

Gli utenti verranno richiesto di accedere usando l'identità di Azure Active Directory quando tentano di accedere all'URL del gateway Windows Admin Center. Tenere presente che gli utenti devono essere anche un membro degli utenti locali nel server gateway per accedere a Windows Admin Center.

Utenti e amministratori possono visualizzare l'account connesso e anche di disconnessione dell'account di Azure Active Directory dal **Account** scheda delle impostazioni di Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configurazione dell'autenticazione di Azure Active Directory per Windows Admin Center

[Per configurare l'autenticazione di Azure AD, è prima necessario registrare il gateway con Azure](azure-integration.md) (è sufficiente eseguire questa operazione una sola volta per il gateway di Windows Admin Center). Questo passaggio viene creata un'applicazione di Azure AD da cui è possibile gestire gateway utente e accedere come amministratore gateway.

Se si desidera specifico di Azure AD concede agli utenti o gruppi gateway utente o l'accesso amministratore gateway al servizio Windows Admin Center, è necessario eseguire quanto segue:

1.  Passare all'applicazione di esperti di dominio Azure AD nel portale di Azure. 
    -   Quando fa clic su **controllo di accesso di modifica** e quindi selezionare **Azure Active Directory** dalle impostazioni dell'accesso di Windows Admin Center, è possibile usare il collegamento fornito nell'interfaccia utente di accesso di Azure AD applicazione nel portale di Azure. Questo collegamento ipertestuale è disponibile nella finestra Impostazioni accesso anche dopo aver fare clic su Salva e aver selezionato Azure AD come provider di identità di controllo di accesso.
    -   È anche possibile trovare l'applicazione nel portale di Azure visitando **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni** ed eseguendo ricerche **SME** (l'app Azure AD verrà denominato SME -<gateway>). Se non si ottengono risultati di ricerca, assicurarsi **mostrare** è impostata su **tutte le applicazioni**, **lo stato dell'applicazione** è impostata su **qualsiasi** e fare clic su Applica, Provare la ricerca. Dopo aver trovato l'applicazione, passare a **utenti e gruppi**
2.  Nella scheda proprietà, impostare **assegnazione utente obbligatoria** su Sì.
    Dopo aver eseguito questo, solo i membri elencati nella **utenti e gruppi** scheda sarà in grado di accedere al gateway di Windows Admin Center.
3.  Nella scheda gruppi e utenti, selezionare **Add user**. È necessario assegnare un utente di gateway o un ruolo di amministratore di gateway per ogni utente o gruppo aggiunto.

Dopo il salvataggio di Azure AD controllo degli accessi nel **controllo di accesso di modifica** riquadro riavvia il servizio gateway e deve aggiornare il browser. È possibile aggiornare l'accesso utente per l'applicazione Windows Admin Center di Azure AD nel portale di Azure in qualsiasi momento. 

Gli utenti verranno richiesto di accedere usando l'identità di Azure Active Directory quando tentano di accedere all'URL del gateway Windows Admin Center. Tenere presente che gli utenti devono essere anche un membro degli utenti locali nel server gateway per accedere a Windows Admin Center. 

Usando il **Azure** scheda delle impostazioni generali di Windows Admin Center, gli utenti e gli amministratori possa visualizzare l'account connesso e anche di disconnessione dell'account di Azure Active Directory.

### <a name="conditional-access-and-multi-factor-authentication"></a>Accesso condizionale e multi-factor authentication

Uno dei vantaggi dell'uso di Azure AD come un ulteriore livello di sicurezza per controllare l'accesso al gateway di Windows Admin Center è che è possibile usare potenti funzionalità di sicurezza Azure AD, ad esempio l'accesso condizionale e multi-factor authentication. 

[Altre informazioni sulla configurazione dell'accesso condizionale con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configura l'accesso Single Sign-On

**Accesso Single sign-on quando viene distribuita come servizio in Windows Server**

Quando si installa Windows Admin Center su Windows 10, è possibile usare single sign-on. Se si intende usare Windows Admin Center su Windows Server, tuttavia, è necessario configurare una forma di delega Kerberos nell'ambiente in uso prima di poter usare single sign-on. La delega consente di configurare il computer gateway come attendibili per la delega al nodo di destinazione. 

Per configurare [basata su risorse per la delega vincolata](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) nell'ambiente in uso, eseguire i cmdlet di PowerShell seguenti. (Utilizzare tenere presente che questa operazione richiede un controller di dominio che esegue Windows Server 2012 o versione successiva).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

In questo esempio, il gateway di Windows Admin Center è installato nel server **WindowsAdminCenterGW**, e il nome del nodo di destinazione viene **ManagedNode**.

Per rimuovere questa relazione, eseguire il cmdlet seguente:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

Controllo degli accessi in base al ruolo consente di fornire agli utenti con accesso limitato al computer anziché lasciare che tali amministratori locali completo.
[Per ulteriori informazioni su controllo degli accessi in base al ruolo e i ruoli disponibili.](../plan/user-access-options.md#role-based-access-control)

Configurazione di RBAC è costituito da 2 passaggi: abilitazione del supporto nei computer di destinazione e l'assegnazione di utenti ai ruoli rilevanti.

> [!TIP]
> Assicurarsi di avere i privilegi di amministratore locale nei computer in cui si sta configurando il supporto per controllo degli accessi in base al ruolo.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Applicare il controllo di accesso basato sui ruoli a un singolo computer

Il modello di distribuzione singolo computer è ideale per ambienti più semplici con solo pochi computer da gestire.
Configurazione di una macchina con il supporto per controllo degli accessi in base al ruolo comporterà le modifiche seguenti:

-   I moduli di PowerShell con le funzioni richiesti da Windows Admin Center verranno installati nell'unità di sistema, in `C:\Program Files\WindowsPowerShell\Modules`. Tutti i moduli inizieranno con **Microsoft.Sme**
-   Desired State Configuration eseguirà una configurazione una tantum per configurare un endpoint di Just Enough Administration nel computer, denominato **Microsoft.Sme.PowerShell**. Questo endpoint definisce i 3 ruoli utilizzati da Windows Admin Center e verrà eseguito come amministratore locale temporaneo quando un utente si connette a esso.
-   Per controllare quali utenti con accesso a quali ruoli verranno creati 3 nuovi gruppi locali:
    -   Amministratori Windows Admin Center
    -   Amministratori di Hyper-V Windows Admin Center
    -   Lettori Windows Admin Center

Per abilitare il supporto per il controllo di accesso basato sui ruoli in un singolo computer, seguire questa procedura:

1.  Aprire Windows Admin Center e connettersi al computer che si desidera configurare con controllo degli accessi basato su ruolo con un account con privilegi di amministratore locale nel computer di destinazione.
2.  Nel **Overview** dello strumento, fare clic su **impostazioni** > **controllo degli accessi in base al ruolo**.
3.  Fare clic su **applica** nella parte inferiore della pagina per abilitare il supporto per il controllo di accesso basato sui ruoli nel computer di destinazione. Il processo dell'applicazione comporta la copia degli script di PowerShell e richiamare una configurazione (tramite PowerShell Desired State Configuration) nel computer di destinazione. Potrebbero occorrere fino a 10 minuti e comporterà il riavvio di WinRM. Si disconnette temporaneamente agli utenti di Windows Admin Center, PowerShell e WMI.
4.  Aggiornare la pagina per controllare lo stato del controllo degli accessi in base al ruolo. Quando è pronto per l'uso, lo stato verrà modificato in **applicato**.

Dopo aver applicata la configurazione, è possibile assegnare utenti ai ruoli:

1.  Aprire il **utenti e gruppi locali** dello strumento e passare al **gruppi** scheda.
2.  Selezionare il **Windows Admin Center lettori** gruppo.
3.  Nel *informazioni dettagliate* riquadro nella parte inferiore, fare clic su **Add User** e immettere il nome di un gruppo di utenti o di sicurezza che deve disporre di accesso in lettura al server tramite Windows Admin Center. Gli utenti e gruppi possono provenire da computer locale o dominio di Active Directory.
4.  Ripetere i passaggi 2 e 3 per il **gli amministratori di Hyper-V di Windows Admin Center** e **gli amministratori di Windows Admin Center** gruppi.

È inoltre possibile compilare questi gruppi in modo coerente in tutto il dominio tramite la configurazione di un oggetto Criteri di gruppo con il [impostazione del criterio gruppi con restrizioni](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Applicare il controllo di accesso basato sui ruoli in più computer

In una distribuzione aziendale di grandi dimensioni, è possibile utilizzare gli strumenti di automazione esistenti per eseguire il push la funzionalità di controllo di accesso basato sui ruoli per i computer, scaricare il pacchetto di configurazione dal gateway di Windows Admin Center.
Il pacchetto di configurazione è progettato per essere usato con PowerShell Desired State Configuration, ma è possibile adattare il funzionamento con la soluzione di automazione Preferiti.

#### <a name="download-the-role-based-access-control-configuration"></a>Scaricare la configurazione di controllo di accesso basato sui ruoli

Per scaricare il pacchetto di configurazione di controllo di accesso basato sui ruoli, è necessario avere accesso a un prompt di PowerShell e Windows Admin Center.

Se si esegue il gateway di Windows Admin Center in modalità servizio, in Windows Server, usare il comando seguente per scaricare il pacchetto di configurazione.
Assicurarsi di aggiornare l'indirizzo del gateway con quella corretta per l'ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se si esegue il gateway di Windows Admin Center nel computer Windows 10, eseguire invece il comando seguente:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Quando si espande l'archivio zip, si noterà la struttura di cartelle seguente:

- InstallJeaFeatures.ps1
- JustEnoughAdministration (directory)
- Moduli (directory)
    - Microsoft.SME. \* (Directory)
    - WindowsAdminCenter.Jea (directory)

Per configurare il supporto per il controllo di accesso basato sui ruoli in un nodo, è necessario eseguire le azioni seguenti:

1.  Copiare il JustEnoughAdministration, Microsoft.SME. \*e i moduli WindowsAdminCenter.Jea per la directory del modulo PowerShell nel computer di destinazione. Si tratta in genere disponibile all'indirizzo `C:\Program Files\WindowsPowerShell\Modules`.
2.  Update **InstallJeaFeature.ps1** file in modo che corrisponda alla configurazione desiderata per l'endpoint RBAC.
3.  Eseguire InstallJeaFeature.ps1 per compilare la risorsa DSC.
4.  Distribuire la configurazione DSC a tutti i computer per applicare la configurazione.

La sezione seguente viene illustrato come eseguire questa operazione tramite comunicazione remota di PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Distribuire in più computer

Per distribuire la configurazione è stato scaricato in più computer, è necessario aggiornare il **InstallJeaFeatures.ps1** script per includere i gruppi di sicurezza appropriati per l'ambiente, copiare i file in ogni computer, e richiamare gli script di configurazione.
È possibile usare degli strumenti di automazione Preferiti per eseguire questa operazione, tuttavia, questo articolo è incentrato su un approccio basato su PowerShell puro.

Per impostazione predefinita, lo script di configurazione crea gruppi di sicurezza locale nel computer per controllare l'accesso a ciascuno dei ruoli.
Ciò è ideale per gruppo di lavoro e computer appartenenti al dominio, ma se si esegue la distribuzione in un ambiente di dominio di sola lettura si potrebbe voler direttamente associare un gruppo di sicurezza di dominio a ogni ruolo.
Per aggiornare la configurazione per usare i gruppi di sicurezza di dominio, aprire **InstallJeaFeatures.ps1** e apportare le modifiche seguenti:

1.  Rimuovere il 3 **gruppo** risorse dal file:
    1.  "Gruppo di MS-lettori-Group"
    2.  "Gruppo di MS-Hyper-V-gli amministratori-Group"
    3.  "Gruppo di amministratori gruppo MS"
2.  Rimuovere il gruppo di 3 risorse dal JeaEndpoint **DependsOn** proprietà
    1.  "[Raggruppare] MS-lettori-Group"
    2.  "[Raggruppare] MS-Hyper-V-gli amministratori-Group"
    3.  "[Raggruppare] MS-gli amministratori-Group"
3.  Modificare i nomi del gruppo di JeaEndpoint **RoleDefinitions** proprietà ai gruppi di sicurezza desiderato. Ad esempio, se si dispone di un gruppo di sicurezza *CONTOSO\MyTrustedAdmins* che deve essere assegnato l'accesso al ruolo Administrators di Windows Admin Center, change `'$env:COMPUTERNAME\Windows Admin Center Administrators'` a `'CONTOSO\MyTrustedAdmins'`. Le tre stringhe, che è necessario aggiornare sono:
    1.  ' $env: degli amministratori di COMPUTERNAME\Windows Admin Center
    2.  ' $env: degli amministratori di COMPUTERNAME\Windows Admin Center Hyper-V
    3.  ' $env: COMPUTERNAME\Windows Admin Center Readers'

> [!NOTE]
> Assicurarsi di usare i gruppi di sicurezza univoco per ogni ruolo. Configurazione avrà esito negativo se il gruppo di sicurezza stesso viene assegnato a più ruoli.

Successivamente, alla fine del **InstallJeaFeatures.ps1** , aggiungere le righe di PowerShell seguenti nella parte inferiore dello script:

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

Infine, è possibile copiare la cartella che contiene i moduli, risorse DSC e configurazione per ogni nodo di destinazione ed eseguire la **InstallJeaFeature.ps1** script.
A questo scopo in modalità remota dalla workstation di amministratore, è possibile eseguire i comandi seguenti:

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
