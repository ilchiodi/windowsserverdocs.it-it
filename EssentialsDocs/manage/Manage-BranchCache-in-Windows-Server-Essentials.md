---
title: Gestire BranchCache in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bfdd66276e2ff62e7223c9e542a94781d707b99b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311349"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gestire BranchCache in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache consente di ottimizzare l'utilizzo di Internet, migliorare le prestazioni delle applicazioni in rete e ridurre il traffico sulla rete WAN quando il server Windows Server Essentials si trova in remoto dall'ufficio o quando i computer client connessione a un server locale usare risorse basate sul cloud, ad esempio le raccolte di SharePoint Online.  
  
 Con BranchCache abilitato, quando un computer client richiede contenuto da un server di Windows Server Essentials remoto, il contenuto viene memorizzato nella cache dell'ufficio locale. In seguito, altri computer nello stesso ufficio potranno ottenere il contenuto localmente invece di scaricarlo di nuovo dal server sulla rete WAN. Questo comportamento può migliorare le prestazioni delle applicazioni in rete e ridurre il consumo di larghezza di banda sula rete WAN.  
  
 Se il server di Windows Server Essentials è locale o remoto, BranchCache può migliorare i tempi di risposta per le cartelle condivise del server e il contenuto Web ospitato nel server, ad esempio le raccolte di SharePoint Online.  
  
 Dato che BranchCache non richiede nuovo hardware o modifiche alla topologia di rete, questa funzionalità offre un modo semplice per ottimizzare l'utilizzo della larghezza di banda e migliorare i tempi di risposta per i servizi e le risorse a cui si accede sulla rete WAN.  
  
## <a name="branchcache-scenarios"></a>Scenari di BranchCache  
 Gli scenari di base per l'uso di BranchCache con un server remoto sono tre:  
  
-   Il server di Windows Server Essentials è ospitato in Microsoft Azure.  
  
-   Il server di Windows Server Essentials è ospitato nel Data Center di un provider di servizi di terze parti.  
  
-   Il server di Windows Server Essentials si trova in un altro ufficio in una posizione fisica diversa.  
  
## <a name="distributed-cache-mode"></a>Modalità cache distribuita  
 In Windows Server Essentials, BranchCache viene implementato in *modalità cache distribuita*, una delle due modalità di memorizzazione nella cache disponibili in BranchCache. In modalità cache distribuita, la cache di contenuti presso nella succursale viene distribuita tra i computer client. Dato che non è richiesto hardware aggiuntivo o modifiche della topologia, questa modalità è adatta per piccoli uffici che usano un server remoto o un server locale per accedere ai servizi basati sul cloud come SharePoint Online. Quando si attiva BranchCache in Windows Server Essentials, viene implementata la modalità cache distribuita.  
  
> [!NOTE]
>  Nelle succursali più grandi con più subnet o con un numero elevato di dipendenti che usano applicazioni di rete, può essere utile implementare BranchCache in *modalità cache ospitata*. In modalità cache ospitata la cache di contenuti è archiviata in uno o più server di cache ospitata presso la succursale.
  
## <a name="requirements"></a>Requisiti  
 Per usare BranchCache in Windows Server Essentials, i computer server e client devono soddisfare i requisiti seguenti:  
  
-   Il server deve eseguire il sistema operativo Windows Server Essentials o il sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials.  
  
     In Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter Server, BranchCache viene aggiunto quando si aggiunge il ruolo esperienza Windows Server Essentials. Per attivare BranchCache, è necessario accedere al dashboard di Windows Server Essentials con le credenziali di amministratore di dominio.  
  
-   Nei computer client deve essere in esecuzione il sistema operativo Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise o Windows 8.1 Enterprise.  
  
-   In modalità cache distribuita tutti i computer client devono trovarsi nella stessa subnet.  
  
    > [!NOTE]
    >  Se i computer client sono connessi al server Windows Server Essentials senza essere stati aggiunti al dominio, saranno esclusi dalla memorizzazione nella cache per impostazione predefinita. Per includere un computer client che non è stato aggiunto al dominio per la memorizzazione nella cache, eseguire il cmdlet di Windows PowerShell [Enable BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) nel computer client. Per altre informazioni, vedere la pagina relativa ai [cmdlet BranchCache in Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Attivare BranchCache  
 Per attivare BranchCache in modalità cache distribuita, è sufficiente fare clic su un pulsante nel dashboard di Windows Server Essentials. La memorizzazione nella cache inizia immediatamente e viene eseguita in modo trasparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Per attivare BranchCache in Windows Server Essentials  
  
1.  Accedere al server Windows Server Essentials con l'account amministratore.  
  
2.  Nel dashboard di Windows Server Essentials fare clic su **Impostazioni**.  
  
     Verrà aperta la procedura guidata impostazioni.  
  
3.  Fare clic su **BranchCache**.  
  
4.  Nella pagina **Impostazioni BranchCache** fare clic su **Attiva**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Usare Windows PowerShell per attivare o disattivare la BranchCache  
 È possibile usare Windows PowerShell per controllare lo stato di BranchCache (Abilitato o Disabilitato) e per attivare o disattivare BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Per attivare o disattivare BranchCache con Windows PowerShell  
  
1.  Sul server aprire Windows PowerShell come amministratore. Nella pagina **Start** fare clic con il pulsante destro del mouse su **Windows PowerShell**, fare clic su **Esegui come amministratore** e quindi su **Sì**.  
  
2.  Al prompt dei comandi immettere uno dei cmdlet seguenti.  
  
    -   Per controllare lo stato di BranchCache (Abilitato o Disabilitato), immettere:  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   Per attivare BranchCache, immettere:  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   Per disattivare BranchCache, immettere:  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>Vedere anche  
    
-   [Panoramica di BranchCache](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
