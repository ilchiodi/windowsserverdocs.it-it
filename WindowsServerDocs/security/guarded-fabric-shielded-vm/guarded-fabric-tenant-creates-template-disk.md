---
title: Macchine virtuali schermate per i tenant - creazione di un disco modello - facoltativo
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2709c84a16dadc2211af4a6f5b43c13266ede155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874632"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>Macchine virtuali schermate per i tenant - creazione di un disco modello (facoltativo)

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per creare una nuova macchina virtuale schermata, è necessario usare un disco modello firmato appositamente preparato. I metadati da dischi modello firmati aiuta a garantire che i dischi non vengono modificati dopo che sono state create e consente un tenant per limitare i dischi può essere utilizzati per creare le macchine virtuali schermate. Un modo per fornire questo disco è per te, il tenant, per crearlo, come descritto in questo argomento. 

> [!IMPORTANT]
> Se si preferisce, è possibile usare invece un disco modello fornito da provider di hosting del servizio. In questo caso, è importante per la distribuzione di un test della macchina virtuale usando tale disco modello ed eseguire i propri strumenti (antivirus, scanner delle vulnerabilità e così via) per convalidare il disco è in realtà, in uno stato attendibile.

## <a name="prepare-an-operating-system-vhdx"></a>Preparare un sistema operativo VHDX

Per creare un disco modello schermato, è necessario innanzitutto preparare un disco del sistema operativo che verrà eseguito tramite la creazione guidata disco modello. Questo disco verrà utilizzato come disco del sistema operativo nelle macchine virtuali schermate. È possibile utilizzare gli strumenti per creare questo disco, ad esempio Microsoft Desktop Image Service Manager (DISM), o configurare una macchina virtuale con un file VHDX vuoti e installare manualmente il sistema operativo su tale disco. Quando si configura il disco, è necessario rispettare i requisiti seguenti sono specifici di generazione 2 e/o macchine virtuali schermate: 

| Requisito per VHDX | Motivo |
|-----------|----|
|Deve essere un disco di tabella di partizione GUID (GPT) | Necessario per le macchine virtuali di generazione 2 supportare UEFI|
|Tipo di disco deve essere **base** anziché **dinamica**. <br>Nota: Si riferisce al tipo di disco logico, non "a espansione dinamica" VHDX funzionalità supportata da Hyper-V. | BitLocker non supporta i dischi dinamici.|
|Il disco con almeno due partizioni. Una partizione deve includere l'unità in cui è installato Windows. Si tratta dell'unità che verrà crittografata. L'altra partizione è la partizione attiva, che contiene il bootloader e rimane non crittografata in modo che il computer può essere avviato.|Necessario per BitLocker|
|È di tipo file system NTFS | Necessario per BitLocker|
|Il sistema operativo installato nel disco VHDX è uno dei seguenti:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>- Windows 10, Windows 8.1, Windows 8| Necessari per supportare macchine virtuali di generazione 2 e il modello di avvio protetto Microsoft|
|Sistema operativo deve essere generalizzato (esecuzione sysprep.exe) | Il provisioning di modello comporta specializzato macchine virtuali per il carico di lavoro di un tenant specifico| 

> [!NOTE]
> Non copiare il disco modello nella libreria VMM in questa fase. 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>I pacchetti per creare un disco modello Nano Server necessari

Se si prevede di eseguire Nano Server come il sistema operativo guest nelle macchine virtuali schermate, è necessario verificare che l'immagine di Nano Server include i pacchetti seguenti:

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Eseguire Windows Update sul sistema operativo modello

Nel disco modello, verificare che il sistema operativo disponga di tutti gli aggiornamenti Windows più recenti installati. Di recente gli aggiornamenti rilasciati migliorano l'affidabilità del processo di schermatura end-to-end: un processo che potrebbe non riuscire a complete se il sistema operativo modello non è aggiornato.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Accedere e proteggere il file VHDX con la creazione guidata disco modello

Per usare un disco modello con le macchine virtuali schermate, il disco deve essere firmato e crittografato con BitLocker. A tale scopo, si utilizzerà la creazione guidata disco schermate di modello. Questa procedura guidata genererà un hash per il disco e aggiungerlo a un catalogo delle firme del volume (VSC). Il VSC viene firmato usando un certificato specifica e viene usato durante il processo di provisioning per assicurarsi che il disco da distribuire per un tenant non è stato modificato o sostituito con un disco che non considera attendibile il tenant. Infine, i BitLocker viene installato nel sistema operativo del disco (se non è già presente) per preparare il disco per la crittografia durante il provisioning della macchina virtuale.

> [!NOTE]
> La creazione guidata disco modello verrà modificato il disco modello si specifica sul posto. È possibile creare una copia del VHDX non protetto prima di eseguire la procedura guidata per eseguire aggiornamenti sul disco in un secondo momento. Non sarà in grado di modificare un disco in cui è stato protetto con la creazione guidata disco modello.

In un computer che esegue Windows Server 2016 (non è necessario essere un host sorvegliato o il server VMM), procedere come segue:

1. Copiare il file VHDX generalizzato creato nella [preparare un sistema operativo VHDX](#prepare-an-operating-system-vhdx) al server, se non è già presente.

2. Installare il **strumenti macchina virtuale schermata** funzionalità dal **strumenti di amministrazione remota del Server** nel computer.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Ottenere o creare un certificato per firmare il VHDX che diventerà il disco modello per nuove VM schermate. Informazioni dettagliate su questo certificato verranno incorporate in un file di dati di schermatura, che autorizza il disco come disco attendibile. Pertanto, è importante ottenere questo certificato da un'autorità di certificazione che si e il servizio di hosting del servizio trust del provider. In scenari aziendali in cui si è il tenant sia l'host, è possibile considerare il rilascio di questo certificato da PKI.

    Se si sta configurando un ambiente di test e si vuole solo usare un certificato autofirmato per firmare il disco modello, eseguire un comando simile al seguente nel computer:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Avviare il **Creazione guidata disco modello** dalle **strumenti di amministrazione** cartella nel menu Start o digitando **TemplateDiskWizard.exe** in un prompt dei comandi.

5. Nel **certificato** pagina, fare clic su **Sfoglia** per visualizzare un elenco di certificati. Selezionare il certificato con cui firmare il modello di disco. Fare clic su **OK** e quindi su **Avanti**.

6. Nella pagina del disco virtuale, fare clic su **esplorare** per selezionare il file VHDX preparata, quindi fare clic su **successivo**.

7. Nella pagina catalogo delle firme, fornire un **il nome del disco** e **versione.** Questi campi sono presenti per facilitare l'identificazione del disco dopo che è stato firmato.

    Ad esempio, per **il nome del disco** è possibile digitare _WS2016_ e per **versione**, _1.0.0.0_

8. Rivedere le selezioni nella pagina della procedura guidata rivedere le impostazioni. Quando fa clic su **genera**, la procedura guidata verrà abilitare BitLocker nel disco modello, calcolare l'hash del disco e creare il catalogo delle firme del Volume, che viene archiviato nei metadati VHDX.

    Attendere fino al termine il processo di firma prima di provare a montare o spostare il disco modello. Questo processo potrebbe richiedere alcuni minuti, a seconda delle dimensioni del disco. 

9. Nel **riepilogo** pagina informazioni sul modello di disco, il certificato usato per firmare il modello, e viene mostrato l'emittente del certificato. Fare clic su **Chiudi** per uscire dalla procedura guidata.


Specificare il modello di disco schermato al provider di hosting del servizio, insieme a un file di dati di schermatura creati dall'utente, come descritto nella [la creazione dei dati per definire una macchina virtuale schermata di schermatura](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="see-also"></a>Vedere anche

- [Distribuire VM schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
