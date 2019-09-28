---
title: Installare HGS in una foresta Bastion esistente
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5c503331893dbea65c99d79eb7444893d5f3b657
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403600"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Installare HGS in una foresta Bastion esistente 

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Aggiungere il server HGS al dominio esistente

In una foresta Bastion esistente è necessario aggiungere HGS al dominio radice. Usare Server Manager o [Add-computer per aggiungere](https://go.microsoft.com/fwlink/?LinkId=821564) il server HGS al dominio radice.

## <a name="add-the-hgs-server-role"></a>Aggiungere il ruolo del server HGS

Eseguire tutti i comandi in questo argomento in una sessione di PowerShell con privilegi elevati.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Se il Data Center dispone di una foresta Bastion protetta in cui si desidera aggiungere i nodi HGS, attenersi alla seguente procedura.
È anche possibile usare questi passaggi per configurare 2 o più cluster HGS indipendenti aggiunti allo stesso dominio.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Aggiungere il server HGS al dominio esistente

Usare Server Manager o [Add-computer per aggiungere](https://go.microsoft.com/fwlink/?LinkId=821564) i server HGS al dominio desiderato.

## <a name="prepare-active-directory-objects"></a>Preparare oggetti Active Directory

Creare un account del servizio gestito del gruppo e 2 gruppi di sicurezza.
È anche possibile pre-installare gli oggetti cluster se l'account con cui si sta inizializzando HGS non dispone dell'autorizzazione per la creazione di oggetti computer nel dominio.

## <a name="group-managed-service-account"></a>Account del servizio gestito del gruppo

L'account del servizio gestito del gruppo (gMSA) è l'identità usata da HGS per recuperare e usare i relativi certificati. Usare [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) per creare un gMSA.
Se questo è il primo gMSA nel dominio, sarà necessario aggiungere una chiave radice del servizio di distribuzione delle chiavi.

Ogni nodo HGS dovrà essere autorizzato ad accedere alla password di gMSA.
Il modo più semplice per configurare questa operazione consiste nel creare un gruppo di sicurezza che contenga tutti i nodi di HGS e concedere a tale gruppo di sicurezza l'accesso per recuperare la password di gMSA.

È necessario riavviare il server HGS dopo averlo aggiunto a un gruppo di sicurezza per assicurarsi che ottenga la nuova appartenenza al gruppo.

```powershell
# Check if the KDS root key has been set up
if (-not (Get-KdsRootKey)) {
    # Adds a KDS root key effective immediately (ignores normal 10 hour waiting period)
    Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
}

# Create a security group for HGS nodes
$hgsNodes = New-ADGroup -Name 'HgsServers' -GroupScope DomainLocal -PassThru

# Add your HGS nodes to this group
# If your HGS server object is under an organizational unit, provide the full distinguished name instead of "HGS01"
Add-ADGroupMember -Identity $hgsNodes -Members "HGS01"

# Create the gMSA
New-ADServiceAccount -Name 'HGSgMSA' -DnsHostName 'HGSgMSA.yourdomain.com' -PrincipalsAllowedToRetrieveManagedPassword $hgsNodes
```

GMSA richiede il diritto di generare eventi nel registro di sicurezza in ogni server HGS.
Se si usa Criteri di gruppo per configurare l'assegnazione dei diritti utente, assicurarsi che all'account gMSA venga concesso il [privilegio genera eventi di controllo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) nei server HGS.

> [!NOTE]
> Gli account del servizio gestito del gruppo sono disponibili a partire dallo schema di Active Directory di Windows Server 2012.
> Per ulteriori informazioni, vedere [requisiti dell'account del servizio gestito del gruppo](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Gruppi di sicurezza JEA

Quando si configura HGS, un endpoint di PowerShell [Jea (just enough Administration)](https://aka.ms/JEAdocs) viene configurato per consentire agli amministratori di gestire HGS senza che siano necessari privilegi di amministratore locale completi.
Non è necessario usare JEA per gestire HGS, ma è comunque necessario configurarlo quando si esegue Initialize-HgsServer.
La configurazione dell'endpoint JEA è costituita da due gruppi di sicurezza che contengono gli amministratori di HGS e i revisori HGS.
Gli utenti che appartengono al gruppo di amministratori possono aggiungere, modificare o rimuovere criteri in HGS; i revisori possono solo visualizzare la configurazione corrente.

Creare due gruppi di sicurezza per questi gruppi di JEA usando Active Directory strumenti [di amministrazione o New-ADGroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Oggetti cluster

Se l'account utilizzato per configurare HGS non dispone dell'autorizzazione per la creazione di nuovi oggetti computer nel dominio, sarà necessario eseguire la pre-installazione degli oggetti cluster.
Questi passaggi sono illustrati in pre-installare [oggetti computer del cluster in Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Per configurare il primo nodo HGS, è necessario creare un oggetto nome cluster (oggetto nome cluster) e un oggetto computer virtuale (VCO).
OGGETTO nome cluster rappresenta il nome del cluster e viene utilizzato principalmente internamente dal clustering di failover.
Il VCO rappresenta il servizio HGS che risiede in cima al cluster e sarà il nome registrato con il server DNS.

> [!IMPORTANT]
> L'utente che eseguirà `Initialize-HgsServer` richiede il **controllo completo** sugli oggetti oggetto nome cluster e VCO nel Active Directory.

Per eseguire rapidamente la pre-installazione di oggetto nome cluster e VCO, chiedere a un amministratore Active Directory di eseguire i comandi di PowerShell seguenti:

```powershell
# Create the CNO
$cno = New-ADComputer -Name 'HgsCluster' -Description 'HGS CNO' -Enabled $false -Passthru

# Create the VCO
$vco = New-ADComputer -Name 'HgsService' -Description 'HGS VCO' -Passthru

# Give the CNO full control over the VCO
$vcoPath = Join-Path "AD:\" $vco.DistinguishedName
$acl = Get-Acl $vcoPath
$ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $cno.SID, "GenericAll", "Allow"
$acl.AddAccessRule($ace)
Set-Acl -Path $vcoPath -AclObject $acl

# Allow time for your new CNO and VCO to replicate to your other Domain Controllers before continuing
```

## <a name="security-baseline-exceptions"></a>Eccezioni di base per la sicurezza

Se si distribuisce HGS in un ambiente con blocco elevato, alcune impostazioni Criteri di gruppo possono impedire il funzionamento di HGS in modo normale.
Controllare gli oggetti Criteri di gruppo per le seguenti impostazioni e seguire le istruzioni in caso di impatto:

### <a name="network-logon"></a>Accesso alla rete

**Percorso criterio:** Configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri locali\Assegnazione diritti assegnati

**Nome criterio:** Nega accesso al computer dalla rete

**Valore obbligatorio:** Verificare che il valore non blocchi gli accessi di rete per tutti gli account locali. Tuttavia, è possibile bloccare in modo sicuro gli account di amministratore locale.

**Motivo** Il clustering di failover si basa su un account locale non amministratore denominato CLIUSR per gestire i nodi del cluster. Se si blocca l'accesso alla rete per questo utente, il cluster non funzionerà correttamente.

### <a name="kerberos-encryption"></a>Crittografia Kerberos

**Percorso criterio:** Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri locali\Opzioni di sicurezza

**Nome criterio:** Sicurezza di rete: Configurare i tipi di crittografia consentiti per Kerberos

**Azione**: Se questo criterio è configurato, è necessario aggiornare l'account gMSA con [Set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) per usare solo i tipi di crittografia supportati in questo criterio. Ad esempio, se il criterio consente solo AES128 @ no__t-0HMAC @ no__t-1SHA1 e AES256 @ no__t-2HMAC @ no__t-3SHA1, è necessario eseguire `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Passaggi successivi

- Per i passaggi successivi per la configurazione dell'attestazione basata su TPM, vedere [inizializzare il cluster HGS con la modalità TPM in una foresta Bastion esistente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Per i passaggi successivi per la configurazione dell'attestazione della chiave host, vedere [inizializzare il cluster HGS usando la modalità chiave in una foresta Bastion esistente](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Per i passaggi successivi per la configurazione dell'attestazione basata sull'amministratore (deprecata in Windows Server 2019), vedere [inizializzare il cluster HGS usando la modalità ad in una foresta Bastion esistente](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

