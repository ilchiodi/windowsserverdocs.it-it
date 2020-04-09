---
title: VM schermate per i tenant-creazione di un disco modello-facoltativo
ms.prod: windows-server
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1f51a0f90f60847929f6fe46732c98f355a6a859
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856444"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>VM schermate per i tenant-creazione di un disco modello (facoltativo)

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per creare una nuova macchina virtuale schermata, è necessario usare un disco modello firmato appositamente preparato. I metadati dai dischi modello firmati consentono di garantire che i dischi non vengano modificati dopo che sono stati creati e consentono all'utente come tenant di limitare i dischi che possono essere usati per creare le macchine virtuali schermate. Un modo per fornire questo disco è il tenant per crearlo, come descritto in questo argomento. 

> [!IMPORTANT]
> Se si preferisce, è possibile usare invece un disco modello fornito dal provider di servizi di hosting. In tal caso, è importante distribuire una macchina virtuale di test usando il disco del modello ed eseguire i propri strumenti (antivirus, scanner di vulnerabilità e così via) per convalidare che il disco sia, in realtà, in uno stato attendibile.

## <a name="prepare-an-operating-system-vhdx"></a>Preparare un sistema operativo VHDX

Per creare un disco modello schermato, è necessario innanzitutto preparare un disco del sistema operativo che verrà eseguito tramite la creazione guidata disco modello. Questo disco verrà usato come disco del sistema operativo nelle VM schermate. È possibile usare qualsiasi strumento esistente per creare questo disco, ad esempio Microsoft Desktop Image Service Manager (DISM), oppure configurare manualmente una macchina virtuale con un VHDX vuoto e installare il sistema operativo su tale disco. Quando si configura il disco, è necessario rispettare i requisiti seguenti specifici per le macchine virtuali di seconda generazione e/o schermate: 

| Requisito per VHDX | Motivo |
|-----------|----|
|Deve essere un disco della tabella di partizione GUID (GPT) | Necessario per le macchine virtuali di seconda generazione per supportare UEFI|
|Il tipo di disco deve essere di **base** anziché **dinamico**. <br>Nota: si riferisce al tipo di disco logico, non alla funzionalità VHDX "ad espansione dinamica" supportata da Hyper-V. | BitLocker non supporta i dischi dinamici.|
|Il disco contiene almeno due partizioni. Una partizione deve includere l'unità in cui è installato Windows. Si tratta dell'unità che verrà crittografata da BitLocker. L'altra partizione è la partizione attiva, che contiene il bootloader e rimane non crittografata, in modo che il computer possa essere avviato.|Necessaria per BitLocker|
|File system NTFS | Necessaria per BitLocker|
|Il sistema operativo installato in VHDX è uno dei seguenti:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Necessaria per supportare le macchine virtuali di seconda generazione e il modello di avvio protetto Microsoft|
|Il sistema operativo deve essere generalizzato (eseguire Sysprep. exe) | Il provisioning dei modelli implica la specializzazione di macchine virtuali per il carico di lavoro di un tenant specifico| 

> [!NOTE]
> Non copiare il disco modello nella libreria VMM in questa fase. 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Pacchetti necessari per la creazione di un disco modello di nano server

Se si prevede di eseguire nano server come sistema operativo guest in macchine virtuali schermate, è necessario assicurarsi che l'immagine di nano Server includa i pacchetti seguenti:

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Eseguire Windows Update sul sistema operativo del modello

Sul disco del modello, verificare che nel sistema operativo siano installati tutti gli aggiornamenti di Windows più recenti. Gli aggiornamenti rilasciati di recente migliorano l'affidabilità del processo di schermatura end-to-end, ovvero un processo che potrebbe non essere completato se il sistema operativo del modello non è aggiornato.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Firmare e proteggere VHDX con la creazione guidata disco modello

Per usare un disco modello con VM schermate, il disco deve essere firmato e crittografato con BitLocker. A tale scopo, si utilizzerà la creazione guidata disco modello schermato. Questa procedura guidata consente di generare un hash per il disco e di aggiungerlo a un catalogo delle firme del volume (VSC). Il VSC viene firmato usando un certificato specificato e viene usato durante il processo di provisioning per garantire che il disco distribuito per un tenant non sia stato modificato o sostituito con un disco che il tenant non considera attendibile. Infine, BitLocker viene installato sul sistema operativo del disco, se non è già presente, per preparare il disco per la crittografia durante il provisioning della macchina virtuale.

> [!NOTE]
> La creazione guidata disco modello modificherà il disco del modello specificato sul posto. È possibile creare una copia del VHDX non protetto prima di eseguire la procedura guidata per eseguire aggiornamenti al disco in un secondo momento. Non sarà possibile modificare un disco protetto con la creazione guidata disco modello.

Eseguire i passaggi seguenti in un computer che esegue Windows Server 2016 (non deve essere un host sorvegliato o il server VMM):

1. Copiare il VHDX generalizzato creato in [preparare un VHDX del sistema operativo](#prepare-an-operating-system-vhdx) nel server, se non è già presente.

2. Installare la funzionalità **strumenti di macchine virtuali schermate** da **strumenti di amministrazione remota del server** nel computer.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Ottenere o creare un certificato per firmare il VHDX che diventerà il disco modello per le nuove macchine virtuali schermate. I dettagli relativi a questo certificato verranno incorporati in un file di dati di schermatura, che autorizza il disco come disco attendibile. Pertanto, è importante ottenere questo certificato da un'autorità di certificazione che l'utente e il provider di servizi di hosting considera attendibili. Negli scenari aziendali in cui si è sia il provider di hosting che il tenant, è possibile prendere in considerazione l'emissione di questo certificato dall'infrastruttura a chiave pubblica (PKI).

    Se si sta configurando un ambiente di testing e si vuole solo usare un certificato autofirmato per firmare il disco del modello, eseguire un comando simile al seguente nel computer:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Avviare la **creazione guidata disco modello** dalla cartella **strumenti di amministrazione** nel menu Start o digitando **TemplateDiskWizard. exe** in un prompt dei comandi.

5. Nella pagina **certificato** fare clic su **Sfoglia** per visualizzare un elenco di certificati. Selezionare il certificato con cui firmare il modello di disco. Fare clic su **OK** e quindi su **Avanti**.

6. Nella pagina disco virtuale fare clic su **Sfoglia** per selezionare il VHDX preparato, quindi fare clic su **Avanti**.

7. Nella pagina catalogo di firma specificare un nome e una versione del **disco** descrittivo **.** Questi campi sono presenti per facilitare l'identificazione del disco dopo che è stato firmato.

    Per il nome del **disco** , ad esempio, è possibile digitare _WS2016_ e per **Version**, _1.0.0.0_

8. Controllare le selezioni effettuate nella pagina Verifica impostazioni della procedura guidata. Quando si fa clic su **genera**, la procedura guidata Abilita BitLocker sul disco modello, calcola l'hash del disco e crea il catalogo delle firme dei volumi, che viene archiviato nei metadati VHDX.

    Attendere il completamento del processo di firma prima di provare a montare o spostare il disco modello. Il completamento di questo processo può richiedere del tempo, a seconda delle dimensioni del disco. 

9. Nella pagina **Riepilogo** vengono visualizzate le informazioni sul modello di disco, il certificato utilizzato per firmare il modello e l'autorità emittente del certificato. Fare clic su **Chiudi** per uscire dalla procedura guidata.


Fornire il modello di disco schermato al provider di servizi di hosting, insieme a un file di dati di schermatura creato, come descritto in [creazione di dati di schermatura per definire una macchina virtuale schermata](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="see-also"></a>Vedere anche

- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
