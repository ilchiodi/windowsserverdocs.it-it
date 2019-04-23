---
title: Delegare l'accesso Commandlet di Powershell di AD FS agli utenti senza privilegi di amministratore
description: In questo documento descirbes come delegare le autorizzazioni per utenti non amministratori di cmdlts di PowerShell per AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 265d22b045011e34192e56bdb970955b601cda56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883562"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Delegare l'accesso Commandlet di Powershell di AD FS agli utenti senza privilegi di amministratore 
Per impostazione predefinita, amministrazione di AD FS tramite PowerShell può essere eseguita solo dagli amministratori di AD FS. Per molte organizzazioni di grandi dimensioni, questo potrebbe non essere un modello operativo valido quando si lavora con altri utenti, ad esempio una personale dell'help desk.  

Con Just Enough Administration (JEA), i clienti possono delegare a questo punto i commandlet di specifici ai gruppi personale diverso.  
Un buon esempio di questo caso d'uso consenta personale dell'help desk per eseguire query AD FS lo stato del blocco dell'account e reimposta lo stato di blocco degli account in AD FS, una volta che un utente è stato esaminato. In questo caso, i cmdlet che dovranno essere delegato sono: 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Si userà questo esempio nella parte restante di questo documento. Tuttavia, possibile personalizzare questa opzione per consentire anche la delega impostare le proprietà di relying party e passare questa funzionalità per i proprietari delle applicazioni all'interno dell'organizzazione.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Creare gruppi necessari necessarie per concedere le autorizzazioni agli utenti 
1. Creare un [Account del servizio gestito del gruppo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). L'account gMSA viene usato per consentire all'utente JEA di accedere alle risorse di rete come altri computer o servizi web. Fornisce un'identità di dominio che può essere usata per l'autenticazione con le risorse in qualsiasi computer all'interno del dominio. All'account gMSA vengono concessi i diritti amministrativi necessari in un secondo momento nel programma di installazione. In questo esempio definiamo l'account **gMSAContoso**. 
2. Creare un Active Directory gruppo può essere popolato con gli utenti che devono essere concessi i diritti per i comandi delegati. In questo esempio, personale dell'help desk vengono concesse le autorizzazioni per leggere, aggiornare e reimpostare lo stato di blocco di ad FS. Si fa riferimento a questo gruppo in tutto l'esempio come **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Installare l'account gMSA nel Server ad FS: 
Creare un account del servizio che dispone di diritti amministrativi per i server ad FS. Ciò può essere eseguita nel controller di dominio o in remoto come long è installato il pacchetto RSAT di Active Directory.  L'account del servizio deve essere creato nella stessa foresta del server ad FS. Modificare i valori di esempio per la configurazione della farm. 

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

Installare l'account gMSA nel server ADFS.  Questa operazione deve essere eseguito in ogni nodo di ad FS nella farm. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Concedere i diritti di amministratore Account gMSA 
Se la farm Usa l'amministrazione delegata, concedere gMSA diritti di amministratore Account, aggiungerlo al gruppo esistente che ha delegato l'accesso come amministratore.  
 
Se la farm non usa l'amministrazione delegata, concedere gMSA diritti di amministratore account, rendendo il gruppo di amministratori locali in tutti i server ad FS. 
 
 
### <a name="create-the-jea-role-file"></a>Creare il File del ruolo JEA 
 
Creare il ruolo JEA in un file di blocco note. Le istruzioni per creare il ruolo viene fornito [capacità del ruolo JEA](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
I cmdlet di delega in questo esempio sono `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Delega dell'accesso del commandlet 'Reset-ADFSAccountLockout', 'Get-ADFSAccountActivity' e 'Set-ADFSAccountActivity' ruolo JEA di esempio:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Creare il File di configurazione sessione JEA 
Seguire le istruzioni per creare il [configurazione di sessione JEA](https://docs.microsoft.com/powershell/jea/session-configurations) file. Il file di configurazione determina chi può usare l'endpoint JEA e quali funzionalità di cui hanno accesso. 

Le funzionalità del ruolo vengono fatto riferimento dal nome flat (nome file senza estensione) del file delle funzionalità del ruolo. Se più funzionalità del ruolo sono disponibili nel sistema con lo stesso nome flat, PowerShell Usa l'ordine di ricerca implicito per selezionare il file di capacità del ruolo effettivo. Ma non concede l'accesso a tutti i file di funzionalità di ruolo con lo stesso nome. 

Per specificare un File delle funzionalità del ruolo con un percorso, usare il `RoleCapabilityFiles` argomento. Per una sottocartella, JEA è simile per i moduli di Powershell validi che contengono una `RoleCapabilities` sottocartella, in cui la `RoleCapabilityFiles` argomento deve essere modificato per essere `RoleCapabilities`. 

Esempio di file di configurazione di sessione: 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

Salvare il file di configurazione di sessione. 
 
È fortemente consigliabile [testare il file di configurazione di sessione](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) se è stato modificato il file pssc manualmente usando un editor di testo per assicurarsi che la sintassi sia corretto. Se un file di configurazione di sessione non supera il test, non si è registrato correttamente nel sistema.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Installare la configurazione di sessione JEA nel Server AD FS 

Installare la configurazione di sessione JEA nel server AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Istruzioni operative 
Una volta configurata, JEA registrazione e controllo può essere usato per stabilire se gli utenti corretti hanno accesso all'endpoint JEA. 

Per usare i comandi delegati: 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 
```
