---
title: Creare un disco modello di macchina virtuale schermata di Windows
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: 04fdd52544b69d2c41abcbee00dd00b31bf5f21c
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949781"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Creare un disco modello di macchina virtuale schermata di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2019


Come per le macchine virtuali normali, è possibile creare un modello di macchina virtuale (ad esempio, un [modello di macchina virtuale in Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) per semplificare la distribuzione di nuove macchine virtuali nell'infrastruttura da parte di tenant e amministratori usando un disco modello. Poiché le macchine virtuali schermate sono asset sensibili alla sicurezza, è necessario eseguire altri passaggi per creare un modello di macchina virtuale che supporti la schermatura. In questo argomento vengono illustrati i passaggi per creare un disco modello schermato e un modello di macchina virtuale in VMM.

Per comprendere il modo in cui questo argomento si integra nel processo generale di distribuzione delle macchine virtuali schermate, vedere la [procedura di configurazione del provider di servizi di hosting per gli host sorvegliati e le VM schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Preparare un sistema operativo VHDX

Preparare innanzitutto un disco del sistema operativo che verrà quindi eseguito tramite la creazione guidata disco modello schermato. Questo disco verrà usato come disco del sistema operativo nelle macchine virtuali del tenant. È possibile usare qualsiasi strumento esistente per creare questo disco, ad esempio Microsoft Desktop Image Service Manager (DISM), oppure configurare manualmente una macchina virtuale con un VHDX vuoto e installare il sistema operativo su tale disco. Quando si configura il disco, è necessario rispettare i requisiti seguenti specifici per le macchine virtuali di seconda generazione e/o schermate: 

| Requisito per VHDX | Motivo |
|-----------|----|
|Deve essere un disco della tabella di partizione GUID (GPT) | Necessario per le macchine virtuali di seconda generazione per supportare UEFI|
|Il tipo di disco deve essere di **base** anziché **dinamico**. <br>Nota: si riferisce al tipo di disco logico, non alla funzionalità VHDX "ad espansione dinamica" supportata da Hyper-V. | BitLocker non supporta i dischi dinamici.|
|Il disco contiene almeno due partizioni. Una partizione deve includere l'unità in cui è installato Windows. Si tratta dell'unità che verrà crittografata da BitLocker. L'altra partizione è la partizione attiva, che contiene il bootloader e rimane non crittografata, in modo che il computer possa essere avviato.|Necessaria per BitLocker|
|File system NTFS | Necessaria per BitLocker|
|Il sistema operativo installato in VHDX è uno dei seguenti:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Necessaria per supportare le macchine virtuali di seconda generazione e il modello di avvio protetto Microsoft|
|Il sistema operativo deve essere generalizzato (eseguire Sysprep. exe) | Il provisioning dei modelli implica la specializzazione di macchine virtuali per il carico di lavoro di un tenant specifico| 

> [!NOTE]
> Se si usa VMM, non copiare il disco modello nella libreria VMM in questa fase. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Eseguire Windows Update sul sistema operativo del modello

Sul disco del modello, verificare che nel sistema operativo siano installati tutti gli aggiornamenti di Windows più recenti. Gli aggiornamenti rilasciati di recente migliorano l'affidabilità del processo di schermatura end-to-end, ovvero un processo che potrebbe non essere completato se il sistema operativo del modello non è aggiornato.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Preparare e proteggere VHDX con la creazione guidata disco modello

Per usare un disco modello con VM schermate, il disco deve essere preparato e crittografato con BitLocker usando la creazione guidata disco modello schermato. Questa procedura guidata consente di generare un hash per il disco e di aggiungerlo a un catalogo delle firme del volume (VSC). Il VSC viene firmato usando un certificato specificato e viene usato durante il processo di provisioning per garantire che il disco distribuito per un tenant non sia stato modificato o sostituito con un disco che il tenant non considera attendibile. Infine, BitLocker viene installato sul sistema operativo del disco, se non è già presente, per preparare il disco per la crittografia durante il provisioning della macchina virtuale.

> [!NOTE]
> La creazione guidata disco modello modificherà il disco del modello specificato sul posto. È possibile creare una copia del VHDX non protetto prima di eseguire la procedura guidata per eseguire aggiornamenti al disco in un secondo momento. Non sarà possibile modificare un disco protetto con la creazione guidata disco modello.

Eseguire i passaggi seguenti in un computer in cui è in esecuzione Windows Server 2016, Windows 10 (con strumenti di gestione remota del server, amministrazione remota del server installata) o versioni successive (non è necessario che sia un host sorvegliato o un server VMM):

1. Copiare il VHDX generalizzato creato in [preparare un VHDX del sistema operativo](#prepare-an-operating-system-vhdx) nel server, se non è già presente.

2. Per amministrare il server localmente, installare la funzionalità **strumenti di macchine virtuali schermate** da **strumenti di amministrazione remota del server** sul server.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    È anche possibile amministrare il server da un computer client in cui è stato installato [Windows 10 strumenti di amministrazione remota del server](https://www.microsoft.com/download/details.aspx?id=45520).

3. Ottenere o creare un certificato per firmare il VSC per il VHDX che diventerà il disco modello per le nuove macchine virtuali schermate. I dettagli relativi a questo certificato verranno visualizzati ai tenant quando creeranno i file di dati di schermatura e autorizzano i dischi considerati attendibili. È quindi importante ottenere questo certificato da un'autorità di certificazione reciprocamente attendibile da te e dai tuoi tenant. Negli scenari aziendali in cui si è sia il provider di hosting che il tenant, è possibile prendere in considerazione l'emissione di questo certificato dall'infrastruttura a chiave pubblica (PKI).

    Se si sta configurando un ambiente di testing e si vuole usare un certificato autofirmato per preparare il disco modello, eseguire un comando simile al seguente:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Avviare la **creazione guidata disco modello** dalla cartella **strumenti di amministrazione** nel menu Start o digitando **TemplateDiskWizard. exe** in un prompt dei comandi.

5. Nella pagina **certificato** fare clic su **Sfoglia** per visualizzare un elenco di certificati. Selezionare il certificato con cui preparare il modello di disco. Fare clic su **OK** e quindi su **Avanti**.

6. Nella pagina disco virtuale fare clic su **Sfoglia** per selezionare il VHDX preparato, quindi fare clic su **Avanti**.

7. Nella pagina catalogo di firma specificare un nome e una versione del **disco** descrittivo **.** Questi campi sono disponibili per facilitare l'identificazione del disco dopo che è stato preparato.

    Per il nome del **disco** , ad esempio, è possibile digitare _WS2016_ e per **Version**, _1.0.0.0_

8. Controllare le selezioni effettuate nella pagina Verifica impostazioni della procedura guidata. Quando si fa clic su **genera**, la procedura guidata Abilita BitLocker sul disco modello, calcola l'hash del disco e crea il catalogo delle firme dei volumi, che viene archiviato nei metadati VHDX.

    Attendere il completamento del processo di preparazione prima di provare a montare o spostare il disco modello. Il completamento di questo processo può richiedere del tempo, a seconda delle dimensioni del disco.

    > [!IMPORTANT]
    > I dischi modello possono essere usati solo con il processo di provisioning della macchina virtuale schermato protetto.
    > Il tentativo di avviare una macchina virtuale normale (non schermata) con un disco modello genererà un errore irreversibile (schermata blu) e non è supportato.

9. Nella pagina **Riepilogo** vengono visualizzate le informazioni sul modello di disco, il certificato usato per firmare il VSC e l'autorità emittente del certificato. Fare clic su **Chiudi** per uscire dalla procedura guidata.

Se si usa VMM, seguire i passaggi nelle sezioni rimanenti di questo argomento per incorporare un disco modello in un modello di macchina virtuale schermato in VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copiare il disco modello nella libreria VMM

Se si usa VMM, dopo aver creato un disco modello, è necessario copiarlo in una condivisione di libreria VMM in modo che gli host possano scaricare e usare il disco durante il provisioning di nuove macchine virtuali. Utilizzare la seguente procedura per copiare il disco modello nella libreria VMM e quindi aggiornare la libreria.

1. Copiare il file VHDX nella cartella della condivisione di libreria VMM. Se è stata usata la configurazione predefinita di VMM, copiare il disco modello in _\\<vmmserver>\MSSCVMMLibrary\VHDs_.

2. Aggiornare il server di libreria. Aprire l'area di lavoro **libreria** , espandere **server di libreria**, fare clic con il pulsante destro del mouse sul server di libreria che si desidera aggiornare, quindi scegliere **Aggiorna**.

3. Fornire quindi a VMM le informazioni sul sistema operativo installato nel disco modello:

    a. Trovare il disco modello appena importato nel server di libreria nell'area di lavoro **libreria** .

    b. Fare clic con il pulsante destro del mouse sul disco, quindi scegliere **Proprietà**.

    c. Per **sistema operativo**, espandere l'elenco e selezionare il sistema operativo installato nel disco. La selezione di un sistema operativo indica a VMM che il VHDX non è vuoto.

    d. Dopo aver aggiornato le proprietà, fare clic su **OK**.

L'icona a forma di scudo piccolo accanto al nome del disco indica il disco come disco modello preparato per le macchine virtuali schermate. È anche possibile fare clic con il pulsante destro del mouse sulle intestazioni di colonna e selezionare la colonna **schermata** per visualizzare una rappresentazione testuale che indica se un disco è destinato a distribuzioni di macchine virtuali normali o schermate.

![Disco modello di macchina virtuale schermato](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Creare il modello di macchina virtuale schermata in VMM usando il disco modello preparato

Con un disco modello preparato nella libreria VMM, è possibile creare un modello di macchina virtuale per le macchine virtuali schermate. I modelli di macchina virtuale per le macchine virtuali schermate differiscono leggermente dai modelli di VM tradizionali, in quanto determinate impostazioni sono fisse (VM di seconda generazione, UEFI e avvio protetto abilitato e così via) e altre non sono disponibili (la personalizzazione del tenant è limitata a pochi, selezionare le proprietà della VM) . Per creare il modello di macchina virtuale, seguire questa procedura:

1. Nell'area di lavoro **libreria** fare clic su **Crea modello di macchina virtuale** nella scheda Home nella parte superiore.

2. Nella pagina **Seleziona origine** fare clic su **Utilizza un modello di macchina virtuale esistente o un disco rigido virtuale nella libreria**, quindi fare clic su **Sfoglia**.

3. Nella finestra che viene visualizzata selezionare un disco modello preparato dalla libreria VMM. Per identificare più facilmente i dischi preparati, fare clic con il pulsante destro del mouse su un'intestazione di colonna e abilitare la colonna **schermata** . Fare clic su **OK** , quindi su **Avanti**.

4. Specificare il nome del modello di macchina virtuale e, facoltativamente, una descrizione, quindi fare clic su **Avanti**.

5. Nella pagina **Configura hardware** specificare le funzionalità delle macchine virtuali create da questo modello. Assicurarsi che almeno una scheda di interfaccia di rete sia disponibile e configurata nel modello di macchina virtuale. L'unico modo in cui un tenant si connette a una macchina virtuale schermata è tramite Connessione Desktop remoto, Gestione remota Windows o altri strumenti di gestione remota preconfigurati che funzionano su protocolli di rete.

    Se si sceglie di sfruttare i pool di indirizzi IP statici in VMM anziché eseguire un server DHCP nella rete tenant, sarà necessario avvisare i tenant di questa configurazione. Quando un tenant fornisce il file di dati di schermatura, che contiene il file di installazione automatica per VMM, sarà necessario specificare valori segnaposto speciali per le informazioni sul pool di indirizzi IP statici. Per ulteriori informazioni sui segnaposto VMM nei file di installazione automatica tenant, vedere [creare un file di risposte](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. Nella pagina **Configura sistema operativo** , VMM mostrerà solo alcune opzioni per le macchine virtuali schermate, tra cui il codice Product Key, il fuso orario e il nome del computer. Alcune informazioni sicure, ad esempio la password dell'amministratore e il nome di dominio, vengono specificate dal tenant tramite un file di dati di schermatura (. File PDK). 

    > [!NOTE]
    > Se si sceglie di specificare un codice Product Key in questa pagina, assicurarsi che sia valido per il sistema operativo nel disco modello. Se viene usato un codice Product Key errato, la creazione della macchina virtuale avrà esito negativo.

Una volta creato il modello, i tenant possono usarlo per creare nuove macchine virtuali. È necessario verificare che il modello di macchina virtuale sia una delle risorse disponibili per il ruolo utente Amministratore tenant (in VMM, i ruoli utente si trovano nell'area di lavoro **Impostazioni** ).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Preparare e proteggere il VHDX usando PowerShell

In alternativa all'esecuzione della creazione guidata disco modello, è possibile copiare il disco modello e il certificato in un computer che esegue strumenti di amministrazione remota del server ed eseguire [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) per avviare il processo di firma.
Nell'esempio seguente vengono usate le informazioni sul nome e sulla versione specificate dai parametri _templateName_ e _Version_ .
Le VHDX fornite al parametro `-Path` verranno sovrascritte con il disco modello aggiornato. Assicurarsi quindi di creare una copia prima di eseguire il comando.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

Il disco modello è ora pronto per essere usato per il provisioning di macchine virtuali schermate.
Se si usa System Center Virtual Machine Manager per distribuire la VM, è ora possibile copiare il VHDX nella libreria VMM.

È anche possibile estrarre il catalogo delle firme del volume dal VHDX.
Questo file viene usato per fornire informazioni sul certificato di firma, il nome del disco e la versione ai proprietari delle macchine virtuali che vogliono usare il modello.
È necessario importare questo file nella creazione guidata file di dati di schermatura per autorizzare l'utente, l'autore del modello che possiede il certificato di firma, a creare questo e i dischi modello futuri.

Per estrarre il catalogo delle firme del volume, eseguire il comando seguente in PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare un file di dati di schermatura](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Vedi anche

- [Procedura di configurazione del provider di servizi di hosting per host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
