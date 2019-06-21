---
title: Distribuire un gateway Microsoft Windows Admin Center in Azure
description: Come distribuire un gateway Microsoft Windows Admin Center in Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a3fa1838096d910505faf9a2c5bd819b3a256fe2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280416"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Distribuire Windows Admin Center in Azure

## <a name="deploy-using-script"></a>Distribuzione tramite script

È possibile scaricare [Distribuisci WACAzVM.ps1](https://aka.ms/deploy-wacazvm) che viene eseguito dal [Azure Cloud Shell](https://shell.azure.com) per configurare un gateway Microsoft Windows Admin Center in Azure. Questo script può creare l'intero ambiente, inclusi il gruppo di risorse.

[Passare alla procedura di distribuzione manuale](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Prerequisiti

* Configurare l'account nel [Azure Cloud Shell](https://shell.azure.com). Se questa è la prima volta che usa Cloud Shell, verrà richiesto di associare o creare un account di archiviazione di Azure con Cloud Shell.
* In un **PowerShell** Cloud Shell, passare alla home directory: ```PS Azure:\> cd ~```
* Per caricare il ```Deploy-WACAzVM.ps1``` file, trascinare e rilasciarlo dal computer locale in un punto qualsiasi nella finestra della Cloud Shell.

Se si specifica un certificato personalizzato:

* Caricare il certificato [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). In primo luogo, creare un insieme di credenziali delle chiavi nel portale di Azure, quindi caricare il certificato nell'insieme di credenziali delle chiavi. In alternativa, è possibile usare il portale di Azure per generare un certificato per l'utente.

### <a name="script-parameters"></a>Parametri dello script

* **ResourceGroupName** -[stringa] Specifica il nome del gruppo di risorse in cui verrà creata la macchina virtuale.

* **Nome** -[stringa] Specifica il nome della macchina virtuale.

* **Credenziale** -[PSCredential] Specifica le credenziali per la macchina virtuale.

* **PercorsoMSI** -[stringa] Specifica il percorso locale del file MSI di Windows Admin Center quando si distribuisce Windows Admin Center in una VM esistente. Per impostazione predefinita la versione da http://aka.ms/WACDownload se viene omesso.

* **VaultName** -[stringa] Specifica il nome dell'insieme di credenziali chiave che contiene il certificato.

* **CertName** -[stringa] Specifica il nome del certificato da usare per l'installazione MSI.

* **GenerateSslCert** -[opzione] True se il file MSI deve generare un certificato ssl firmato self.

* **Numero di porta** -[int] Specifica il numero di porta ssl per il servizio Windows Admin Center. Il valore predefinito è 443 se viene omesso.

* **OpenPorts** -[int []] Specifica le porte aperte per la macchina virtuale.

* **Percorso** -[stringa] Specifica il percorso della macchina virtuale.

* **Dimensioni** -[stringa] Specifica le dimensioni della macchina virtuale. Il valore predefinito è "Standard_DS1_v2" Se viene omesso.

* **Immagine** -[stringa] Specifica l'immagine della macchina virtuale. Il valore predefinito è "Win2016Datacenter" Se viene omesso.

* **VirtualNetworkName** -[stringa] Specifica il nome della rete virtuale per la macchina virtuale.

* **SubnetName** -[stringa] Specifica il nome della subnet per la macchina virtuale.

* **SecurityGroupName** -[stringa] Specifica il nome del gruppo di sicurezza per la macchina virtuale.

* **PublicIpAddressName** -[stringa] Specifica il nome dell'indirizzo IP pubblico per la macchina virtuale.

* **InstallWACOnly** -[opzione] impostato su True se WAC deve essere installato in una VM di Azure preesistente.

Sono disponibili 2 opzioni diverse per l'identità del servizio gestito per la distribuzione e il certificato usato per l'installazione di MSI. Il file MSI che può essere scaricato da aka.ms/WACDownload o, se la distribuzione in una macchina virtuale esistente, è possibile assegnare il percorso di file di un file MSI in locale nella macchina virtuale. Il certificato è reperibile in due Azure Key Vault o verrà generato un certificato autofirmato per l'identità del servizio gestito.

### <a name="script-examples"></a>Esempi di script

In primo luogo, definire variabili comuni per i parametri dello script.

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Esempio 1: Usare lo script per distribuire gateway WAC in una nuova macchina virtuale in un nuovo gruppo di risorse e della rete virtuale. Usare MSI da aka.ms/WACDownload e un certificato autofirmato da MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Esempio 2: Uguale a #1, ma usa un certificato da Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Esempio 3: Utilizzo di un'identità del servizio gestito locale in una VM esistente per distribuire WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Requisiti per la macchina virtuale in esecuzione il gateway di Windows Admin Center

È necessario aprire la porta 443 (HTTPS).
Usa le stesse variabili definite per lo script, è possibile usare il codice seguente in Azure Cloud Shell per aggiornare il gruppo di sicurezza di rete:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Requisiti per gestito VM di Azure

Porta 5985 (WinRM su HTTP) deve essere aperte e avere un listener attivo.
È possibile usare il codice seguente in Azure Cloud Shell per aggiornare i nodi gestiti. ```$ResourceGroupName``` e ```$Name``` usare le stesse variabili dello script di distribuzione, ma è necessario usare il ```$Credential``` specifiche per la macchina virtuale si sta gestendo.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Distribuire manualmente in una macchina virtuale di Azure esistente

Prima di installare Windows Admin Center nel gateway desiderato della macchina virtuale, installare un certificato SSL da usare per la comunicazione HTTPS oppure è possibile scegliere di usare un certificato autofirmato generato da Windows Admin Center. Tuttavia, si otterrà un avviso quando si tenta di connettersi da un browser se si sceglie la seconda opzione. È possibile ignorare questo avviso in Microsoft Edge facendo **dettagli > passare alla pagina Web** o, in Chrome, selezionando **avanzate > passare alla pagina [Web]** . È consigliabile che usare solo i certificati autofirmati per ambienti di test.

> [!NOTE]
> Queste istruzioni sono per l'installazione in Windows Server con esperienza Desktop, non in un'installazione Server Core. 

1. [Download Windows Admin Center](https://aka.ms/windowsadmincenter) nel computer locale.

2. Stabilire una connessione desktop remoto alla macchina virtuale, quindi copiare il file MSI nel computer locale e incollare la macchina virtuale.

3. Fare doppio clic sul file MSI per avviare l'installazione e seguire le istruzioni della procedura guidata. Tenere presente quanto segue:

   - Per impostazione predefinita, il programma di installazione utilizza la porta 443 (HTTPS) consigliata. Se si desidera selezionare un'altra porta, tenere presente che è necessario aprire tale porta nel firewall anche. 

   - Se già installato un certificato SSL nella macchina virtuale, assicurarsi di selezionare l'opzione corrispondente e immettere l'identificazione personale.

4. Avviare il servizio Windows Admin Center (eseguito programma c: o file/Windows Admin Center/sme.exe)

[Altre informazioni sulla distribuzione di Windows Admin Center.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configurare il gateway VM per abilitare l'accesso alla porta HTTPS: 

1. Passare alla macchina virtuale nel portale di Azure e seleziona **Networking**. 

2. Selezionare **Aggiungi regola porta in ingresso** e selezionare **HTTPS** sotto **servizio**. 

> [!NOTE]
> Se è stata selezionata una porta diversa da quella predefinita 443, scegliere **Custom** nel servizio e immettere la porta scelto nel passaggio 3 nella sezione **gli intervalli di porta**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>L'accesso a un gateway di Windows Admin Center installato in una VM di Azure

A questo punto, è necessario essere in grado di accedere a Windows Admin Center da un browser moderno (Edge o Chrome) nel computer locale, passare al nome DNS del gateway di macchine Virtuali. 

> [!NOTE]
> Se si seleziona una porta diversa dalla 443, è possibile accedere Windows Admin Center, passare a https://\<nome DNS della macchina virtuale\>:\<porta personalizzata\>

Quando si tenta di accedere a Windows Admin Center, il browser chiederà le credenziali per accedere alla macchina virtuale in cui è installato Windows Admin Center. In questo caso è necessario immettere le credenziali che si trovano gli utenti locali o in gruppo administrators locale della macchina virtuale. 

Per aggiungere altre macchine virtuali nella rete virtuale, verificare che WinRM è in esecuzione in VM di destinazione eseguendo il comando seguente in PowerShell o prompt dei comandi nella macchina virtuale di destinazione: `winrm quickconfig`

Se è stato ancora aggiunto al dominio macchina virtuale di Azure, la macchina virtuale si comporta come un server nel gruppo di lavoro, pertanto è necessario assicurarsi che non è spiegare [utilizzo di Windows Admin Center in un gruppo di lavoro](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).