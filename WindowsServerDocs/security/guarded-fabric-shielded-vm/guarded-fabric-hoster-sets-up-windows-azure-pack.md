---
title: VM schermate - Il provider di servizi di hosting configura Windows Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9ca9f41770c977b6e7c4900b090471dbfe11a450
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855722"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>VM schermate - Il provider di servizi di hosting configura Windows Azure Pack

Questo argomento descrive come un provider di servizi possibile configurare Windows Azure Pack in modo che i tenant possono usare per distribuire le macchine virtuali schermate. Windows Azure Pack è un portale web che estende le funzionalità di System Center Virtual Machine Manager per consentire ai tenant da distribuire e gestire le proprie macchine virtuali tramite una semplice interfaccia web. Windows Azure Pack interamente supporta le macchine virtuali schermate e rende ancora più semplice per i tenant creare e gestire i file di dati di schermatura.

Per comprendere come in questo argomento si integra nel processo complessivo della distribuzione di macchine virtuali schermate, vedere [passaggi di configurazione di provider di servizi di Hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configurazione di Windows Azure Pack

Si completeranno le attività seguenti per configurare Windows Azure Pack nell'ambiente in uso:

1. Completare la configurazione di System Center 2016 - Virtual Machine Manager (VMM) per l'infrastruttura di hosting. È inoltre necessario impostare i modelli di macchina virtuale e un cloud VM, che verrà esposta tramite Windows Azure Pack:

    [Scenario: distribuire host sorvegliati e macchine virtuali schermate in VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Installare e configurare System Center 2016 - Service Provider Foundation (SPF). Questo software consente a Windows Azure Pack per comunicare con i server VMM:

    [Distribuzione di Service Provider Foundation - SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Installare Windows Azure Pack e configurarlo in modo da comunicare con SPF:

    - [Installare Windows Azure Pack](#install-windows-azure-pack) (in questo argomento)
    - [Configurare Windows Azure Pack](#configure-windows-azure-pack) (in questo argomento)

4. Creare uno o più piani di hosting in Windows Azure Pack per consentire l'accesso ai cloud VM i tenant:

    [Creare un piano in Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (in questo argomento)

## <a name="install-windows-azure-pack"></a>Installare Windows Azure Pack

Installare e configurare Windows Azure Pack (WAP) nel computer in cui si vuole ospitare il portale web per i tenant. Questo computer dovrà essere in grado di raggiungere il server di SPF e deve essere raggiungibile per i tenant.

1.  Revisione [requisiti di sistema WAP](https://technet.microsoft.com/library/dn296442.aspx) e installare le [prerequisiti software](https://technet.microsoft.com/library/dn469335.aspx).

2.  Scaricare e installare il [instalace Webové Platformy](https://www.microsoft.com/web/downloads/platform.aspx). Se il computer non è connesso a Internet, seguire le [istruzioni di installazione offline](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Aprire l'installazione guidata piattaforma Web e trovare **Windows Azure Pack: Portale e API Express** sotto la **prodotti** scheda. Fare clic su **Add**, quindi **installare** nella parte inferiore della finestra.

4.  Proseguire con l'installazione. Al termine dell'installazione, il sito di configurazione (*https://&lt;wapserver&gt;: 30101 /*) apre nel browser web. In questo sito Web, forniscono informazioni sull'istanza di SQL server e completare la configurazione di WAP.

Per ulteriori informazioni sulla configurazione di Windows Azure Pack, vedere [installare una distribuzione con installazione rapida di Windows Azure Pack](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Se si esegue già Windows Azure Pack nell'ambiente in uso, è possibile usare l'installazione esistente. Per poter funzionare con le ultime funzionalità delle VM schermate, tuttavia, è necessario aggiornare l'installazione di almeno aggiornamento cumulativo 10.

### <a name="configure-windows-azure-pack"></a>Configurare Windows Azure Pack

Prima di utilizzare Windows Azure Pack, si dovrebbe già installato e configurato per l'infrastruttura.

1.  Passare al portale di amministrazione di Windows Azure Pack al *https://&lt;wapserver&gt;: 30091*e quindi accedere con le credenziali di amministratore.

2.  Nel riquadro sinistro, fare clic su **cloud VM**.

3.  Connettersi a Windows Azure Pack per l'istanza di Service Provider Foundation facendo **registra System Center Service Provider Foundation**. È necessario specificare l'URL per Service Provider Foundation, nonché un nome utente e password.

    ![Registra System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Al termine, sarà in grado di visualizzare i cloud VM impostato nell'ambiente VMM. Assicurarsi di che disporre di almeno un cloud della macchina virtuale che supporta macchine virtuali schermate di disponibili per WAP prima di continuare.

### <a name="create-a-plan-in-windows-azure-pack"></a>Creare un piano in Windows Azure Pack

Per consentire ai tenant di creare le VM in WAP, è innanzitutto necessario creare un piano di hosting a cui i tenant possano sottoscriversi. I piani di definiscono il cloud di macchine Virtuali consentiti, modelli, reti ed entità di fatturazione per i tenant.

1.  Nel riquadro inferiore del portale, fare clic su **+ nuovo** &gt; **piano** &gt; **crea piano**.

2.  Nel primo passaggio della procedura guidata, scegliere un nome per il piano. Si tratta del nome che di tenant verrà visualizzato quando la sottoscrizione.

3.  Nel secondo passaggio, selezionare **cloud di macchine VIRTUALI** come uno dei servizi da offrire nel piano.

4.  Ignorare il passaggio sulla selezione di tutti i componenti aggiuntivi per il piano.

5.  Fare clic su **OK** (segno di spunta) per creare il piano. Anche se si crea il piano, non è ancora presente in uno stato configurato.

    ![I piani in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6.  Per iniziare a configurare il piano, fare clic sul relativo nome.

7.  Nella pagina successiva, sotto **servizi del piano**, fare clic su **Virtual Machine Clouds**. Verrà visualizzata la pagina in cui è possibile configurare le quote per questo piano.

8.  Sotto **base**, selezionare il Server di gestione VMM e il Cloud di macchine virtuali che si desidera offrire ai tenant. Verranno visualizzati con i cloud che possono offrire le macchine virtuali schermate **(schermatura supportata)** accanto al nome.

9.  Selezionare le quote da applicare in questo piano. (Ad esempio, i limiti per core della CPU e utilizzo della RAM). Assicurarsi di lasciare il **consentire le macchine virtuali per essere schermata** casella di controllo selezionata.

    ![Impostazioni per il cloud di macchine virtuali in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10.  Scorrere fino alla sezione intitolata **modelli**e quindi selezionare uno o più modelli per offrire ai tenant. È possibile offrire entrambi schermate e i modelli schermati ai tenant, ma un modello schermato deve essere offerti per offrire garanzie di end-to-end sull'integrità della VM e i propri segreti tenant.

11.  Nel **reti** sezione, aggiungere una o più reti per i tenant.

12.  Dopo l'impostazione altri impostazioni o le quote per il piano, fare clic su **salvare** nella parte inferiore.

13.  Nella parte superiore sinistra della schermata, fare clic sulla freccia per consentono di eseguire il backup per il **pianificare** pagina.

14.  Nella parte inferiore della schermata, modificare il piano da in corso **privati** al **pubblico** in modo che i tenant possono sottoscrivere il piano.

    ![Modificare l'accesso per un piano in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    A questo punto, è configurato Windows Azure Pack e i tenant potranno sottoscrivere il piano che appena creato e distribuire VM schermate. Per altri passaggi che è necessario completare i tenant, vedere [schermato macchine virtuali per tenant - distribuzione di una macchina virtuale schermata usando Microsoft Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Vedere anche

- [Passaggi di configurazione di provider di servizi di hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
