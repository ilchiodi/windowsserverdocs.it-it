---
title: Creare un disco modello VM schermato Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: e00322186ea34784048366bf17881af742cb4444
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853692"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Creare un disco modello VM schermato Windows

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Come con normali macchine virtuali, è possibile creare un modello di macchina virtuale (ad esempio, un [modello di macchina virtuale in Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) per renderla facilmente i tenant e amministratori di distribuire nuove macchine virtuali nell'infrastruttura usando un disco modello. Poiché le macchine virtuali schermate sono risorse sensibili alla sicurezza, sono disponibili passaggi aggiuntivi per creare un modello di macchina virtuale che supporta la schermatura. In questo argomento illustra i passaggi per creare un disco modello schermato e un modello di macchina virtuale in VMM.

Per comprendere come in questo argomento si integra nel processo complessivo della distribuzione di macchine virtuali schermate, vedere [passaggi di configurazione di provider di servizi di Hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Preparare un sistema operativo VHDX

Preparare un disco del sistema operativo che verrà quindi eseguito tramite la creazione guidata disco schermate di modello. Questo disco verrà utilizzato come disco del sistema operativo nelle macchine virtuali del tenant. È possibile utilizzare gli strumenti per creare questo disco, ad esempio Microsoft Desktop Image Service Manager (DISM), o configurare una macchina virtuale con un file VHDX vuoti e installare manualmente il sistema operativo su tale disco. Quando si configura il disco, è necessario rispettare i requisiti seguenti sono specifici di generazione 2 e/o macchine virtuali schermate: 

| Requisito per VHDX | Motivo |
|-----------|----|
|Deve essere un disco di tabella di partizione GUID (GPT) | Necessario per le macchine virtuali di generazione 2 supportare UEFI|
|Tipo di disco deve essere **base** anziché **dinamica**. <br>Nota: Si riferisce al tipo di disco logico, non "a espansione dinamica" VHDX funzionalità supportata da Hyper-V. | BitLocker non supporta i dischi dinamici.|
|Il disco con almeno due partizioni. Una partizione deve includere l'unità in cui è installato Windows. Si tratta dell'unità che verrà crittografata. L'altra partizione è la partizione attiva, che contiene il bootloader e rimane non crittografata in modo che il computer può essere avviato.|Necessario per BitLocker|
|È di tipo file system NTFS | Necessario per BitLocker|
|Il sistema operativo installato nel disco VHDX è uno dei seguenti:<br>-Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>- Windows 10, Windows 8.1, Windows 8| Necessari per supportare macchine virtuali di generazione 2 e il modello di avvio protetto Microsoft|
|Sistema operativo deve essere generalizzato (esecuzione sysprep.exe) | Il provisioning di modello comporta specializzato macchine virtuali per il carico di lavoro di un tenant specifico| 

> [!NOTE]
> Se si usa VMM, non copiare il disco modello nella libreria VMM in questa fase. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Eseguire Windows Update sul sistema operativo modello

Nel disco modello, verificare che il sistema operativo disponga di tutti gli aggiornamenti Windows più recenti installati. Di recente gli aggiornamenti rilasciati migliorano l'affidabilità del processo di schermatura end-to-end: un processo che potrebbe non riuscire a complete se il sistema operativo modello non è aggiornato.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Preparare e proteggere il file VHDX con la creazione guidata disco modello

Per usare un disco modello con le macchine virtuali schermate, il disco deve essere preparato e crittografato con BitLocker mediante la creazione guidata disco schermate di modello. Questa procedura guidata genererà un hash per il disco e aggiungerlo a un catalogo delle firme del volume (VSC). Il VSC viene firmato usando un certificato specifica e viene usato durante il processo di provisioning per assicurarsi che il disco da distribuire per un tenant non è stato modificato o sostituito con un disco che non considera attendibile il tenant. Infine, i BitLocker viene installato nel sistema operativo del disco (se non è già presente) per preparare il disco per la crittografia durante il provisioning della macchina virtuale.

> [!NOTE]
> La creazione guidata disco modello verrà modificato il disco modello si specifica sul posto. È possibile creare una copia del VHDX non protetto prima di eseguire la procedura guidata per eseguire aggiornamenti sul disco in un secondo momento. Non sarà in grado di modificare un disco in cui è stato protetto con la creazione guidata disco modello.

In un computer che esegue Windows Server 2016 (non è necessario essere un host sorvegliato o un server VMM), procedere come segue:

1. Copiare il file VHDX generalizzato creato nella [preparare un sistema operativo VHDX](#prepare-an-operating-system-vhdx) al server, se non è già presente.

2. Per amministrare il server in locale, installare il **strumenti macchina virtuale schermata** funzionalità dal **strumenti di amministrazione remota del Server** nel server.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    È anche possibile gestire il server da un computer client in cui è stata installata il [strumenti di amministrazione di Windows 10 remoto Server](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

3. Ottenere o creare un certificato per firmare il VSC per il file VHDX che diventerà il disco modello per nuove VM schermate. Informazioni dettagliate su questo certificato verranno visualizzate ai tenant quando si creano i file di dati di schermatura e autorizza i dischi di che cui si fidano. Pertanto, è importante ottenere questo certificato da un'autorità di certificazione attendibile reciprocamente dall'utente e i tenant. In scenari aziendali in cui si è il tenant sia l'host, è possibile considerare il rilascio di questo certificato da PKI.

    Se si sta configurando un ambiente di test e si vuole solo usare un certificato autofirmato per preparare il disco modello, eseguire un comando simile al seguente:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Avviare il **Creazione guidata disco modello** dalle **strumenti di amministrazione** cartella nel menu Start o digitando **TemplateDiskWizard.exe** in un prompt dei comandi.

5. Nel **certificato** pagina, fare clic su **Sfoglia** per visualizzare un elenco di certificati. Selezionare il certificato da utilizzare per preparare il disco modello. Fare clic su **OK** e quindi su **Avanti**.

6. Nella pagina del disco virtuale, fare clic su **esplorare** per selezionare il file VHDX preparata, quindi fare clic su **successivo**.

7. Nella pagina catalogo delle firme, fornire un **il nome del disco** e **versione.** Questi campi sono presenti per facilitare l'identificazione del disco dopo aver preparato.

    Ad esempio, per **il nome del disco** è possibile digitare _WS2016_ e per **versione**, _1.0.0.0_

8. Rivedere le selezioni nella pagina della procedura guidata rivedere le impostazioni. Quando fa clic su **genera**, la procedura guidata verrà abilitare BitLocker nel disco modello, calcolare l'hash del disco e creare il catalogo delle firme del Volume, che viene archiviato nei metadati VHDX.

    Attendere fino al termine del processo di preparazione prima di provare a montare o spostare il disco modello. Questo processo potrebbe richiedere alcuni minuti, a seconda delle dimensioni del disco.

    > [!IMPORTANT]
    > Dischi modello possono essere utilizzati solo con la macchina virtuale schermata sicura processo di provisioning.
    > Il tentativo di eseguire l'avvio di una normale macchina virtuale (eseguendolo) usando un disco modello comporterà un errore irreversibile (schermata blu) e non è supportata.

9. Nel **riepilogo** pagina informazioni sul modello di disco, il certificato usato per firmare il VSC e viene mostrato l'emittente del certificato. Fare clic su **Chiudi** per uscire dalla procedura guidata.

Se si usa VMM, seguire i passaggi nelle sezioni rimanenti di questo argomento per incorporare un disco modello in un modello di VM schermato in VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copiare il disco modello nella libreria VMM

Se si usa VMM, dopo aver creato un disco modello, è necessario copiarlo in una condivisione di libreria VMM in modo che gli host possono scaricare e usare il disco durante il provisioning di nuove macchine virtuali. Utilizzare la procedura seguente per copiare il disco modello nella libreria VMM e quindi aggiornare la libreria.

1. Copiare il file VHDX per la cartella di condivisione di libreria VMM. Se si usa la configurazione di VMM predefinito, copiare il disco modello  _\\ <vmmserver>\MSSCVMMLibrary\VHDs_.

2. Aggiornare il server di libreria. Aprire il **libreria** dell'area di lavoro, espandere **server di libreria**, fare clic sul server di libreria che si desidera aggiornare e fare clic su **Aggiorna**.

3. Successivamente, fornire a VMM con informazioni sul sistema operativo installato nel disco modello:

    a. Trovare il disco modello appena importata nel server di libreria nel **libreria** dell'area di lavoro.

    b. Fare doppio clic su disco e quindi fare clic su **proprietà**.

    c. Per la **sistema operativo**, espandere l'elenco e selezionare il sistema operativo installato nel disco. Selezione di un sistema operativo indica a VMM che il VHDX non è vuoto.

    d. Dopo aver aggiornato le proprietà, fare clic su **OK**.

L'icona dello scudo di piccole dimensioni accanto al nome del disco indica il disco come disco modello preparato per le macchine virtuali schermate. È anche possibile con il pulsante destro scegliere le intestazioni di colonna e attiva/disattiva il **schermate** colonna per visualizzare una rappresentazione testuale che indica se un disco ha l'obiettivo per distribuzioni di macchine Virtuali schermate o regolari.

![Disco di modello di macchina virtuale schermata](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Creare il modello di VM schermato in VMM usando il disco modello preparato

Con un disco modello preparato nella libreria VMM, si è pronti per creare un modello di macchina virtuale per le macchine virtuali schermate. Modelli di macchina virtuale per le macchine virtuali schermate sono leggermente diverse da modelli di macchine Virtuali tradizionali in quanto alcune impostazioni sono fisse (generazione 2 macchina virtuale, UEFI e avvio protetto abilitato e così via) e altri utenti non saranno disponibili (personalizzazione di tenant è limitata a poche, selezionare proprietà della macchina virtuale) . Per creare il modello di macchina virtuale, seguire i passaggi seguenti:

1. Nel **Library** dell'area di lavoro, fare clic su **Crea modello di macchina virtuale** della scheda home nella parte superiore.

2. Nella pagina **Seleziona origine** fare clic su **Utilizza un modello di macchina virtuale esistente o un disco rigido virtuale nella libreria**, quindi fare clic su **Sfoglia**.

3. Nella finestra visualizzata, selezionare un disco modello preparato dalla libreria VMM. Per identificare più facilmente quali dischi sono pronti, fare doppio clic su un'intestazione di colonna e abilitare la **schermate** colonna. Fare clic su **OK** quindi **successivo**.

4. Specificare il nome di un modello di macchina virtuale e facoltativamente una descrizione e quindi fare clic su **successivo**.

5. Nel **Configura Hardware** , specificare le funzionalità delle macchine virtuali create da questo modello. Assicurarsi che almeno una scheda di rete sia disponibile e configurato nel modello di macchina virtuale. L'unico modo per un tenant per la connessione a una macchina virtuale schermata è tramite connessione Desktop remoto, gestione remota Windows o altri strumenti di gestione remota preconfigurata che funzionano tramite protocolli di rete.

    Se si sceglie di usare i pool IP statici in VMM anziché eseguire un server DHCP sulla rete tenant, è necessario per i tenant a questa configurazione di avviso. Quando un tenant fornisce i file di dati di schermatura, che contiene il file di installazione automatica per VMM, sarà necessario fornire i valori segnaposto speciale per le informazioni sul pool IP statici. Per altre informazioni su VMM segnaposto nel file di installazione automatica di tenant, vedere [creare un file di risposte](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. Nel **Configura sistema operativo** pagina, VMM mostrerà solo alcune opzioni per le macchine virtuali schermate, inclusi il codice product key, fuso orario e nome del computer. Alcune informazioni protette, ad esempio il nome di dominio e password amministratore, viene specificati dal tenant tramite un file di dati di schermatura (. File con estensione PDK). 

    > [!NOTE]
    > Se si sceglie di specificare un codice product key in questa pagina, assicurarsi che è valido per il sistema operativo nel disco modello. Se viene usata una chiave di prodotto non corrette, la creazione della macchina virtuale avrà esito negativo.

Dopo aver creato il modello, i tenant possono usarlo per creare nuove macchine virtuali. È necessario verificare che il modello di macchina virtuale sia presente tra le risorse disponibili per il ruolo utente amministratore Tenant (in VMM, i ruoli utente sono nel **impostazioni** dell'area di lavoro).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Preparare e proteggere il file VHDX usando PowerShell

Come alternativa all'esecuzione della procedura guidata disco modello, è possibile copiare il disco modello e un certificato in un computer che esegue Amministrazione remota del server ed eseguire [Proteggi TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) per avviare il processo di firma.
L'esempio seguente usa le informazioni di nome e la versione specificate per il _TemplateName_ e _versione_ parametri.
Il file VHDX è fornire la `-Path` parametro verrà sovrascritto con il disco modello aggiornato, pertanto assicurarsi di creare una copia prima di eseguire il comando.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

Il disco modello è ora pronto per essere usato per effettuare il provisioning schermato macchine virtuali.
Se si usa System Center Virtual Machine Manager per distribuire la VM, è ora possibile copiare il file VHDX per la libreria VMM.

È anche possibile estrarre il catalogo delle firme del volume da nel disco VHDX.
Questo file viene usato per fornire informazioni sul certificato di firma, il nome del disco e versione ai proprietari della macchina virtuale che desiderano utilizzare un modello.
È necessario importare questo file in guidata File dati di schermatura per concedere l'autorizzazione, all'autore del modello in possesso del certificato di firma, per creare questa e modello future dischi per loro.

Per estrarre il catalogo delle firme del volume, eseguire il comando seguente in PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Creare un file di dati di schermatura](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Vedere anche

- [Passaggi di configurazione di provider di servizi di hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
