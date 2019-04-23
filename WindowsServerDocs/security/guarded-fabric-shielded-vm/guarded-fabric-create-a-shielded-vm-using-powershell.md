---
title: Creare una macchina virtuale schermata usando PowerShell
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 76be26e107bd16165367d5432e1dd757dea2f9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855412"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Creare una macchina virtuale schermata usando PowerShell

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

In ambiente di produzione, si utilizza in genere una gestione dell'infrastruttura (ad esempio, VMM) per distribuire le macchine virtuali schermate. Tuttavia, la procedura illustrata di seguito consentono di distribuire e convalidare l'intero scenario senza una gestione dell'infrastruttura.

In breve, si crea un disco modello, un file di dati di schermatura, un file di risposte di installazione automatica e altri elementi di sicurezza in qualsiasi computer, quindi copiare questi file in un host sorvegliato e il provisioning della VM schermata.

## <a name="create-a-signed-template-disk"></a>Creare un disco modello firmato

Per creare una nuova macchina virtuale schermata, è necessario prima di tutto un disco modello VM schermato che viene pre-crittografato con relativo OS volume (o le partizioni di avvio e di radice in Linux) firmato.
Seguire i collegamenti seguenti per altre informazioni su come creare un disco modello.

- [Preparare un disco modello Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Preparare un disco modello Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

È necessario anche una copia del catalogo delle firme del volume del disco per creare il file di dati di schermatura.
Per salvare questo file, eseguire il comando seguente nel computer in cui è stato creato il disco modello:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Scaricare i metadati di sorveglianza

Per ognuna delle infrastrutture di virtualizzazione in cui si vuole eseguire la macchina virtuale schermata, è necessario ottenere i metadati di sorveglianza per i cluster HGS delle infrastrutture.
Il provider di hosting deve essere in grado di fornire queste informazioni per l'utente.

Se si trovano in un ambiente aziendale e possono comunicare con il server HGS, i metadati di sorveglianza sono disponibile all'indirizzo *http://\<HGSCLUSTERNAME\>/KeyProtection/service/metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>Creare file di dati di schermatura (PDK)

I dati di schermatura viene creato e i proprietari della macchina virtuale del tenant di proprietà e contengano i segreti necessari per creare le macchine virtuali schermate che devono essere protette per l'amministratore dell'infrastruttura, ad esempio la password di amministratore della macchina virtuale schermata.
I dati di schermatura sono crittografato in modo che possa essere decrittografato solo i server HGS e tenant.
Dopo aver creato dal proprietario del tenant o della VM, il file con estensione PDK risulta deve essere copiato per l'infrastruttura sorvegliata.
Per altre informazioni, vedere [quali sia i dati di schermatura e perché sono necessari?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Inoltre, è necessario un file di risposte di installazione automatica (Unattend. XML per Windows, a seconda per Linux). Visualizzare [creare un file di risposte](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) per indicazioni su cosa includere nel file di risposte.

Eseguire i cmdlet seguenti in un computer con gli strumenti di amministrazione remota del Server per le macchine virtuali schermate installato.
Se si sta creando un PDK per una VM Linux, è necessario farlo in un server che esegue Windows Server, versione 1709 o successiva.

 
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
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Macchina virtuale schermata di effettuare il provisioning in un host sorvegliato
Copiare il file del disco modello (ServerOS.vhdx) e il file con estensione PDK (contoso.pdk) per l'host sorvegliato prepararsi per la distribuzione.

Nell'host sorvegliati, installare il modulo di PowerShell di strumenti dell'infrastruttura protetta, che contiene il cmdlet New-ShieldedVM per semplificare il processo di provisioning. Se l'host sorvegliato può accedere a Internet, eseguire il comando seguente:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

È anche possibile scaricare il modulo in un altro computer con Internet accedere e copiare il modulo risulta `C:\Program Files\WindowsPowerShell\Modules` nell'host sorvegliato.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Dopo aver installato il modulo, si è pronti a effettuare il provisioning di VM schermate.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Se il disco modello contiene un sistema operativo basato su Linux, includere il `-Linux` flag quando si esegue il comando:

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Controllare il contenuto di assistenza mediante `Get-Help New-ShieldedVM -Full` per altre informazioni sulle altre opzioni è possibile passare al cmdlet.

Una volta che la macchina virtuale completa il provisioning, verrà attivata la fase di specializzazione del sistema operativo specifico, dopo il quale sarà pronto per l'uso.
Assicurarsi di connettere la macchina virtuale a una rete valida, quindi è possibile connettersi a esso una volta che è in esecuzione (tramite lo strumento di gestione Preferiti, PowerShell, SSH o RDP).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Macchine virtuali schermate in esecuzione in un cluster Hyper-V

Se si tenta di distribuire VM schermate sugli host sorvegliati in cluster (usando un Cluster di Failover di Windows), è possibile configurare la macchina virtuale schermata per la disponibilità elevata usando il cmdlet seguente:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

La macchina virtuale schermata a questo punto è possibile in tempo reale la migrazione all'interno del cluster.

## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Distribuire un schermate con VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)