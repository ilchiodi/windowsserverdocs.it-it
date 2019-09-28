---
title: Delega AD FS cmdlet di PowerShell per l'accesso agli utenti non amministratori
description: Questo documento descirbes come delegare le autorizzazioni a non amministratori per AD FS cmdlts di PowerShell.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 334bb96c77b0bc1e76a54ed1e0871f53753ded87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357746"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Delega AD FS cmdlet di PowerShell per l'accesso agli utenti non amministratori 
Per impostazione predefinita, AD FS amministrazione tramite PowerShell può essere eseguita solo dagli amministratori di AD FS. Per molte organizzazioni di grandi dimensioni, questo potrebbe non essere un modello operativo funzionante quando si gestiscono altri utenti, ad esempio un personale help desk.  

Con JEA (just enough Administration), i clienti possono ora delegare un cmdlet specifico a gruppi di personale diversi.  
Un valido esempio di questo caso d'uso è consentire a help desk personale di eseguire query AD FS stato di blocco degli account e di reimpostare lo stato di blocco dell'account in AD FS dopo che un utente è stato controllato. In questo caso, i cmdlet che devono essere delegati sono: 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Questo esempio viene usato nel resto di questo documento. Tuttavia, è possibile personalizzarlo in modo da consentire anche la delega per impostare le proprietà delle relying party e passarle ai proprietari dell'applicazione all'interno dell'organizzazione.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Creare i gruppi necessari per concedere le autorizzazioni agli utenti 
1. Creare un [account del servizio gestito del gruppo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). L'account gMSA viene usato per consentire all'utente di JEA di accedere alle risorse di rete come altri computer o servizi Web. Fornisce un'identità di dominio che può essere usata per eseguire l'autenticazione in base alle risorse in qualsiasi computer all'interno del dominio. All'account gMSA vengono concessi i diritti amministrativi necessari in un secondo momento nel programma di installazione. Per questo esempio, viene chiamato l'account **gMSAContoso**. 
2. Creare un gruppo di Active Directory può essere popolato con gli utenti a cui devono essere concessi i diritti per i comandi delegati. In questo esempio, al personale help desk vengono concesse le autorizzazioni per la lettura, l'aggiornamento e la reimpostazione dello stato di blocco di ADFS. Si fa riferimento a questo gruppo nell'esempio di **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Installare l'account gMSA nel server ADFS: 
Creare un account del servizio con diritti amministrativi per i server ADFS. Questa operazione può essere eseguita sul controller di dominio o in remoto purché sia installato il pacchetto AD remota.  L'account del servizio deve essere creato nella stessa foresta del server ADFS. Modificare i valori di esempio nella configurazione della farm. 

```powershell
 # This command should only be run if this is the first time gMSA accounts are enabled in the forest 
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))  
 
# Run this on every node that you want to have JEA configured on  
$adfsServer = Get-ADComputer server01.contoso.com  
 
# Run targeted at domain controller  
$serviceaccount = New-ADServiceAccount gMSAcontoso -DNSHostName <FQDN of the domain containing the KDS key> - PrincipalsAllowedToRetrieveManagedPassword $adfsServer –passthru 
 
# Run this on every node 
Add-ADComputerServiceAccount -Identity server01.contoso.com -ServiceAccount $ServiceAccount 
```

Installare l'account gMSA nel server ADFS.  Questa operazione deve essere eseguita in ogni nodo ADFS della farm. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Concedere i diritti di amministratore dell'account gMSA 
Se la farm usa l'amministrazione delegata, concedere i diritti di amministratore dell'account gMSA aggiungendola al gruppo esistente con accesso amministratore delegato.  
 
Se la farm non utilizza l'amministrazione delegata, concedere i diritti di amministratore dell'account gMSA rendendola il gruppo di amministrazione locale su tutti i server ADFS. 
 
 
### <a name="create-the-jea-role-file"></a>Creare il file del ruolo JEA 
 
Nel server di AD FS creare il ruolo JEA in un file del blocco note. Le istruzioni per la creazione del ruolo sono disponibili per le [funzionalità del ruolo Jea](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
Il cmdlet delegato in questo esempio è `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Ruolo JEA di esempio che delega l'accesso di ' Reset-ADFSAccountLockout ',' Get-ADFSAccountActivity ' è set-ADFSAccountActivity ' cmdlet:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Creare il file di configurazione della sessione JEA 
Seguire le istruzioni per creare il file di [configurazione della sessione Jea](https://docs.microsoft.com/powershell/jea/session-configurations) . Il file di configurazione determina chi può usare l'endpoint JEA e a quali funzionalità hanno accesso. 

Le funzionalità del ruolo fanno riferimento al nome Flat (FileName senza estensione) del file di capacità del ruolo. Se nel sistema sono disponibili più funzionalità del ruolo con lo stesso nome Flat, PowerShell usa l'ordine di ricerca implicito per selezionare il file di capacità del ruolo effettivo. Non fornisce l'accesso a tutti i file delle funzionalità del ruolo con lo stesso nome. 

Per specificare un file di capacità del ruolo con un percorso, `RoleCapabilityFiles` utilizzare l'argomento. Per una sottocartella, Jea Cerca i moduli di PowerShell validi che contengono `RoleCapabilities` una sottocartella, in `RoleCapabilityFiles` cui l'argomento deve essere modificato `RoleCapabilities`in modo da essere. 

Esempio di file di configurazione della sessione: 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

Salvare il file di configurazione della sessione. 
 
Si consiglia vivamente di [testare il file di configurazione della sessione](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) se il file PSSC è stato modificato manualmente usando un editor di testo per assicurarsi che la sintassi sia corretta. Se il test non viene superato, il file di configurazione della sessione non viene registrato correttamente nel sistema.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Installare la configurazione di sessione JEA nel server AD FS 

Installare la configurazione di sessione JEA nel server AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Istruzioni operative 
Una volta configurato, è possibile usare la registrazione e il controllo JEA per determinare se gli utenti corretti hanno accesso all'endpoint JEA. 

Per usare i comandi delegati: 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 


```
