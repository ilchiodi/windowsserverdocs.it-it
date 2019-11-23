---
title: Distribuire Cartelle di lavoro con AD FS e Panoramica del Proxy dell'applicazione Web
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 40cc953ce7393781497d957fc8e6690c5c9abc0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365918"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: Panoramica

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Gli argomenti in questa sezione forniscono istruzioni per la distribuzione di Cartelle di lavoro con Active Directory Federation Services (AD FS) e Proxy applicazione Web. Le istruzioni sono progettate per consentirti di creare una configurazione completa di Cartelle di lavoro che funzioni con i computer client pronti per iniziare ad usare Cartelle di lavoro in locale o su Internet.  
  
Cartelle di lavoro è un componente introdotta in Windows Server 2012 R2 che consente agli information worker di sincronizzare i file di lavoro tra i loro dispositivi. Per ulteriori informazioni su Cartelle di lavoro, vedi [Panoramica di Cartelle di lavoro](Work-Folders-Overview.md).  
  
Per consentire agli utenti di sincronizzare le loro istanze di Cartelle di lavoro tramite Internet, è necessario pubblicare Cartelle di lavoro tramite un proxy inverso per renderlo disponibile esternamente su Internet. Proxy dell'applicazione Web, incluso in AD FS, è un'opzione che puoi utilizzare per fornire funzionalità di proxy inverso. Proxy dell'applicazione Web preautentica l'accesso all'applicazione web di Cartelle di lavoro mediante AD FS, in modo tale che gli utenti, con qualsiasi dispositivo, possano accedere a Cartelle di lavoro dall'esterno della rete aziendale. 

> [!NOTE]
>   Le istruzioni descritte in questa sezione sono per un ambiente Windows Server 2016. Se usi Windows Server 2012 R2, segui le [istruzioni di Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).
  
Gli argomenti forniscono i seguenti dettagli:  
  
-   Istruzioni dettagliate per la configurazione e distribuzione di Cartelle di lavoro con AD FS e Proxy dell'applicazione Web tramite l'interfaccia utente di Windows Server. Le istruzioni illustrano come configurare un ambiente semplice di test con certificati autofirmati. È quindi possibile utilizzare l'esempio di test come una guida per la creazione di un ambiente di produzione che utilizza certificati pubblicamente attendibili.  
  
## <a name="prerequisites"></a>Prerequisiti  
Per seguire le procedure e gli esempi in questi argomenti, è necessario che i componenti seguenti siano pronti:  
  
-   Una foresta di AD DS con estensioni dello schema in Windows Server 2012 R2 per supportare il riferimento automatico di computer e dispositivi al file server corretto quando si usano più file server. È preferibile che DNS sia attivato nella foresta, ma non è obbligatorio.  
  
-   Un controller di dominio: un server che ha abilitato il ruolo di Active Directory Domain Services ed è configurato con un dominio (per l'esempio di test, contoso.com).  
  
    Un controller di dominio che esegua almeno Windows Server 2012 R2 è necessario per supportare la registrazione dei dispositivi per Workplace Join. Se non si desidera utilizzare Workplace Join, è possibile eseguire Windows Server 2012 nel controller di dominio.  
  
-   Due server che vengono aggiunti al dominio (ad esempio, contoso.com) e che eseguono Windows Server 2016. Un server verrà utilizzato per AD FS e l'altro per Cartelle di lavoro.  
  
-   Un server che non è stato aggiunto al dominio e che esegue Windows Server 2016. Il server eseguirà il Proxy dell'applicazione Web e deve disporre di una scheda di rete per il dominio di rete (ad esempio, contoso.com) e di un'altra scheda di rete per la rete esterna.  
  
-   Un computer client aggiunto a un dominio che esegue Windows 7 o versione successiva.  
  
-   Un computer client non aggiunto a un dominio che esegue Windows 7 o versione successiva.  
  
Per l'ambiente di test che illustriamo in questa guida, dovresti avere la topologia visualizzata nel seguente diagramma. I computer possono essere fisici o macchine virtuali. 
  
![Il diagramma che mostra i segmenti di rete Internet, della rete perimetrale e di Contoso. Nel segmento Internet: Client2; nella rete perimetrale: un server WAP. nel segmento Contoso: Server di Cartelle di lavoro, un controller di dominio, un server AD FS e Client1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>Cenni preliminari sulla distribuzione  
In questo gruppo di argomenti, verranno illustrati esempi dettagliati di configurazione di AD FS, Proxy applicazione Web e di Cartelle di lavoro in un ambiente di test. I componenti verranno configurati in questo ordine:  
  
1.  AD FS  
  
2.  Cartelle di lavoro  
  
3.  Proxy applicazione Web  
  
4.  La workstation appartenente al dominio e quella non appartenente al dominio  
  
Inoltre, userai uno script di Windows PowerShell per creare certificati autofirmati.  
  
## <a name="deployment-steps"></a>Fasi di distribuzione  
Per eseguire la distribuzione tramite l'interfaccia utente di Windows Server, segui i passaggi illustrati in questi argomenti:  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 1, configurazione AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 2 AD FS lavoro di post-configurazione](deploy-work-folders-adfs-step2.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 3, configurare cartelle di lavoro](deploy-work-folders-adfs-step3.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 4, configurare il proxy dell'applicazione Web](deploy-work-folders-adfs-step4.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 5, configurare i client](deploy-work-folders-adfs-step5.md)  

## <a name="see-also"></a>Vedi anche  
[Panoramica di cartelle di lavoro](Work-Folders-Overview.md)  
[Progettazione di un'implementazione di Cartelle di lavoro](Plan-Work-Folders.md)  
[Distribuzione di Cartelle di lavoro](Deploy-Work-Folders.md)  
  

