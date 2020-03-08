---
title: Configurazione delle autorizzazioni e del controllo di accesso utente
description: Informazioni su come configurare le autorizzazioni e il controllo di accesso utente usando Active Directory o Azure AD (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 39af45506ff7023cebe437992e90f6d4ec051333
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371713"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurare le autorizzazioni e il controllo di accesso utente

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Acquisisci familiarità con le [opzioni di controllo di accesso utente in Windows Admin Center](../plan/user-access-options.md) se non l'hai già fatto

> [!NOTE]
> L'accesso basato sui gruppi in Windows Admin Center non è supportato negli ambienti di gruppo di lavoro o nei domini non attendibili.

## <a name="gateway-access-role-definitions"></a>Definizioni dei ruoli di accesso al gateway

Per l'accesso al servizio gateway di Windows Admin Center sono disponibili due ruoli:

Gli **utenti del gateway** possono connettersi al servizio gateway di Windows Admin Center per gestire i server attraverso tale gateway, ma non possono modificare le autorizzazioni di accesso e il meccanismo usato per l'autenticazione al gateway.

Gli **amministratori del gateway** possono configurare gli utenti autorizzati ad accedere al servizio gateway e la relativa modalità di autenticazione. Solo gli amministratori del gateway possono visualizzare e configurare le impostazioni di accesso in Windows Admin Center. Gli amministratori locali del computer gateway sono sempre amministratori del servizio gateway di Windows Admin Center.

> [!NOTE]
> L'accesso al gateway non implica l'accesso ai server gestiti visibili dal gateway. Per gestire un server di destinazione, l'utente che si connette deve usare credenziali con accesso amministrativo al server di destinazione (le relative credenziali pass-through di Windows o quelle fornite nella sessione di Windows Admin Center tramite il comando **Account di gestione**).

## <a name="active-directory-or-local-machine-groups"></a>Active Directory o gruppi di computer locali

Per impostazione predefinita, per controllare l'accesso al gateway vengono usati i gruppi di computer locali o Active Directory. Se disponi di un dominio di Active Directory, puoi gestire l'accesso di utenti e amministratori del gateway dall'interfaccia di Windows Admin Center.

Nella scheda **Utenti** puoi controllare gli utenti autorizzati ad accedere a Windows Admin Center come utenti del gateway. Per impostazione predefinita, e se non specifichi un gruppo di sicurezza, possono eseguire l'accesso tutti gli utenti autorizzati ad accedere all'URL del gateway. Dopo che hai aggiunto uno o più gruppi di sicurezza all'elenco degli utenti, l'accesso sarà limitato ai membri di tali gruppi.

Se nel tuo ambiente non è configurato un dominio di Active Directory, l'accesso viene controllato dai gruppi locali `Users` e `Administrators` nel computer gateway di Windows Admin Center.

### <a name="smartcard-authentication"></a>Autenticazione tramite smart card

Puoi imporre l'**autenticazione tramite smart card** specificando un gruppo _obbligatorio_ aggiuntivo per i gruppi di sicurezza basati su smart card. Dopo che hai aggiunto un gruppo di sicurezza basato su smart card, un utente potrà accedere al servizio di Windows Admin Center solo se è membro di un gruppo di sicurezza E di un gruppo basato su smart card incluso nell'elenco degli utenti.

Nella scheda **Amministratori** puoi controllare gli utenti autorizzati ad accedere a Windows Admin Center come amministratori del gateway. Il gruppo Administrators locale nel computer disporrà sempre dell'accesso amministratore completo e non potrà essere rimosso dall'elenco. Aggiungendo gruppi di sicurezza, assegni ai membri di tali gruppi i privilegi necessari per modificare le impostazioni del gateway di Windows Admin Center. L'elenco degli amministratori supporta l'autenticazione tramite smart card in modo analogo all'elenco degli utenti, ovvero con la condizione AND per un gruppo di sicurezza e un gruppo basato su smart card.

## <a name="azure-active-directory"></a>Azure Active Directory

Se nella tua organizzazione viene usato il servizio Azure Active Directory (Azure AD), puoi scegliere di introdurre un livello di sicurezza **aggiuntivo** per Windows Admin Center richiedendo l'autenticazione di Azure AD per l'accesso al gateway. Per accedere a Windows Admin Center, l'**account Windows** dell'utente deve anche avere accesso al server gateway, sebbene venga usata l'autenticazione di Azure AD. Quando viene usato il servizio Azure AD, le autorizzazioni di accesso di utenti e amministratori di Windows Admin Center vengono gestite dal portale di Azure anziché dall'interfaccia utente di Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Accesso a Windows Admin Center con l'autenticazione di Azure AD abilitata

A seconda del browser usato, alcuni utenti che accedono a Windows Admin Center con l'autenticazione di Azure AD configurata riceveranno un messaggio aggiuntivo **dal browser** in cui verrà chiesto di specificare le credenziali dell'account Windows per il computer in cui è installato Windows Admin Center. Dopo aver immesso queste informazioni, gli utenti riceveranno una richiesta di autenticazione aggiuntiva di Azure Active Directory, per cui dovranno specificare le credenziali di un account Azure autorizzato ad accedere all'applicazione di Azure AD in Azure.

> [!NOTE]
> Gli utenti che dispongono di un account Windows con **diritti di amministratore** nel computer gateway non riceveranno la richiesta di autenticazione di Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configurazione dell'autenticazione di Azure Active Directory per Windows Admin Center Preview

In Windows Admin Center vai a **Impostazioni** > **Accesso** e usa l'interruttore per abilitare "Use Azure Active Directory to add a layer of security to the gateway" (Usa Azure Active Directory per aggiungere un livello di sicurezza al gateway). Se non hai registrato il gateway in Azure, ti verrà indicato come eseguire l'operazione in questo momento.

Per impostazione predefinita, tutti i membri del tenant di Azure AD dispongono dell'accesso utente al servizio gateway di Windows Admin Center. Solo gli amministratori locali del computer gateway dispongono dell'accesso amministratore a tale servizio. Tieni presente che i diritti degli amministratori locali nel computer gateway non possono essere limitati. Gli amministratori locali possono eseguire qualsiasi operazione indipendentemente dall'uso di Azure AD per l'autenticazione.

Se vuoi assegnare a utenti o gruppi specifici di Azure AD l'accesso come utente o amministratore del servizio gateway di Windows Admin Center, devi eseguire queste operazioni:

1.  Vai all'applicazione Windows Admin Center di Azure AD nel portale di Azure usando il collegamento ipertestuale visualizzato in Impostazioni di accesso. Tieni presente che questo collegamento è disponibile solo quando è abilitata l'autenticazione di Azure Active Directory. 
    -   Puoi anche trovare l'applicazione nel portale di Azure selezionando **Azure Active Directory** > **Applicazioni aziendali** > **Tutte le applicazioni** e cercando **WindowsAdminCenter**. L'app di Azure AD sarà denominata WindowsAdminCenter-<gateway name>. Se non viene restituito alcun risultato, verifica che **Mostra** sia impostato su **Tutte le applicazioni** e che **Stato applicazione** sia impostato su **Qualsiasi**, quindi fai clic su Applica e ripeti la ricerca. Dopo aver trovato l'applicazione, vai a **Utenti e gruppi**.
2.  Nella scheda Proprietà imposta **Assegnazione di utenti obbligatoria** su Sì.
    Al termine di questa operazione, solo i membri elencati nella scheda **Utenti e gruppi** saranno in grado di accedere al gateway di Windows Admin Center.
3.  Nella scheda Utenti e gruppi seleziona **Aggiungi utente**. Devi assegnare un ruolo di utente o amministratore del gateway per ogni utente o gruppo aggiunto.

Dopo che hai abilitato l'autenticazione di Azure AD, il servizio gateway viene riavviato e devi aggiornare il browser. Puoi aggiornare l'accesso utente per l'applicazione SME di Azure AD nel portale di Azure in qualsiasi momento.

Durante il tentativo di accesso all'URL del gateway di Windows Admin Center, gli utenti dovranno eseguire l'accesso con la propria identità di Azure Active Directory. Tieni presente che, per accedere a Windows Admin Center, gli utenti devono essere anche membri del gruppo Utenti locale nel server gateway.

Gli utenti e gli amministratori possono visualizzare il proprio account connesso e anche disconnettersi dall'account Azure AD usando la scheda **Account** nelle impostazioni di Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configurazione dell'autenticazione di Azure Active Directory per Windows Admin Center

[Per configurare l'autenticazione di Azure AD, devi prima registrare il gateway con Azure](azure-integration.md). Questa operazione deve essere eseguita una sola volta per il gateway di Windows Admin Center. Questo passaggio consente di creare un'applicazione di Azure AD da cui è possibile gestire l'accesso per gli utenti e gli amministratori del gateway.

Se vuoi assegnare a utenti o gruppi specifici di Azure AD l'accesso come utente o amministratore del servizio gateway di Windows Admin Center, devi eseguire queste operazioni:

1.  Vai all'applicazione SME di Azure AD nel portale di Azure. 
    -   Quando fai clic su **Change access control** (Modifica controllo di accesso) e quindi selezioni **Azure Active Directory** dalle impostazioni di accesso di Windows Admin Center, puoi usare il collegamento ipertestuale visualizzato nell'interfaccia utente per accedere all'applicazione di Azure AD nel portale di Azure. Questo collegamento ipertestuale è disponibile anche nelle impostazioni di accesso dopo che hai fatto clic su Salva e hai selezionato Azure AD come provider di identità del controllo di accesso.
    -   Puoi anche trovare l'applicazione nel portale di Azure selezionando **Azure Active Directory** > **Applicazioni aziendali** > **Tutte le applicazioni** e cercando **SME**. L'app di Azure AD sarà denominata SME-<gateway>. Se non viene restituito alcun risultato, verifica che **Mostra** sia impostato su **Tutte le applicazioni** e che **Stato applicazione** sia impostato su **Qualsiasi**, quindi fai clic su Applica e ripeti la ricerca. Dopo aver trovato l'applicazione, vai a **Utenti e gruppi**.
2.  Nella scheda Proprietà imposta **Assegnazione di utenti obbligatoria** su Sì.
    Al termine di questa operazione, solo i membri elencati nella scheda **Utenti e gruppi** saranno in grado di accedere al gateway di Windows Admin Center.
3.  Nella scheda Utenti e gruppi seleziona **Aggiungi utente**. Devi assegnare un ruolo di utente o amministratore del gateway per ogni utente o gruppo aggiunto.

Dopo aver salvato il controllo di accesso di Azure AD nel riquadro **Change access control** (Modifica controllo di accesso), il servizio gateway viene riavviato e devi aggiornare il browser. Puoi aggiornare l'accesso utente per l'applicazione Windows Admin Center di Azure AD nel portale di Azure in qualsiasi momento. 

Durante il tentativo di accesso all'URL del gateway di Windows Admin Center, gli utenti dovranno eseguire l'accesso con la propria identità di Azure Active Directory. Tieni presente che, per accedere a Windows Admin Center, gli utenti devono essere anche membri del gruppo Utenti locale nel server gateway. 

Usando la scheda **Azure** nelle impostazioni generali di Windows Admin Center, gli utenti e gli amministratori possono visualizzare il proprio account connesso e anche disconnettersi da questo account Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Accesso condizionale e autenticazione a più fattori

Uno dei vantaggi offerti dall'uso di Azure AD come livello di sicurezza aggiuntivo per controllare l'accesso al gateway di Windows Admin Center consiste nella possibilità di sfruttare le potenti funzionalità di sicurezza di Azure AD, ad esempio l'accesso condizionale e l'autenticazione a più fattori. 

[Altre informazioni sulla configurazione dell'accesso condizionale con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurazione di Single Sign-On

**Accesso Single Sign-On distribuito come servizio in Windows Server**

Quando installi Windows Admin Center in Windows 10, l'applicazione è pronta per l'uso dell'accesso Single Sign-On. Se tuttavia intendi usare Windows Admin Center in Windows Server, devi configurare una qualche forma di delega Kerberos nel tuo ambiente prima di poter usare l'accesso Single Sign-On. La delega configura il computer gateway come attendibile per la delega al nodo di destinazione. 

Per configurare la [delega vincolata basata sulle risorse](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) nel tuo ambiente, usa l'esempio di PowerShell seguente. Questo esempio illustra come configurare Windows Server [node01.contoso.com] per accettare la delega dal gateway dell'interfaccia di amministrazione di Windows [wac.contoso.com] nel dominio contoso.com.

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount (Get-ADComputer wac)
```

Per rimuovere questa relazione, esegui il cmdlet seguente:

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Controllo di accesso in base ai ruoli

Il controllo degli accessi in base al ruolo consente di fornire agli utenti l'accesso limitato al computer invece di concedere loro i privilegi completi di amministratore locale.
[Scopri di più sul controllo degli accessi in base al ruolo e sui ruoli disponibili.](../plan/user-access-options.md#role-based-access-control)

La configurazione del controllo degli accessi in base al ruolo consiste in due passaggi: abilitazione del supporto in uno o più computer di destinazione e assegnazione di utenti ai ruoli pertinenti.

> [!TIP]
> Verifica di disporre dei privilegi di amministratore locale sui computer in cui stai configurando il supporto per il controllo degli accessi in base al ruolo.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Applicare il controllo degli accessi in base al ruolo a un singolo computer

Il modello di distribuzione su singolo computer è ideale per gli ambienti semplici con solo pochi computer da gestire.
La configurazione di un computer con il supporto per il controllo degli accessi in base al ruolo ha come risultato le modifiche seguenti:

-   Nell'unità di sistema, in `C:\Program Files\WindowsPowerShell\Modules`, verranno installati i moduli di PowerShell con le funzioni richieste da Windows Admin Center. Tutti i moduli inizieranno con **Microsoft.Sme**
-   Desired State Configuration eseguirà una singola configurazione per impostare un endpoint JEA (Just Enough Administration), denominato **Microsoft.Sme.PowerShell**, nel computer. Questo endpoint definisce i tre ruoli usati da Windows Admin Center e verrà eseguito come amministratore locale temporaneo al momento della connessione da parte di un utente.
-   Verranno creati tre nuovi gruppi locali per controllare gli utenti a cui viene assegnato l'accesso a determinati ruoli:
    -   Windows Admin Center Administrators
    -   Windows Admin Center Hyper-V Administrators
    -   Windows Admin Center Readers

Per abilitare il supporto per il controllo degli accessi in base al ruolo in un singolo computer, segui questa procedura:

1.  Apri Windows Admin Center ed esegui la connessione al computer che vuoi configurare con il controllo degli accessi in base al ruolo usando un account con privilegi di amministratore locale sul computer di destinazione.
2.  Nello strumento **Panoramica** fai clic su **Impostazioni** > **Controllo degli accessi in base al ruolo**.
3.  Fai clic su **Applica** nella parte inferiore della pagina per abilitare il supporto per il controllo degli accessi in base al ruolo nel computer di destinazione. Con il processo di applicazione vengono copiati gli script di PowerShell e viene richiamata una configurazione (tramite Desired State Configuration di PowerShell) nel computer di destinazione. Il completamento del processo può richiedere al massimo 10 minuti e ha come risultato il riavvio di WinRM. Per effetto di questa operazione, gli utenti di Windows Admin Center, PowerShell e WMI verranno temporaneamente disconnessi.
4.  Aggiorna la pagina per verificare lo stato del controllo degli accessi in base al ruolo. Quando la configurazione è pronta per l'uso, lo stato diventerà **Applicato**.

Una volta applicata la configurazione, puoi assegnare gli utenti ai ruoli:

1.  Apri lo strumento **Utenti e gruppi locali** e passa alla scheda **Gruppi**.
2.  Seleziona il gruppo **Windows Admin Center Readers**.
3.  Nel riquadro *Dettagli* visualizzato nella parte inferiore della schermata fai clic su **Aggiungi utente** e immetti il nome di un utente o di un gruppo di sicurezza che deve avere accesso in sola lettura al server tramite Windows Admin Center. Gli utenti e i gruppi possono provenire dal computer locale o dal dominio di Active Directory.
4.  Ripeti i passaggi 2-3 per i gruppi **Windows Admin Center Hyper-V Administrators** e **Windows Admin Center Administrators**.

Puoi anche impostare questi gruppi in modo coerente nel dominio configurando un oggetto Criteri di gruppo con l'[impostazione Gruppi con restrizioni](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Applicare il controllo degli accessi in base al ruolo a più computer

In una distribuzione in un'azienda di grandi dimensioni, puoi usare gli strumenti di automazione esistenti per eseguire il push della funzionalità di controllo degli accessi in base al ruolo nei computer scaricando il pacchetto di configurazione dal gateway di Windows Admin Center.
Il pacchetto di configurazione è stato progettato per Desired State Configuration di PowerShell, ma puoi adattarlo per l'uso con la soluzione di automazione preferita.

#### <a name="download-the-role-based-access-control-configuration"></a>Scaricare la configurazione del controllo degli accessi in base al ruolo

Per scaricare il pacchetto di configurazione del controllo degli accessi in base al ruolo, devi avere accesso a Windows Admin Center e a un prompt di PowerShell.

Se esegui il gateway di Windows Admin Center in modalità servizio in Windows Server, usa il comando seguente per scaricare il pacchetto di configurazione.
Verifica di aggiornare l'indirizzo del gateway con quello corretto per il tuo ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se esegui il gateway di Windows Admin Center nel computer Windows 10, esegui invece il comando seguente:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Quando espandi l'archivio con estensione zip, viene visualizzata la struttura di cartelle seguente:

- InstallJeaFeatures.ps1
- JustEnoughAdministration (directory)
- Modules (directory)
    - Microsoft.SME.\* (directory)
    - WindowsAdminCenter.Jea (directory)

Per configurare il supporto per il controllo degli accessi in base al ruolo su un nodo, devi eseguire queste operazioni:

1.  Copia i moduli JustEnoughAdministration, Microsoft.SME.\* e WindowsAdminCenter.Jea nella directory dei moduli di PowerShell sul computer di destinazione. La directory si trova in genere nel percorso `C:\Program Files\WindowsPowerShell\Modules`.
2.  Aggiorna il file **InstallJeaFeature.ps1** in modo che corrisponda alla configurazione desiderata per l'endpoint del controllo degli accessi in base al ruolo.
3.  Esegui InstallJeaFeature.ps1 per compilare la risorsa DSC.
4.  Distribuisci la configurazione di DSC in tutti i computer per applicare la configurazione.

La sezione seguente illustra come eseguire questa operazione usando la comunicazione remota di PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Distribuire la configurazione in più computer

Per distribuire la configurazione scaricata in più computer, devi aggiornare lo script **InstallJeaFeatures.ps1** in modo da includere i gruppi di sicurezza appropriati per il tuo ambiente, copiare i file in ogni computer e richiamare gli script di configurazione.
Per ottenere questo risultato, puoi usare gli strumenti di automazione che preferisci, ma questo articolo illustrerà in particolare un approccio basato esclusivamente su PowerShell.

Per impostazione predefinita, lo script di configurazione creerà gruppi di sicurezza locali nel computer per controllare l'accesso a ogni ruolo.
Questo approccio è adatto ai computer aggiunti a gruppi di lavoro e a domini, ma se esegui la distribuzione in un ambiente di solo dominio, può essere opportuno associare direttamente un gruppo di sicurezza di dominio a ogni ruolo.
Per aggiornare la configurazione per l'uso dei gruppi di sicurezza di dominio, apri **InstallJeaFeatures.ps1** ed esegui le modifiche seguenti:

1.  Rimuovi le tre risorse **Group** dal file:
    1.  "Group MS-Readers-Group"
    2.  "Group MS-Hyper-V-Administrators-Group"
    3.  "Group MS-Administrators-Group"
2.  Rimuovi le tre risorse Group dalla proprietà **DependsOn** di JeaEndpoint
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  Modifica i nomi dei gruppi nella proprietà **RoleDefinitions** di JeaEndpoint specificando i gruppi di sicurezza desiderati. Se, ad esempio, hai un gruppo di sicurezza *CONTOSO\MyTrustedAdmins* a cui deve essere assegnato l'accesso al ruolo Windows Admin Center Administrators, modifica `'$env:COMPUTERNAME\Windows Admin Center Administrators'` in `'CONTOSO\MyTrustedAdmins'`. Le tre stringhe da aggiornare sono:
    1.  '$env:COMPUTERNAME\Windows Admin Center Administrators'
    2.  '$env:COMPUTERNAME\Windows Admin Center Hyper-V Administrators'
    3.  '$env:COMPUTERNAME\Windows Admin Center Readers'

> [!NOTE]
> Verifica di usare gruppi di sicurezza univoci per ogni ruolo. Se lo stesso gruppo di sicurezza viene assegnato a più ruoli, la configurazione avrà esito negativo.

Alla fine del file **InstallJeaFeatures.ps1** aggiungi le righe di PowerShell seguenti in fondo allo script:

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

Puoi infine copiare la cartella che contiene i moduli, la risorsa DSC e la configurazione in ogni nodo di destinazione ed eseguire lo script **InstallJeaFeature.ps1**.
Per effettuare questa operazione in modalità remota dalla workstation di amministrazione, puoi eseguire questi comandi:

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
