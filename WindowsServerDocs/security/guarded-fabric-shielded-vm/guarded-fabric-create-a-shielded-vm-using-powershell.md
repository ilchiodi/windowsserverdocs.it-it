---
title: Creare una macchina virtuale schermata usando PowerShell
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 09e09fa30a38ef5f6046f623e24be0bc7b6ce87e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856754"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Creare una macchina virtuale schermata usando PowerShell

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

In produzione, in genere si usa un gestore di infrastruttura, ad esempio VMM, per distribuire macchine virtuali schermate. Tuttavia, i passaggi illustrati di seguito consentono di distribuire e convalidare l'intero scenario senza un gestore di infrastruttura.

In breve, si creerà un disco modello, un file di dati di schermatura, un file di risposte per l'installazione automatica e altri elementi di sicurezza in qualsiasi computer, quindi copiare i file in un host sorvegliato ed effettuare il provisioning della macchina virtuale schermata.

## <a name="create-a-signed-template-disk"></a>Creare un disco modello firmato

Per creare una nuova macchina virtuale schermata, è necessario innanzitutto un disco modello di macchina virtuale schermato precrittografato con il relativo volume del sistema operativo (o partizioni di avvio e radice in Linux) firmato.
Per ulteriori informazioni su come creare un disco modello, seguire i collegamenti seguenti.

- [Preparare un disco modello di Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Preparare un disco modello Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

Per creare il file di dati di schermatura, sarà necessaria anche una copia del catalogo delle firme del volume del disco.
Per salvare il file, eseguire il comando seguente nel computer in cui è stato creato il disco modello:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Scarica i metadati di sorveglianza

Per ogni tessuto di virtualizzazione in cui si vuole eseguire la macchina virtuale schermata, è necessario ottenere i metadati del tutore per i cluster HGS di Fabric.
Il provider di hosting dovrebbe essere in grado di fornire queste informazioni.

Se ci si trova in un ambiente aziendale e si è in grado di comunicare con il server HGS, i metadati del Guardian sono disponibili all'indirizzo *http://\<HGSCLUSTERNAME\>/KeyProtection/Service/Metadata/2014-07/Metadata.XML*

## <a name="create-shielding-data-pdk-file"></a>Crea file di dati di schermatura (PDK)

I dati di schermatura vengono creati e sono di proprietà dei proprietari delle macchine virtuali tenant e contengono i segreti necessari per creare VM schermate che devono essere protette dall'amministratore dell'infrastruttura, ad esempio la password di amministratore della macchina virtuale schermata.
I dati di schermatura vengono crittografati in modo che solo i server HGS e il tenant possano decrittografarlo.
Una volta creati dal proprietario del tenant o della macchina virtuale, il file PDK risultante deve essere copiato nell'infrastruttura sorvegliata.
Per altre informazioni, vedere [che cos'è la schermatura dei dati e perché è necessaria?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Inoltre, sarà necessario un file di risposte per l'installazione automatica (Unattend. XML per Windows, varia per Linux). Per informazioni sugli elementi da includere nel file di risposte, vedere [creare un file di risposte](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) .

Eseguire i cmdlet seguenti in un computer con il Strumenti di amministrazione remota del server per le macchine virtuali schermate installate.
Se si sta creando una PDK per una macchina virtuale Linux, è necessario eseguire questa operazione in un server che esegue Windows Server, versione 1709 o successiva.

 
```powershell
# Create owner certificate, don't lose this!
# The certificate is stored at Cert:\LocalMachine\Shielded VM Local Certificates
$Owner = New-HgsGuardian –Name 'Owner' –GenerateCertificates
 
# Import the HGS guardian for each fabric you want to run your shielded VM
$Guardian = Import-HgsGuardian -Path C:\HGSGuardian.xml -Name 'TestFabric'
 
# Create the PDK file
# The "Policy" parameter describes whether the admin can see the VM's console or not
# Use "EncryptionSupported" if you are testing out shielded VMs and want to debug any issues during the specialization process
New-ShieldingDataFile -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Owner $Owner –Guardian $guardian –VolumeIDQualifier (New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\MyTemplateDiskCatalog.vsc' -VersionRule Equals) -WindowsUnattendFile 'C:\unattend.xml' -Policy Shielded
```
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Eseguire il provisioning di macchine virtuali schermate in un host sorvegliato
Copiare il file del disco del modello (Serveros. vhdx) e il file PDK (contoso. PDK) nell'host sorvegliato per prepararsi alla distribuzione.

Nell'host sorvegliato installare il modulo di PowerShell per gli strumenti di infrastruttura sorvegliata, che contiene il cmdlet New-ShieldedVM per semplificare il processo di provisioning. Se l'host sorvegliato ha accesso a Internet, eseguire il comando seguente:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

È anche possibile scaricare il modulo in un altro computer che dispone di accesso a Internet e copiare il modulo risultante per `C:\Program Files\WindowsPowerShell\Modules` nell'host sorvegliato.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Una volta installato il modulo, è possibile effettuare il provisioning della macchina virtuale schermata.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Se il file di risposte dei dati di schermatura include valori di specializzazione, è possibile fornire i valori di sostituzione a New-ShieldedVM. In questo esempio, il file di risposte è configurato con i valori segnaposto per un indirizzo IPv4 statico.

```powershell
$specializationValues = @{
    "@IP4Addr-1@" = "192.168.1.10/24"
    "@MacAddr-1@" = "Ethernet"
    "@Prefix-1-1@" = "24"
    "@NextHop-1-1@" = "192.168.1.254"
}
New-ShieldedVM -Name 'MyStaticIPVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -SpecializationValues $specializationValues -Wait

```

Se il disco del modello contiene un sistema operativo basato su Linux, includere il flag di `-Linux` durante l'esecuzione del comando:

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Per ulteriori informazioni sulle altre opzioni che è possibile passare al cmdlet, consultare il contenuto della Guida utilizzando `Get-Help New-ShieldedVM -Full`.

Al termine del provisioning della macchina virtuale, entrerà nella fase di specializzazione specifica del sistema operativo, dopo la quale sarà pronta per l'uso.
Assicurarsi di connettere la macchina virtuale a una rete valida per potersi connettere al momento dell'esecuzione (usando RDP, PowerShell, SSH o lo strumento di gestione preferito).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Esecuzione di VM schermate in un cluster Hyper-V

Se si sta provando a distribuire VM schermate in host sorvegliati in cluster (usando un cluster di failover di Windows), è possibile configurare la macchina virtuale schermata in modo che sia a disponibilità elevata usando il cmdlet seguente:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

È ora possibile eseguire la migrazione in tempo reale della macchina virtuale schermata all'interno del cluster.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Distribuire un oggetto schermato con VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)