---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: Le macchine virtuali schermate per i tenant - creazione di una nuova schermata della macchina virtuale in locale e spostarlo in un'infrastruttura sorvegliata
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2da1e33d24fa6d68815f4fbc0891be0616004856
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817472"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Le macchine virtuali schermate per i tenant - creazione di una nuova schermata della macchina virtuale in locale e spostarlo in un'infrastruttura sorvegliata

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


<!-- NOTE THAT THIS FILE HAS A "redirect_url" LINE IN THE METADATA. EVENTUALLY WE WILL PROBABLY STRIP OUT THE DETAILED METADATA AND THE CONTENT BELOW, SO IT'S PURELY A REDIRECTED TOPIC. However, as of mid-November 2016, we're still deciding. -->



In questo argomento vengono descritti i passaggi per creare una macchina virtuale schermata usando solo Hyper-V; vale a dire senza Virtual Machine Manager, i dischi modello o un file di dati di schermatura. Questo è uno scenario comune per il cloud pubblico più ambienti di hosting, ma può essere utile durante il test di un'infrastruttura sorvegliata o in enterprise scenari in cui una macchina virtuale viene spostata da un reparto fabric condiviso infrastruttura IT e devono essere crittografati prima della migrazione.

Per comprendere come in questo argomento si integra nel processo complessivo della distribuzione di macchine virtuali schermate, vedere [passaggi di configurazione di provider di servizi di Hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importare la configurazione di sorveglianza nel server tenant Hyper-V

1.  Prima di iniziare la procedura, verificare che trovano in un host Hyper-V che esegue Windows Server 2016 con i seguenti ruoli e funzionalità installate:

    - Ruolo

        - Hyper-V

    - Funzionalità

        - Strumenti di amministrazione remota del Server\\strumenti di amministrazione delle funzionalità\\schermate degli strumenti della macchina virtuale

    > [!NOTE]
    > L'host utilizzato qui dovrebbe *non* come host nell'infrastruttura sorvegliata. Si tratta di un host separato in cui le macchine virtuali siano preparate prima di essere passati per l'infrastruttura sorvegliata.

2.  Prima di poter eseguire una macchina virtuale schermata in questo computer, è necessario garantire che la sicurezza basata sulla virtualizzazione sia abilitato e in esecuzione. Eseguire il comando seguente in una finestra di Windows PowerShell con privilegi elevata per installare la funzionalità di supporto Hyper-V per sorveglianza Host. Si configurerà tutte le impostazioni necessarie nel computer sia in grado di eseguire le macchine virtuali schermate.

        Install-WindowsFeature HostGuardian

3.  È necessario acquisire i metadati di sorveglianza per l'infrastruttura sorvegliata in cui verrà eseguita la macchina virtuale. Questi metadati vengano usato per autorizzare tale infrastruttura a eseguire VM schermate. Come ottenere queste informazioni saranno diversi per ogni provider di servizi o enterprise. Il provider di servizi di hosting (oppure, se si ha accesso alla rete dell'infrastruttura sorvegliata) è possibile acquisire queste informazioni eseguendo il comando Windows PowerShell seguente:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    Nell'esempio precedente, "hgs" è il nome rete distribuita del cluster HGS e "bastion.local" è il nome del dominio HGS.

4.  Per importare la chiave di sorveglianza, che sarà necessario in una procedura successiva, eseguire il comando seguente.

    Per &lt;tracciato&gt;&lt;Filename&gt;, sostituire il percorso e nome file del codice XML del file viene salvato nel passaggio precedente, ad esempio: **C:\\temp\\GuardianKey.xml**

    Per la &lt;GuardianName&gt;, specificare un nome per il provider di hosting o di un data center aziendale, ad esempio **HostingProvider1**. Prendere nota del nome per la procedura successiva.

    Includere **- AllowUntrustedRoot** solo se il server HGS è configurato con certificati autofirmati. (Questi certificati sono parte del servizio di protezione chiave nel servizio HGS).

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Creare una nuova macchina virtuale schermata nell'host

In questa procedura si crea una macchina virtuale nell'host Hyper-V e prepararlo per l'esportazione per il provider di hosting o l'amministratore di Data Center, che sia possibile eseguirlo in un host sorvegliato.

Come parte della procedura, si creerà una protezione con chiave che contiene due elementi importanti:

-   **Owner**: Nella protezione con chiave, si - o più probabile, il gruppo lavorare, che condivide gli elementi di sicurezza, ad esempio certificati - sono stato identificato come "proprietario" della macchina virtuale. L'identità come proprietario è rappresentata da un certificato che, se si eseguono i comandi, come illustrato, viene generato un certificato autofirmato. Facoltativamente, è possibile invece usare un certificato supportato da infrastruttura PKI e omettere il **- AllowUntrustedRoot** parametro nei comandi.

-   **Guardians**: Anche nella protezione con chiave, il provider di hosting o di un data center aziendale (che esegue servizio HGS e gli host sorvegliati) viene identificato come "sorveglianza". Di sorveglianza è rappresentato dalla chiave importata nella procedura precedente, sorveglianza [importare la configurazione di sorveglianza su server Hyper-V tenant](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Per un'illustrazione che mostra la protezione con chiave, ovvero un elemento in un file di dati di schermatura, vedere [quali sia i dati di schermatura e perché sono necessari?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1.  Per un tenant di host Hyper-V, eseguire il comando seguente per creare una macchina virtuale di nuova generazione 2.

    Per la &lt;ShieldedVMname&gt;, specificare un nome per la macchina virtuale, ad esempio: **ShieldVM1**
    
    Per la &lt;VHDPath&gt;, specificare un percorso per archiviare il file VHDX della macchina virtuale, ad esempio: **C:\\VMs\\ShieldVM1\\ShieldVM1.vhdx**
    
    Per la &lt;nnGB&gt;, specificare una dimensione per VHDX, ad esempio: **60GB**

        New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2.  Installare un sistema operativo supportato (client di Windows Server 2012 o versioni successive, Windows 8 o versione successiva) nella macchina virtuale e abilitare la connessione desktop remoto e una regola del firewall corrispondente. Registrare l'indirizzo IP e/o nome DNS; della macchina virtuale sarà necessario connettersi in modalità remota a esso.

3.  Usare RDP per connettersi alla macchina virtuale e verificare che il protocollo RDP e il firewall siano configurate correttamente in modalità remota. Come parte del processo di schermatura, console per la macchina virtuale tramite Hyper-V verrà disabilitato, pertanto è importante per assicurarsi che sia possibile gestire in remoto il sistema in rete.

4.  Per creare una nuova protezione con chiave (descritto all'inizio di questa sezione), eseguire il comando seguente.

    Per la &lt;GuardianName&gt;, usare il nome specificato nella procedura precedente, ad esempio: **HostingProvider1**

    Includere **- AllowUntrustedRoot** per consentire i certificati autofirmati.

        $Guardian = Get-HgsGuardian -Name '<GuardianName>'

        $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

        $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

    Se si desidera essere in grado di eseguire la macchina virtuale schermata (ad esempio, un sito di ripristino di emergenza e un provider di cloud pubblico) più Data Center, è possibile fornire un elenco dei sorveglianti per il **-sorveglianza** parametro. Per altre informazioni, vedere [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5.  Per abilitare il vTPM usando la protezione con chiave, eseguire il comando seguente. Per la &lt;ShieldedVMname&gt;, usare lo stesso nome VM usato nei passaggi precedenti.

        $VMName="<ShieldedVMname>"

        Stop-VM -Name $VMName -Force

        Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

        Set-VMSecurityPolicy -VMName $VMName -Shielded $true

        Enable-VMTPM -VMName $VMName

6.  Per avviare la macchina virtuale per verificare che la protezione con chiave funziona con i certificati proprietario locale, eseguire il comando seguente.

        Start-VM -Name $VMName

7.  Verificare che la macchina virtuale sia stato avviato nella console di Hyper-V.

8.  Usare RDP per connettersi alla macchina virtuale e abilitare BitLocker in tutte le partizioni di tutti i Vhdx collegati a VM schermate in modalità remota.

    > [!IMPORTANT]
    > Prima di procedere al passaggio successivo, attendere che la crittografia BitLocker terminare in tutte le partizioni in cui è abilitato.

9.  Arrestare la macchina virtuale quando si è pronti per passare all'infrastruttura sorvegliata.

10.  Nel server tenant Hyper-V, esportare la macchina virtuale usando lo strumento di propria scelta (Windows PowerShell o gestione di Hyper-V). Quindi, organizzate per i file da copiare in un host sorvegliato gestito dal provider di hosting o data center aziendale.

11.  **Per il data center aziendale o del provider hosting**:

    Importare la macchina virtuale schermata usando la gestione di Hyper-V o Windows PowerShell. È necessario importare il file di configurazione della macchina virtuale da parte del proprietario della macchina virtuale per avviare la macchina virtuale. Questo avviene perché la protezione con chiave e TPM virtuale della macchina virtuale vengono archiviati nel file di configurazione. Se la macchina virtuale è configurata per l'esecuzione in dell'infrastruttura sorvegliata, deve essere in grado di avviare correttamente.

## <a name="see-also"></a>Vedere anche

- [Passaggi di configurazione di provider di servizi di hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
