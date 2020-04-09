---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: Macchine virtuali schermate per i tenant-creazione di una nuova macchina virtuale schermata in locale e trasferimento in un'infrastruttura sorvegliata
ms.prod: windows-server
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a5ca3ab29b83d0cb6cb2d55507471790f65800a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856724"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Macchine virtuali schermate per i tenant-creazione di una nuova macchina virtuale schermata in locale e trasferimento in un'infrastruttura sorvegliata

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive i passaggi per creare una macchina virtuale schermata usando solo Hyper-V; ovvero senza Virtual Machine Manager, dischi modello o un file di dati di schermatura. Si tratta di uno scenario insolito per la maggior parte degli ambienti di hosting su cloud pubblico, ma può essere utile quando si esegue il test di un'infrastruttura sorvegliata o in scenari aziendali in cui una macchina virtuale viene spostata da un'infrastruttura di reparto a un'infrastruttura IT condivisa ed è necessario crittografarla prima della migrazione.

Per comprendere il modo in cui questo argomento si integra nel processo generale di distribuzione delle macchine virtuali schermate, vedere la [procedura di configurazione del provider di servizi di hosting per gli host sorvegliati e le VM schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importare la configurazione di sorveglianza nel server Hyper-V tenant

1.  Prima di iniziare la procedura, verificare che si trovi in un host Hyper-V che esegue Windows Server 2016 con i ruoli e le funzionalità seguenti installati:

    - Role

        - Hyper-V

    - Caratteristiche

        - Strumenti di amministrazione della funzionalità di Strumenti di amministrazione remota del server\\\\strumenti di VM schermate

    > [!NOTE]
    > L'host usato qui *non* deve essere un host nell'infrastruttura sorvegliata. Si tratta di un host separato in cui le VM vengono preparate prima di essere spostate nell'infrastruttura sorvegliata.

2.  Prima di poter eseguire una macchina virtuale schermata in questo computer, è necessario assicurarsi che la sicurezza basata sulla virtualizzazione sia abilitata e in esecuzione. Eseguire il comando seguente in una finestra di Windows PowerShell con privilegi elevati per installare la funzionalità di supporto per sorveglianza host per Hyper-V. Verranno configurate tutte le impostazioni necessarie nel computer per poter eseguire macchine virtuali schermate.

        Install-WindowsFeature HostGuardian

3.  Sarà necessario acquisire i metadati del tutore per l'infrastruttura sorvegliata in cui viene eseguita la macchina virtuale. Questi metadati vengono usati per autorizzare l'infrastruttura a eseguire la macchina virtuale schermata. Il modo in cui si ottengono queste informazioni sarà diverso per ogni provider di servizi di hosting o Enterprise. Il provider di hosting (o se si ha accesso alla rete dell'infrastruttura sorvegliata) può acquisire queste informazioni eseguendo il comando di Windows PowerShell seguente:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    Nell'esempio precedente, "HGS" è il nome di rete distribuito del cluster HGS e "Bastion. local" è il nome del dominio HGS.

4.  Per importare la chiave di sorveglianza, che sarà necessaria in una procedura successiva, eseguire il comando seguente.

    Per &lt;percorso&gt;&lt;filename&gt;, sostituire il percorso e il nome file del file XML salvato nel passaggio precedente, ad esempio: **C:\\temp\\GuardianKey. XML**

    Per &lt;Guardianname&gt;, specificare un nome per il provider di hosting o il data center aziendale, ad esempio **HostingProvider1**. Registrare il nome per la procedura successiva.

    Include **-AllowUntrustedRoot** solo se il server HGS è stato configurato con certificati autofirmati. Questi certificati fanno parte del servizio di protezione delle chiavi in HGS.

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Creare una nuova macchina virtuale schermata nell'host

In questa procedura verrà creata una macchina virtuale nell'host Hyper-V e verrà preparata per l'esportazione al provider di hosting o all'amministratore del Data Center, che potrà eseguirla in un host sorvegliato.

Come parte della procedura, si creerà una protezione con chiave che contiene due elementi importanti:

-   **Proprietario**: nella protezione con chiave, o più probabilmente, il gruppo in cui si lavora, che condivide gli elementi di sicurezza, ad esempio i certificati, viene identificato come "proprietario" della macchina virtuale. L'identità del proprietario è rappresentata da un certificato che, se si eseguono i comandi come illustrato, viene generato come certificato autofirmato. Facoltativamente, è possibile usare invece un certificato supportato dall'infrastruttura PKI e omettere il parametro **-AllowUntrustedRoot** nei comandi.

-   **Tutori**: anche nella protezione con chiave, il provider di hosting o il data center aziendale (che esegue HGS e host sorvegliati) viene identificato come "tutore". Il Guardian è rappresentato dalla chiave di sorveglianza importata nella procedura precedente, [Importa la configurazione di Guardian nel server Hyper-V tenant](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Per un'illustrazione della protezione con chiave, che è un elemento in un file di dati di schermatura, vedere [che cos'è la schermatura dei dati e perché è necessaria?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1. Per creare una nuova macchina virtuale di seconda generazione in un host Hyper-V tenant, eseguire il comando seguente.

   Per &lt;&gt;ShieldedVMname, specificare un nome per la macchina virtuale, ad esempio: **ShieldVM1**
    
   Per &lt;&gt;VHDPath, specificare un percorso in cui archiviare il VHDX della macchina virtuale, ad esempio: **C:\\vm\\ShieldVM1\\ShieldVM1. vhdx**
    
   Per &lt;&gt;nnGB, specificare una dimensione per il VHDX, ad esempio: **60 GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. Installare un sistema operativo supportato (Windows Server 2012 o versione successiva, Windows 8 client o versione successiva) nella macchina virtuale e abilitare la connessione Desktop remoto e la regola del firewall corrispondente. Registrare l'indirizzo IP della macchina virtuale e/o il nome DNS; sarà necessario per connettersi in remoto.

3. Usare RDP per connettersi in remoto alla macchina virtuale e verificare che RDP e il firewall siano configurati correttamente. Come parte del processo di schermatura, l'accesso alla console alla macchina virtuale tramite Hyper-V verrà disabilitato, pertanto è importante assicurarsi di essere in grado di gestire il sistema in modalità remota tramite la rete.

4. Per creare una nuova protezione con chiave, descritta all'inizio di questa sezione, eseguire il comando seguente.

   Per &lt;Guardianname&gt;, usare il nome specificato nella procedura precedente, ad esempio: **HostingProvider1**

   Includere **-AllowUntrustedRoot** per consentire i certificati autofirmati.

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   Se si desidera che più di un Data Center sia in grado di eseguire la macchina virtuale schermata (ad esempio, un sito di ripristino di emergenza e un provider di cloud pubblico), è possibile fornire un elenco di tutori al parametro **-Guardian** . Per ulteriori informazioni, vedere [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5. Per abilitare vTPM usando la protezione con chiave, eseguire il comando seguente. Per &lt;&gt;ShieldedVMname, usare lo stesso nome di macchina virtuale usato nei passaggi precedenti.

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. Per avviare la macchina virtuale per verificare che la protezione con chiave funzioni con i certificati del proprietario locale, eseguire il comando seguente.

       Start-VM -Name $VMName

7. Verificare che la macchina virtuale sia stata avviata nella console di Hyper-V.

8. Usare RDP per connettersi in remoto alla macchina virtuale e abilitare BitLocker su tutte le partizioni in tutti VHDX collegati alla macchina virtuale schermata.

   > [!IMPORTANT]
   > Prima di procedere con il passaggio successivo, attendere che la crittografia BitLocker venga completata in tutte le partizioni in cui è stata abilitata.

9. Arrestare la macchina virtuale quando si è pronti a spostarla nell'infrastruttura sorvegliata.

10. Nel server Hyper-V tenant esportare la macchina virtuale usando lo strumento di propria scelta (Windows PowerShell o la console di gestione di Hyper-V). Quindi, organizzare i file da copiare in un host sorvegliato gestito dal provider di hosting o dal data center aziendale.

11. **Per il provider di hosting o il data center aziendale**:

    Importare la macchina virtuale schermata usando la console di gestione di Hyper-V o Windows PowerShell. Per poter avviare la macchina virtuale, è necessario importare il file di configurazione della macchina virtuale dal proprietario della macchina virtuale. Ciò è dovuto al fatto che la protezione con chiave e il TPM virtuale della VM vengono archiviati nel file di configurazione. Se la macchina virtuale è configurata per l'esecuzione nell'infrastruttura sorvegliata, dovrebbe essere in grado di avviarsi correttamente.

## <a name="see-also"></a>Vedere anche

- [Procedura di configurazione del provider di servizi di hosting per host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
