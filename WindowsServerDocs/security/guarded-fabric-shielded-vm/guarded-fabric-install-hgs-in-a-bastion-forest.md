---
title: Installare servizio HGS in una foresta bastion esistente
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 147610d9dcb36dfedab3aca11ee1a64731715519
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842222"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Installare servizio HGS in una foresta bastion esistente 

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Aggiungere il server HGS al dominio esistente

In una foresta bastion esistente, HGS deve essere aggiunto al dominio radice. Utilizzare Server Manager oppure [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) per aggiungere il server HGS per il dominio radice.

## <a name="add-the-hgs-server-role"></a>Aggiungere il ruolo del server HGS

In una sessione di PowerShell con privilegi elevata, eseguire tutti i comandi in questo argomento.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Se il tuo Data Center dispone di una foresta bastione sicuro in cui si desidera aggiungere nodi HGS, seguire questa procedura.
È anche possibile usare questi passaggi per configurare cluster HGS 2 o più indipendente che appartengano allo stesso dominio.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Aggiungere il server HGS al dominio esistente

Utilizzare Server Manager oppure [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) aggiungere il server del servizio HGS per il dominio desiderato.

## <a name="prepare-active-directory-objects"></a>Preparare gli oggetti Active Directory

Creare un account del servizio gestito del gruppo e 2 gruppi di sicurezza.
Si possono anche pre-installazione degli oggetti cluster se l'account che si sta inizializzando HGS con non dispone dell'autorizzazione per creare oggetti computer nel dominio.

## <a name="group-managed-service-account"></a>Account del servizio gestito del gruppo

L'account del servizio gestito del gruppo (gMSA) è l'identità usata dal servizio HGS come recuperare e utilizzare i propri certificati. Uso [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) per creare un account gMSA.
Se si tratta di gMSA prima nel dominio, è necessario aggiungere una chiave radice servizio distribuzione chiavi.

Ogni nodo del servizio HGS dovrà essere la possibilità di accedere alla password gMSA.
Il modo più semplice per configurare questa impostazione consiste nel creare un gruppo di sicurezza che contiene tutti i nodi del servizio HGS e concedere a tale gruppo l'accesso per recuperare la password gMSA.

È necessario riavviare il server HGS dopo averla aggiunta a un gruppo di sicurezza per verificare che ottiene le appartenenze a gruppi di nuovi.

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

GMSA richiederà il diritto di generare gli eventi nel Registro di sicurezza in ogni server HGS.
Se si usa criteri di gruppo per configurare l'assegnazione diritti utente, assicurarsi che l'account gMSA sia concesse le [generare il privilegio di eventi controllo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) nei server HGS.

> [!NOTE]
> Gli account di servizio gestito del gruppo sono disponibili a partire con lo schema di Active Directory di Windows Server 2012.
> Per altre informazioni, vedere [requisiti dell'account del servizio gestito del gruppo](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Gruppi di sicurezza di JEA

Quando si configura il servizio HGS, un [Just Enough Administration (JEA)](https://aka.ms/JEAdocs) endpoint di PowerShell sia configurato per consentire agli amministratori di gestire il servizio HGS senza la necessità di privilegi di amministratore locale completo.
Non è necessario usare JEA per gestire il servizio HGS, ma ancora deve essere configurato durante l'esecuzione Initialize-HgsServer.
La configurazione dell'endpoint JEA costituita designazione di 2 gruppi di sicurezza che contengono il HGS amministratori e i revisori HGS.
Gli utenti che appartengono al gruppo amministratore possono aggiungere, modificare o rimuovere i criteri nel servizio HGS; i revisori possono visualizzare solo la configurazione corrente.

Creare 2 gruppi di sicurezza per questi gruppi JEA con strumenti di amministrazione di Active Directory oppure [New-ADGroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Oggetti cluster

Se l'account utilizzato per impostare HGS non dispone dell'autorizzazione per creare nuovi oggetti computer nel dominio, è necessario preparare gli oggetti cluster.
Questi passaggi sono illustrati nella [Prestage Cluster Computer Objects in Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Per configurare il primo nodo del servizio HGS, dovrai creare una virtuale Computer oggetto (VCO) e un oggetto di nome di un Cluster (CNO).
L'oggetto nome cluster rappresenta il nome del cluster e viene principalmente utilizzata internamente dal Clustering di Failover.
L'oggetto computer virtuale rappresenta il servizio HGS che si trova nella parte superiore del cluster e sarà il nome registrato con il server DNS.

> [!IMPORTANT]
> L'utente che eseguirà `Initialize-HgsServer` richiede **controllo completo** sugli oggetti nome cluster e oggetto computer virtuale in Active Directory.

Per rapidamente pre-installare l'oggetto nome cluster e un oggetto computer virtuale, che un amministratore di Active Directory eseguano i comandi PowerShell seguenti:

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

## <a name="security-baseline-exceptions"></a>Eccezioni della linea di base di sicurezza

Se si distribuiscono HGS in un ambiente altamente bloccato, alcune impostazioni dei criteri di gruppo potrebbero impedire servizio HGS di funzionare normalmente.
Controllare gli oggetti Criteri di gruppo per le impostazioni seguenti e seguire le linee guida se si è interessati:

### <a name="network-logon"></a>Accesso alla rete

**Percorso dei criteri:** Computer Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti assegnazioni

**Nome del criterio:** Nega accesso al computer dalla rete

**Valore obbligatorio:** Verificare che il valore non blocca gli accessi alla rete per tutti gli account locali. È possibile bloccare in modo sicuro gli account di amministratore locale, tuttavia.

**Motivo:** Clustering di failover si basa su un account locale senza privilegi di amministratore chiamato CLIUSR per gestire i nodi del cluster. Blocco accesso alla rete per questo utente impedisce il cluster funzioni correttamente.

### <a name="kerberos-encryption"></a>Crittografia Kerberos

**Percorso dei criteri:** Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri locali\Opzioni di sicurezza

**Nome del criterio:** Sicurezza di rete: Configurare i tipi di crittografia consentiti per Kerberos

**Azione**: Se questo criterio è configurato, è necessario aggiornare l'account gMSA con [Set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) usare solo i tipi di crittografia supportati in questo criterio. Ad esempio, se i criteri consentono solo AES128\_HMAC\_SHA1 e AES256\_HMAC\_SHA1, è consigliabile eseguire `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Passaggi successivi

- Per i passaggi successivi configurare l'attestazione basata su TPM, vedere [inizializzare il cluster HGS uso della modalità TPM in una foresta bastion esistente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Per i passaggi successivi configurare l'attestazione chiave host, vedere [inizializzare il cluster HGS usando la modalità con chiave in una foresta bastion esistente](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Per i prossimi passaggi per configurare l'attestazione basata su Admin (deprecata in Windows Server 2019), vedere [inizializzare il cluster HGS utilizza la modalità Active Directory in una foresta bastion esistente](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

