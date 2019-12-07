---
title: Distribuire un gateway dell'interfaccia di amministrazione di Windows in Azure
description: Come distribuire un gateway di Windows Admin Center in Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 1da4df284febbf18b5796322868451c45ab247ab
ms.sourcegitcommit: 7c7fc443ecd0a81bff6ed6dbeeaf4f24582ba339
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/07/2019
ms.locfileid: "74903933"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Distribuire l'interfaccia di amministrazione di Windows in Azure

## <a name="deploy-using-script"></a>Eseguire la distribuzione tramite script

È possibile scaricare [deploy-WACAzVM. ps1](https://aka.ms/deploy-wacazvm) che viene eseguito da [Azure cloud Shell](https://shell.azure.com) per configurare un gateway dell'interfaccia di amministrazione di Windows in Azure. Questo script può creare l'intero ambiente, incluso il gruppo di risorse.

[Passa alla procedura di distribuzione manuale](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Prerequisiti

* Configurare l'account in [Azure cloud Shell](https://shell.azure.com). Se è la prima volta che si usa Cloud Shell, verrà chiesto di associare o creare un account di archiviazione di Azure con Cloud Shell.
* In un Cloud Shell di **PowerShell** passare alla Home directory: ```PS Azure:\> cd ~```
* Per caricare il file di ```Deploy-WACAzVM.ps1```, trascinarlo e rilasciarlo dal computer locale in qualsiasi punto della finestra di Cloud Shell.

Se si specifica un certificato personalizzato:

* Caricare il certificato in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Per prima cosa, creare un insieme di credenziali delle chiavi nel portale di Azure e quindi caricare il certificato nell'insieme di credenziali delle chiavi. In alternativa, è possibile usare il portale di Azure per generare un certificato.

### <a name="script-parameters"></a>Parametri di script

* **ResourceGroupName** -[String] specifica il nome del gruppo di risorse in cui verrà creata la macchina virtuale.

* **Name** -[String] specifica il nome della macchina virtuale.

* **Credential** -[PSCredential] specifica le credenziali per la macchina virtuale.

* **PercorsoMSI** -[String] specifica il percorso locale del file MSI dell'interfaccia di amministrazione di Windows quando si distribuisce l'interfaccia di amministrazione di Windows in una macchina virtuale esistente. Il valore predefinito è la versione di https://aka.ms/WACDownload Se omesso.

* **VAULTNAME** -[String] specifica il nome dell'insieme di credenziali delle chiavi che contiene il certificato.

* **CertName** -[String] specifica il nome del certificato da usare per l'installazione MSI.

* **GenerateSslCert** -[switch] true se l'identità del servizio gestito deve generare un certificato SSL autofirmato.

* **NumeroPorta** -[int] specifica il numero di porta SSL per il servizio centro di amministrazione di Windows. Se omesso, il valore predefinito è 443.

* **Openports** -[int []] specifica le porte aperte per la macchina virtuale.

* **Location** -[String] specifica il percorso della macchina virtuale.

* **Size** -[String] specifica le dimensioni della macchina virtuale. Se omesso, il valore predefinito è "Standard_DS1_v2".

* **Image** -[String] specifica l'immagine della macchina virtuale. Se omesso, il valore predefinito è "Win2016Datacenter".

* **VirtualNetworkName** -[String] specifica il nome della rete virtuale per la macchina virtuale.

* **SubnetName** -[String] specifica il nome della subnet per la macchina virtuale.

* **SecurityGroupName** -[String] specifica il nome del gruppo di sicurezza per la macchina virtuale.

* **PublicIpAddressName** -[String] specifica il nome dell'indirizzo IP pubblico per la macchina virtuale.

* **InstallWACOnly** -[switch] è impostato su true se è necessario installare WAC in una macchina virtuale di Azure preesistente.

Sono disponibili due opzioni diverse per la distribuzione dell'identità del servizio gestito e il certificato usato per l'installazione MSI. L'identità del servizio gestito può essere scaricata da aka.ms/WACDownload o, se si esegue la distribuzione in una VM esistente, è possibile specificare il percorso di un file MSI in locale nella macchina virtuale. Il certificato è reperibile in Azure Key Vault o un certificato autofirmato verrà generato dall'identità del servizio gestito.

### <a name="script-examples"></a>Esempi di script

Definire innanzitutto le variabili comuni necessarie per i parametri dello script.

```PowerShell
$ResourceGroupName = "wac-rg1" 
$VirtualNetworkName = "wac-vnet"
$SecurityGroupName = "wac-nsg"
$SubnetName = "wac-subnet"
$VaultName = "wac-key-vault"
$CertName = "wac-cert"
$Location = "westus"
$PublicIpAddressName = "wac-public-ip"
$Size = "Standard_D4s_v3"
$Image = "Win2016Datacenter"
$Credential = Get-Credential
```

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Esempio 1: usare lo script per distribuire il gateway WAC in una nuova VM in una nuova rete virtuale e in un nuovo gruppo di risorse. Usare MSI da aka.ms/WACDownload e un certificato autofirmato dall'identità del servizio gestito.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Esempio 2: come #1, ma usando un certificato da Azure Key Vault.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm2"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    VaultName = $VaultName
    CertName = $CertName
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Esempio 3: uso di un'identità del servizio gestito locale in una macchina virtuale esistente per distribuire WAC.

```PowerShell
$MsiPath = "C:\Users\<username>\Downloads\WindowsAdminCenter<version>.msi"
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm3"
    Credential = $Credential
    MsiPath = $MsiPath
    InstallWACOnly = $true
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Requisiti per la macchina virtuale che esegue il gateway dell'interfaccia di amministrazione di Windows

È necessario aprire la porta 443 (HTTPS).
Usando le stesse variabili definite per lo script, è possibile usare il codice seguente in Azure Cloud Shell per aggiornare il gruppo di sicurezza di rete:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Requisiti per le macchine virtuali di Azure gestite

La porta 5985 (WinRM su HTTP) deve essere aperta e avere un listener attivo.
Per aggiornare i nodi gestiti, è possibile usare il codice riportato di seguito in Azure Cloud Shell. ```$ResourceGroupName``` e ```$Name``` usano le stesse variabili dello script di distribuzione, ma è necessario usare il ```$Credential``` specifico per la macchina virtuale che si sta gestendo.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Distribuisci manualmente in una macchina virtuale di Azure esistente

Prima di installare l'interfaccia di amministrazione di Windows nella macchina virtuale del gateway desiderata, installare un certificato SSL da usare per la comunicazione HTTPS oppure è possibile scegliere di usare un certificato autofirmato generato dall'interfaccia di amministrazione di Windows. Tuttavia, si riceverà un avviso quando si tenta di connettersi da un browser se si sceglie la seconda opzione. È possibile ignorare questo avviso in Edge facendo clic su **dettagli > accedere alla pagina Web** o, in Chrome, selezionando **Avanzate > passare a [pagina Web]** . Si consiglia di usare solo certificati autofirmati per gli ambienti di test.

> [!NOTE]
> Queste istruzioni sono per l'installazione di in Windows Server con esperienza desktop, non in un'installazione dei componenti di base del server. 

1. [Scaricare](https://aka.ms/windowsadmincenter) l'interfaccia di amministrazione di Windows nel computer locale.

2. Stabilire una connessione Desktop remoto alla VM, quindi copiare l'identità del servizio gestito dal computer locale e incollarla nella VM.

3. Fare doppio clic sul file MSI per avviare l'installazione e seguire le istruzioni della procedura guidata. Tenere presente quanto segue:

   - Per impostazione predefinita, il programma di installazione usa la porta consigliata 443 (HTTPS). Se si vuole selezionare una porta diversa, tenere presente che è necessario aprire anche la porta nel firewall. 

   - Se è già stato installato un certificato SSL nella macchina virtuale, assicurarsi di selezionare l'opzione e immettere l'identificazione personale.

4. Avviare il servizio Windows Admin Center (eseguire C:/Program Files/Windows Admin Center/SME. exe)

[Altre informazioni sulla distribuzione dell'interfaccia di amministrazione di Windows.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configurare la macchina virtuale del gateway per abilitare l'accesso alla porta HTTPS: 

1. Passare alla macchina virtuale nel portale di Azure e selezionare **rete**. 

2. Selezionare **Aggiungi regola porta in ingresso** e selezionare **https** in **servizio**. 

> [!NOTE]
> Se si sceglie una porta diversa da quella predefinita 443, scegliere **personalizzata** in servizio e immettere la porta scelta nel passaggio 3 in **intervalli di porte**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>Accesso a un gateway di Windows Admin Center installato in una macchina virtuale di Azure

A questo punto, dovrebbe essere possibile accedere all'interfaccia di amministrazione di Windows da un browser moderno (Edge o Chrome) nel computer locale passando al nome DNS della VM del gateway. 

> [!NOTE]
> Se è stata selezionata una porta diversa da 443, è possibile accedere al centro di amministrazione di Windows passando a https://\<nome DNS della VM\>:\<porta personalizzata\>

Quando si tenta di accedere all'interfaccia di amministrazione di Windows, il browser richiederà le credenziali per accedere alla macchina virtuale in cui è installato l'interfaccia di amministrazione di Windows. Qui sarà necessario immettere le credenziali presenti nel gruppo utenti locali o Administrators locale della macchina virtuale. 

Per aggiungere altre macchine virtuali in VNet, assicurarsi che WinRM sia in esecuzione nelle VM di destinazione eseguendo il comando seguente in PowerShell o il prompt dei comandi nella macchina virtuale di destinazione: `winrm quickconfig`

Se la macchina virtuale di Azure non è stata aggiunta a un dominio, la macchina virtuale si comporta come un server in un gruppo di lavoro, quindi è necessario assicurarsi di aver preso in considerazione l'uso dell'interfaccia di [amministrazione di Windows in un gruppo di](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)lavoro.