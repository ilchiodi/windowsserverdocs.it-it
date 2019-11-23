---
title: VM schermate - Il provider di servizi di hosting configura Windows Azure Pack
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d388da2b7416543c307bd931636902b4a7543e1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403654"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>VM schermate - Il provider di servizi di hosting configura Windows Azure Pack

Questo argomento descrive come un provider di servizi di hosting può configurare Windows Azure Pack in modo che i tenant possano usarlo per distribuire macchine virtuali schermate. Windows Azure Pack è un portale Web che estende la funzionalità di System Center Virtual Machine Manager per consentire ai tenant di distribuire e gestire le proprie macchine virtuali tramite una semplice interfaccia Web. Windows Azure Pack supporta completamente le VM schermate e rende ancora più semplice per i tenant creare e gestire i file di dati di schermatura.

Per comprendere il modo in cui questo argomento si integra nel processo generale di distribuzione delle macchine virtuali schermate, vedere la [procedura di configurazione del provider di servizi di hosting per gli host sorvegliati e le VM schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configurazione di Windows Azure Pack

Per configurare Windows Azure Pack nell'ambiente in uso, è necessario completare le attività seguenti:

1. Completare la configurazione di System Center 2016-Virtual Machine Manager (VMM) per l'infrastruttura di hosting. Ciò include la configurazione di modelli di VM e un Cloud VM, che verranno esposti tramite Windows Azure Pack:

    [Scenario: distribuire host sorvegliati e macchine virtuali schermate in VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Installare e configurare System Center 2016-Service Provider Foundation (SPF). Questo software consente Windows Azure Pack di comunicare con i server VMM:

    [Distribuzione di Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Installare Windows Azure Pack e configurarlo per la comunicazione con SPF:

    - [Installa Windows Azure Pack](#install-windows-azure-pack) (in questo argomento)
    - [Configurare Windows Azure Pack](#configure-windows-azure-pack) (in questo argomento)

4. Creare uno o più piani di hosting in Windows Azure Pack per consentire ai tenant di accedere ai Cloud VM:

    [Creazione di un piano in Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (in questo argomento)

## <a name="install-windows-azure-pack"></a>Installa Windows Azure Pack

Installare e configurare Windows Azure Pack (WAP) nel computer in cui si vuole ospitare il portale Web per i tenant. Il computer deve essere in grado di raggiungere il server SPF ed essere raggiungibile dai tenant.

1.  Revisione dei [requisiti di sistema WAP](https://technet.microsoft.com/library/dn296442.aspx) e installazione dei [prerequisiti software](https://technet.microsoft.com/library/dn469335.aspx).

2.  Scaricare e installare l' [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx). Se il computer non è connesso a Internet, seguire le [istruzioni per l'installazione offline](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Aprire l'installazione guidata piattaforma Web e trovare **Windows Azure Pack: portale e API Express** nella scheda **prodotti** . fare clic su **Aggiungi**e quindi su **Installa** nella parte inferiore della finestra.

4.  Proseguire con l'installazione. Al termine dell'installazione, nel Web browser verrà aperto il sito di configurazione (*https://&lt;wapserver&gt;: 30101/* ). In questo sito Web, fornire informazioni su SQL Server e completare la configurazione di WAP.

Per informazioni sulla configurazione di Windows Azure Pack, vedere [Install an Express Deployment of Windows Azure Pack](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Se è già stato eseguito Windows Azure Pack nell'ambiente in uso, è possibile usare l'installazione esistente. Per lavorare con le più recenti funzionalità di VM schermate, tuttavia, sarà necessario aggiornare l'installazione almeno all'aggiornamento cumulativo 10.

### <a name="configure-windows-azure-pack"></a>Configurare Windows Azure Pack

Prima di usare Windows Azure Pack, è necessario che sia già installato e configurato per l'infrastruttura.

1.  Passare al portale di amministrazione di Windows Azure Pack in *https://&lt;wapserver&gt;: 30091*, quindi accedere con le credenziali di amministratore.

2.  Nel riquadro sinistro fare clic su **Cloud VM**.

3.  Per connettersi Windows Azure Pack all'istanza di Service Provider Foundation, fare clic su **registra Service Provider Foundation di System Center**. Sarà necessario specificare l'URL per Service Provider Foundation, nonché un nome utente e una password.

    ![Registra Service Provider Foundation di System Center](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Al termine, dovrebbe essere possibile visualizzare i Cloud VM configurati nell'ambiente VMM. Prima di continuare, assicurarsi di avere almeno un Cloud VM che supporta le macchine virtuali schermate disponibili per WAP.

### <a name="create-a-plan-in-windows-azure-pack"></a>Creare un piano in Windows Azure Pack

Per consentire ai tenant di creare macchine virtuali in WAP, è innanzitutto necessario creare un piano di hosting a cui i tenant possono eseguire la sottoscrizione. I piani definiscono i Cloud VM consentiti, i modelli, le reti e le entità di fatturazione per i tenant.

1. Nel riquadro inferiore del portale fare clic su **+ nuovo** &gt; **piano** &gt; **crea piano**.

2. Nel primo passaggio della procedura guidata scegliere un nome per il piano. Si tratta del nome che verrà visualizzato dai tenant durante la sottoscrizione.

3. Nel secondo passaggio selezionare Cloud di **macchine virtuali** come uno dei servizi da offrire nel piano.

4. Ignorare il passaggio relativo alla selezione dei componenti aggiuntivi per il piano.

5. Fare clic su **OK** (segno di spunta) per creare il piano. Sebbene venga creato il piano, lo stato non è ancora configurato.

   ![Piani in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. Per iniziare a configurare il piano, fare clic sul relativo nome.

7. Nella pagina successiva, in **servizi del piano**, fare clic su **cloud macchine virtuali**. Verrà visualizzata la pagina in cui è possibile configurare le quote per questo piano.

8. In **Basic**selezionare il server di gestione VMM e il cloud di macchine virtuali che si vuole offrire ai tenant. I cloud che possono offrire VM schermate verranno visualizzati con **(schermatura supportata)** accanto al nome.

9. Selezionare le quote da applicare al piano. (Ad esempio, limiti per l'utilizzo di CPU e memoria Core). Assicurarsi di lasciare selezionata la casella di controllo **Consenti la schermatura delle macchine virtuali** .

   ![Impostazioni per i cloud di macchine virtuali in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. Scorrere verso il basso fino alla sezione **modelli**, quindi selezionare uno o più modelli da offrire ai tenant. È possibile offrire modelli schermati e non schermati ai tenant, ma è necessario che sia disponibile un modello schermato per concedere ai tenant garanzie end-to-end sull'integrità della VM e sui relativi segreti.

11. Nella sezione **reti** aggiungere una o più reti per i tenant.

12. Dopo aver impostato le altre impostazioni o quote per il piano, fare clic su **Salva** nella parte inferiore.

13. Nella parte superiore sinistra della schermata fare clic sulla freccia per tornare alla pagina del **piano** .

14. Nella parte inferiore della schermata modificare il piano da **privato** a **pubblico** in modo che i tenant possano sottoscrivere il piano.

    ![Modificare l'accesso per un piano in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    A questo punto, Windows Azure Pack è configurato e i tenant saranno in grado di sottoscrivere il piano appena creato e distribuire le macchine virtuali schermate. Per ulteriori passaggi che i tenant devono completare, vedere [macchine virtuali schermate per i tenant-distribuzione di una macchina virtuale schermata usando Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Vedi anche

- [Procedura di configurazione del provider di servizi di hosting per host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
