---
title: Installare e configurare Windows Server Essentials o esperienza Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8a2310b178663c6ca32a4e07d11656f1aaf2a11b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Installare e configurare Windows Server Essentials o esperienza Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server Essentials è un primo server ideale per le piccole imprese con un massimo di 25 utenti e 50 dispositivi. Per le organizzazioni con un massimo di 100 utenti e 200 dispositivi, è ora possibile utilizzare Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato. Questo argomento illustra entrambi gli scenari.  
  
Esperienza Windows Server Essentials è un ruolo in Windows Server 2016 che consente di sfruttare tutte le funzionalità (ad esempio, accesso Web remoto e backup dei PC) disponibili in Windows Server Essentials senza i relativi blocchi e limiti presenti in Windows Server Essentials. Questo ruolo del server è disponibile anche in Windows Server Essentials ed è abilitato per impostazione predefinita.
  
Prima di installare il ruolo esperienza Essentials o Windows Server Essentials, considerare le seguenti limitazioni.  
  
|Esperienza Windows Server Essentials in Windows Server Essentials|Esperienza Windows Server Essentials in Windows Server 2016
|----|----|
|-Deve essere il controller di dominio nella radice della foresta e del dominio e deve contenere tutti i ruoli FSMO.<br /><br /> -Impossibile installare in un ambiente con un dominio Active Directory preesistente (tuttavia, c'è un periodo di tolleranza di 21 giorni per eseguire le migrazioni).|-Non è necessario essere un controller di dominio se è installato in un ambiente con un dominio di Active Directory preesistente.<br /><br /> -Se non esiste un dominio Active Directory, l'installazione del ruolo creerà un dominio Active Directory e il server diventerà il controller di dominio nella radice della foresta e del dominio, che contiene tutti i ruoli FSMO.  
|Può essere distribuito solo in un singolo dominio.|Può essere distribuito solo in un singolo dominio.  
|Un controller di dominio di sola lettura non può esistere nel dominio.|Un controller di dominio di sola lettura non può esistere nel dominio.

> [!NOTE]
>  Per scaricare le versioni di valutazione dei sistemi operativi, visitare il sito TechNet Evaluation Center:  
>   
>  [Scaricare Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [È possibile scaricare Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Opzioni di installazione  
 Questo documento fornisce istruzioni dettagliate per l'installazione e configurazione di Windows Server Essentials. A seconda dell'ambiente di rete, sono disponibili le opzioni di installazione seguenti disponibili:  
  
-    Windows Server Essentials (con il ruolo esperienza Windows Server Essentials abilitato per impostazione predefinita)  
  
-    Windows Server 2016 con il ruolo esperienza Windows Server Essentials installato  
 
|Ambiente di distribuzione|Descrizione|Sezioni correlate|  
|----------------------------|-----------------|---------------------|  
|Nuovo ambiente Active Directory|È possibile installare Windows Server Essentials per creare un nuovo ambiente Active Directory.|[Distribuire Windows Server Essentials per configurare un nuovo ambiente Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Ambiente Active Directory esistente|È possibile installare Windows Server Essentials in un ambiente Active Directory esistente.|[Distribuire Windows Server Essentials in un ambiente Active Directory esistente](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Ambiente virtuale|È possibile distribuire Windows Server Essentials come macchina virtuale.|[Virtualizzare l'ambiente](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Distribuzione automatica|È possibile automatizzare la distribuzione di Windows Server Essentials tramite Windows PowerShell.|[Installare e configurare Windows Server Essentials tramite Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Prima di iniziare  
 Prima di iniziare l'installazione, consultare la documentazione seguente:  
  
-   [Panoramica del prodotto Windows Server Essentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a>Distribuire Windows Server Essentials per configurare un nuovo ambiente Active Directory  
 Windows Server Essentials consente di configurare rapidamente un ambiente Active Directory e le funzionalità server correlate.  
  
###  <a name="BKMK_WSEDeploy"></a>Distribuzione di Windows Server Essentials  
 Se si utilizza Windows Server Essentials, esperienza Windows Server Essentials è già abilitato. Tuttavia, è necessario completare alcuni passaggi per configurare il server.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Per configurare Windows Server Essentials in un server fisico  
  
1.  Dopo Windows **iniziale** pagina il **configurazione guidata di Windows Server Essentials** è visibile sul desktop.  
  
2.  Seguire le istruzioni per completare la procedura guidata, come indicato di seguito:  
  
    1.  Nel **configurare Windows Server Essentials** pagina, fare clic su **Avanti**.  
  
    2.  In **fuso orario**, verificare che la data, ora e fuso orario siano corretti e quindi fare clic su **Avanti**.  
  
    3.  In **informazioni sulla società**, digitare il nome della società, ad esempio **Contoso, Ltd.**, quindi fare clic su **Avanti**. Facoltativamente, è possibile modificare il nome di dominio interno e nome del server.  
  
    4.  In **creare amministratore di rete**, digitare un nome del nuovo account amministratore e una password.  
  
        > [!NOTE]
        >  Non utilizzare il valore predefinito **amministratore** nome account e password.  
  
    5.  Fare clic su **configurare**.  
  
3.  Il server verrà riavviato più volte durante il processo di configurazione e l'accesso verrà eseguito automaticamente fino al completamento della configurazione. Questo processo richiede circa 20 minuti.  
  
4.  Sul desktop, fare clic sull'icona del Dashboard per avviare il Dashboard del server. Nel **Home** completare il **Introduzione** attività elencate nel **installazione** scheda.  
  
 Dopo aver completato la configurazione del server, il server che esegue Windows Server Essentials verrà configurato come controller di dominio.  
  
###  <a name="BKMK_DeployWSERole"></a>Distribuzione del ruolo esperienza Windows Server Essentials in Windows Server 2012 R2 Standard e Datacenter  
 È possibile utilizzare Server Manager per abilitare e configurare il ruolo esperienza Windows Server Essentials in Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter utilizzando la procedura seguente.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Per distribuire il ruolo esperienza Windows Server Essentials in Windows Server 2012 R2  
  
1.  Accedere al server come amministratore locale.  
  
2.  Apri **Server Manager**, quindi fare clic su **Aggiungi ruoli e funzionalità**.  
  
3.  In **Selezione ruoli server**, selezionare il **esperienza Windows Server Essentials** ruolo. Nella finestra di dialogo, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
4.  In **funzionalità**, fare clic su **Avanti**.  
  
5.  Esaminare il **esperienza Windows Server Essentials** descrizione del ruolo, quindi fare clic su **Avanti**.  
  
6.  Nelle pagine successive fare clic su **Avanti**e quindi nella pagina di conferma, fare clic su **installare**.  
  
7.  Al termine dell'installazione, l'esperienza Windows Server Essentials dovrebbe essere elencato come ruolo del server in Server Manager.  
  
8.  Nell'area di notifica flag in Server Manager, fare clic sul contrassegno e quindi fare clic su **configurare Windows Server Essentials**.  
  
9. (Facoltativo) Se necessario, modificare il nome del server.  
  
    > [!IMPORTANT]
    >  Dopo aver configurato Windows Server Essentials, è possibile modificare il nome del server.  
  

10. Seguire la procedura guidata per configurare Windows Server Essentials come descritto in precedenza nel [la distribuzione di Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sezione.  

10. Seguire la procedura guidata per configurare Windows Server Essentials come descritto in precedenza nel [la distribuzione di Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sezione.  

  
##  <a name="BKMK_ExistingAD"></a>Distribuire Windows Server Essentials in un ambiente Active Directory esistente  
 Se l'organizzazione ha già un ambiente Active Directory esistente, è inoltre possibile distribuire Windows Server Essentials. Inoltre, è possibile scegliere se si desidera distribuire Windows Server Essentials come controller di dominio.  
  
> [!IMPORTANT]
>  Questa opzione è disponibile solo se si distribuisce il ruolo esperienza Windows Server Essentials in Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Per distribuire Windows Server Essentials in un ambiente Active Directory esistente  
  
1.  (Facoltativo) Se necessario, modificare il nome del server.  
  
    > [!IMPORTANT]
    >  Dopo aver configurato Windows Server Essentials, è possibile modificare il nome del server.  
  
2.  Aggiungere il server che esegue Windows Server Essentials a un dominio esistente come segue:  
  
    1.  Se si desidera che il server a un controller di dominio, configurare il server come controller di dominio di replica.  
  
    2.  Se non vuoi il server a controller di dominio, aggiungere il server al dominio utilizzando gli strumenti nativi di Windows.  
  
3.  Riavviare il server e accedere al server come amministratore di dominio.  
  
4.  Aprire Server Manager e quindi fare clic su **Aggiungi ruoli e funzionalità**.  
  
5.  Nelle pagine successive fare clic su **Avanti**.  
  
6.  In **Selezione ruoli Server**selezionare **esperienza Windows Server Essentials**. Nella finestra di dialogo, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
7.  In **funzionalità**, fare clic su **Avanti**.  
  
8.  Esaminare il **esperienza Windows Server Essentials** descrizione, quindi fare clic su **Avanti**.  
  
9. Nelle pagine successive fare clic su **Avanti**e quindi nella pagina di conferma, fare clic su **installare**.  
  
10. Al termine dell'installazione, esperienza Windows Server Essentials verrà elencato come ruolo del server in Server Manager.  
  
11. Nell'area di notifica flag in **Server Manager**, fare clic sul contrassegno e quindi fare clic su **configurare Windows Server Essentials**.  
  
12. Seguire la procedura guidata per configurare Windows Server Essentials. A seconda della configurazione di Active Directory, si verrà informati se si configura Windows Server Essentials in un controller di dominio o come membro del dominio. Fare clic su **configura** per avviare la configurazione. Il processo di configurazione richiede circa 10 minuti.  
  
##  <a name="BKMK_VirtualWSE"></a>Virtualizzare l'ambiente  
  Windows Server Essentials, Windows Server 2012 R2 Standard e Datacenter di Windows Server 2012 R2 può essere eseguito come macchine virtuali. Eseguire le macchine virtuali utilizzando gli strumenti di gestione di Hyper-V in un server che esegue Hyper-V. Gestione delle licenze, Windows Server Essentials consente di configurare il ruolo Hyper-V e virtualizzare l'ambiente. La licenza consente di impostare un altro sistema operativo guest che esegue Windows Server Essentials. A seconda del provider di sistema "Stato configurazione, Windows Server Essentials consente di configurare facilmente un ambiente virtualizzato.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Per distribuire Windows Server Essentials come macchina virtuale  
  
1.  Dopo la pagina iniziale di Windows (a seconda della "configurazione del sistema provider stato s), il **prima di iniziare** pagina offre la possibilità di configurare Windows Server Essentials come istanza virtuale o hardware fisico. La disponibilità di queste opzioni è predefinita dal provider di sistema ed entrambe le opzioni potrebbero non essere sempre disponibile. Per installare Windows Server Essentials come macchina virtuale, in **installare Windows Server Essentials**selezionare **installa come un'istanza virtuale**, quindi fare clic su **configura**.  
  
2.  La procedura guidata verrà eseguito il provisioning di una macchina virtuale che richiede circa cinque minuti.  
  

3.  Configurare quindi Windows Server Essentials come descritto in precedenza nel [la distribuzione di Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sezione.  

3.  Configurare quindi Windows Server Essentials come descritto in precedenza nel [la distribuzione di Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) sezione.  

  
##  <a name="BKMK_PowerShell"></a>Installare e configurare Windows Server Essentials tramite Windows PowerShell  
 È possibile automatizzare l'installazione di Windows Server Essentials tramite i cmdlet di Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Per installare Windows Server Essentials tramite Windows PowerShell  
  
1.  Aprire la console di Windows PowerShell da un prompt dei comandi con privilegi elevati.  
  
2.  Installare il ruolo esperienza Windows Server Essentials tramite il comando seguente:  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Eseguire `Get-Help Start-WssConfigurationService`.  
  
    1.  Eseguire il comando seguente per avviare la configurazione di Windows Server Essentials come controller di dominio:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Eseguire il comando seguente per avviare la configurazione di Windows Server Essentials come un membro di dominio esistente. È necessario essere un membro del gruppo Enterprise Admins e gruppo Domain Admins in Active Directory per eseguire questa operazione:  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Monitorare lo stato di avanzamento dell'installazione tramite i comandi seguenti:  
  
    -   Per lo stato visualizzato sulla barra di avanzamento dell'installazione, eseguire `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Per ottenere lo stato di avanzamento immediato senza l'indicatore di stato, eseguire `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [What's New in Windows Server Essentials](../get-started/what-s-new.md)  
  
-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Introduzione a Windows Server Essentials](../get-started/get-started.md)
