---
title: Distribuire un gateway Windows Admin Center in Azure
description: Come distribuire un gateway Windows Admin Center in Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: af766c2e0502d9fe633cae42d999db5cbffc32c8
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296996"
---
# Distribuire Windows Admin Center in Azure

## Distribuire tramite script

Puoi scaricare [Distribuisci WACAzVM.ps1](https://aka.ms/deploy-wacazvm) che verrà eseguita dalla [Shell Cloud di Azure](https://shell.azure.com) per configurare un gateway Windows Admin Center in Azure. Questo script di creare l'intero ambiente, inclusi il gruppo di risorse.

[Passare alla procedura di distribuzione manuale](#deploy-manually-on-an-existing-azure-virtual-machine)

### Prerequisiti

* Impostare l'account nella [Shell di Cloud di Azure](https://shell.azure.com). Se questa è la prima volta tramite Cloud Shell, verrà chiesto di associare o creare un account di archiviazione di Azure con Shell Cloud.
* In una Shell Cloud **PowerShell** , passa alla directory principale: ```PS Azure:\> cd ~```
* Per caricare il ```Deploy-WACAzVM.ps1``` file, trascinare e copiarlo dal computer locale in un punto qualsiasi nella finestra di Shell Cloud.

Se si specifica il proprio certificato:

* Caricare il certificato [all'Insieme di credenziali di Azure chiave](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis). Prima di tutto, crea un insieme di credenziali di chiave nel portale di Azure, quindi caricare il certificato nell'archivio di chiave. In alternativa, è possibile utilizzare il portale di Azure per generare un certificato per te.

### Parametri di script

* **ResourceGroupName** - [stringa] Specifica il nome del gruppo di risorse in cui verrà creata la macchina virtuale.

* **Nome** - [stringa] Specifica il nome della macchina virtuale.

* **Credenziali** - [PSCredential] Specifica le credenziali per la macchina virtuale.

* **Percorso** - [stringa] Specifica il percorso locale di Windows Admin Center MSI durante la distribuzione di Windows Admin Center in una macchina virtuale esistente. Per impostazione predefinita per la versione da http://aka.ms/WACDownload Se omesso.

* **VaultName** - [stringa] Specifica il nome dell'archivio di chiave contenente il certificato.

* **CertName** - [stringa] Specifica il nome del certificato da usare per l'installazione MSI.

* **GenerateSslCert** - [commutatore] True se il file MSI dovrebbe generare un certificato autofirmato ssl firmato.

* **NumeroPorta** - [int] Specifica il numero di porta ssl per il servizio Windows Admin Center. Impostazione predefinita è 443 se omesso.

* **OpenPorts** - [int []] Specifica le porte aperte per la macchina virtuale.

* **Posizione** - [stringa] Specifica la posizione della macchina virtuale.

* **Dimensioni** - [stringa] Specifica la dimensione della macchina virtuale. Impostazione predefinita è "Standard_DS1_v2" Se omesso.

* **Immagine** - [String] Specifica l'immagine della macchina virtuale. Impostazione predefinita è "Win2016Datacenter" Se omesso.

* **VirtualNetworkName** - [stringa] Specifica il nome della rete virtuale per la macchina virtuale.

* **SubnetName** - [stringa] Specifica il nome della subnet per la macchina virtuale.

* **SecurityGroupName** - [stringa] Specifica il nome del gruppo di sicurezza per la macchina virtuale.

* **PublicIpAddressName** - [stringa] Specifica il nome dell'indirizzo IP pubblico per la macchina virtuale.

* **InstallWACOnly** - [commutatore] è impostato su True se WAC deve essere installato in una macchina virtuale di Azure esistenti.

Esistono 2 diverse opzioni per il file MSI per distribuire e il certificato utilizzato per l'installazione MSI. Il file MSI sia possono essere scaricato da aka.ms/WACDownload o, se la distribuzione in una macchina virtuale esistente, è possibile assegnare il percorso di file di un file MSI in locale nella macchina virtuale. Il certificato è reperibile in entrambi insieme di credenziali di Azure chiave o un certificato autofirmato verrà generato dal file MSI.

### Esempi di script

Prima di tutto, definire le variabili comuni necessari per i parametri dello script.

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

#### Esempio 1: Usare lo script per distribuire il gateway WAC in una nuova macchina virtuale in un nuovo gruppo di risorse e della rete virtuale. Usa il file MSI da aka.ms/WACDownload e un certificato autofirmato dal file MSI.

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

#### Esempio 2: Identico #1, ma con un certificato di chiave insieme di credenziali di Azure.

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

#### Esempio 3: Usando un file MSI locale in una macchina virtuale esistente per distribuire WAC.

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

### Requisiti per la macchina virtuale che esegue il gateway Windows Admin Center

La porta 443 (HTTPS) deve essere aperta.
Usando le stesse variabili definite per lo script, è possibile utilizzare il codice seguente nella Shell di Cloud di Azure per aggiornare il gruppo di sicurezza di rete:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### Requisiti per gestito VM di Azure

Porta 5985 (WinRM su HTTP) deve essere aperta e avere un listener attivo.
È possibile utilizzare il codice seguente nella Shell di Cloud di Azure per aggiornare i nodi gestiti. ```$ResourceGroupName``` e ```$Name``` usare le stesse variabili come lo script di distribuzione, ma dovrai usare il ```$Credential``` specifico per la macchina virtuale che si sta gestendo.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## Distribuire manualmente in una macchina virtuale di Azure esistente

Prima di installare Windows Admin Center sul gateway desiderato della macchina virtuale, installare un certificato SSL da usare per le comunicazioni HTTPS, oppure puoi scegliere di usare un certificato autofirmato generato da Windows Admin Center. Tuttavia, verrà visualizzato un avviso quando si tenta di connettersi da un browser, se scegli l'opzione di quest'ultima. È possibile ignorare questo avviso in Edge facendo **gt _ dettagli continua per la pagina Web** o, in Chrome, selezionando **Avanzate gt _ continua a [pagina Web]**. Ti consigliamo di che usare solo i certificati autofirmati per gli ambienti di test.

> [!NOTE]
> Queste istruzioni sono per l'installazione su Windows Server con esperienza Desktop, non in un'installazione Server Core. 

1. [Scarica Windows Admin Center](https://aka.ms/windowsadmincenter) nel computer locale.

2. Stabilire una connessione desktop remoto alla macchina virtuale, quindi copia il file MSI dal computer locale e incollare la macchina virtuale.

3. Fai doppio clic sul file MSI per avviare l'installazione e segui le istruzioni della procedura guidata. Tenere presente quanto segue:

   - Per impostazione predefinita, il programma di installazione utilizza la porta 443 (HTTPS) consigliata. Se vuoi selezionare una porta diversa, tieni presente che è necessario aprire la porta del firewall anche. 

   - Se hai già installato un certificato SSL nella macchina virtuale, assicurarsi si seleziona l'opzione corrispondente e immetti l'identificazione personale.

4. Avvia il servizio Windows Admin Center (eseguito programma/c: file/Windows Admin Center/sme.exe)

[Altre informazioni sulla distribuzione di Windows Admin Center.](../deploy/install.md)

### Configurare il gateway macchina virtuale per abilitare l'accesso della porta HTTPS: 

1. Passa alla macchina virtuale nel portale di Azure e seleziona **di rete**. 

2. Seleziona **Aggiungi regola di porta in ingresso** e **HTTPS** nel **servizio**. 

> [!NOTE]
> Se hai scelto una porta diversa da quella predefinita 443, scegliere **personalizzata** nel servizio e Immetti la porta che hai scelto nel passaggio 3 sotto **intervalli di porte**. 

### Accesso a un gateway Windows Admin Center installato in una macchina virtuale di Azure

A questo punto, dovresti essere in grado di accedere a Windows Admin Center da un browser moderno (Edge o Chrome) nel computer locale passando al nome DNS del gateway della macchina virtuale. 

> [!NOTE]
> Se hai selezionato una porta diversa da 443, puoi accedere a Windows Admin Center per passare a https://\<DNS nome del tuo VM\>:\<custom port\>

Quando si tenta di accedere a Windows Admin Center, il browser ti chiederà le credenziali per accedere alla macchina virtuale in cui è installato Windows Admin Center. Qui, dovrai immettere le credenziali che sono nel gruppo administrators locale della macchina virtuale o gli utenti locali. 

Per poter aggiungere il con altre macchine virtuali, assicurarsi che WinRM è in esecuzione su macchine virtuali di destinazione eseguendo il comando seguente in PowerShell o il prompt dei comandi nella destinazione della macchina virtuale: `winrm quickconfig`

Se si non ancora aggiunti al dominio macchina virtuale di Azure, la macchina virtuale si comporta come un server nel gruppo di lavoro, pertanto è necessario prendere in che considerazione per [l'uso di Windows Admin Center in un gruppo di lavoro](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).