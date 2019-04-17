---
title: Gestire BranchCache in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 13d1d439eb9eeb60de9779d783e36405aee3ddfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gestire BranchCache in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache consente di ottimizzare l'utilizzo di Internet, migliorare le prestazioni delle applicazioni di rete e ridurre il traffico di rete WAN (WAN) quando il server Windows Server Essentials si trova in modalità remota rispetto all'ufficio o quando i computer client connessi a un server locale, utilizzare le risorse basate su cloud, ad esempio raccolte di SharePoint Online.  
  
 Con BranchCache abilitata, quando un computer client richiede contenuto da un server remoto di Windows Server Essentials, il contenuto viene memorizzato nella cache nell'ufficio locale. In seguito, altri computer nello stesso ufficio potranno ottenere il contenuto localmente invece di scaricarlo nuovamente il contenuto dal server tramite la rete WAN. Ciò può migliorare le prestazioni delle applicazioni di rete e riduce il consumo di larghezza di banda sulla rete WAN.  
  
 Se il server Windows Server Essentials è locali o remoti, BranchCache può migliorare i tempi di risposta per le cartelle server condiviso e il contenuto Web ospitato nel server (ad esempio raccolte di SharePoint Online).  
  
 Dato che BranchCache non richiede nuovo hardware o modifiche alla topologia di rete, questa funzionalità offre un modo semplice per ottimizzare l'utilizzo della larghezza di banda e migliorare i tempi di risposta per i servizi e le risorse di cui che si accede tramite la rete WAN.  
  
## <a name="branchcache-scenarios"></a>Scenari di BranchCache  
 Esistono tre scenari di base per l'uso di BranchCache con un server remoto:  
  
-   Il server Windows Server Essentials è ospitato in Microsoft Azure.  
  
-   Il server Windows Server Essentials è ospitato nel Data Center di un provider di servizi di terze parti.  
  
-   Il server Windows Server Essentials è in un altro ufficio in una posizione fisica diversa.  
  
## <a name="distributed-cache-mode"></a>Modalità cache distribuita  
 In Windows Server Essentials, BranchCache è implementata in *la modalità cache distribuita*, una delle due modalità di cache di BranchCache. In modalità cache distribuita, la cache di contenuti nella succursale viene distribuita tra i computer client. Perché non sono necessari ulteriori componenti hardware o modifiche alla topologia, questa modalità è adatta per piccoli uffici che utilizzano un server remoto o utilizzano un server locale per accedere ai servizi basati su cloud come SharePoint Online. Quando si attiva BranchCache in Windows Server Essentials, viene implementata la modalità cache distribuita.  
  
> [!NOTE]
>  In succursali di grandi dimensioni con più subnet o con un numero elevato di dipendenti che usano applicazioni di rete, può essere utile implementare BranchCache in *la modalità cache ospitata*. In modalità cache ospitata, la cache di contenuti è archiviata in uno o più server cache ospitata nella succursale.
  
## <a name="requirements"></a>Requisiti  
 Per usare BranchCache in Windows Server Essentials, il computer client e server devono soddisfare i requisiti seguenti:  
  
-   Il server deve eseguire il sistema operativo Windows Server Essentials o di Windows Server 2012 R2 Standard o sistema operativo Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials.  
  
     In un server Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, BranchCache viene aggiunto quando si aggiunge il ruolo esperienza Windows Server Essentials. Per attivare BranchCache, è necessario accedere al Dashboard di Windows Server Essentials con credenziali di amministratore di dominio.  
  
-   I computer client devono essere in esecuzione di Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise o sistema operativo Windows 8.1 Enterprise.  
  
-   In modalità cache distribuita, tutti i computer client devono trovarsi nella stessa subnet.  
  
    > [!NOTE]
    >  Se i computer client connesso al server Windows Server Essentials senza essere stati aggiunti al dominio, tali computer verranno esclusi dalla memorizzazione nella cache per impostazione predefinita. Per includere un computer client che non aggiunti al dominio in cui la memorizzazione nella cache, eseguire il [Enable BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) cmdlet di Windows PowerShell nel computer client. Per ulteriori informazioni, vedere [BranchCache Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Attivare BranchCache  
 Per attivare BranchCache in modalità cache distribuita, è sufficiente fare clic su un pulsante nel Dashboard di Windows Server Essentials. La memorizzazione nella cache inizia immediatamente e viene eseguita in modo trasparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Per attivare BranchCache in Windows Server Essentials  
  
1.  Accedere al server di Windows Server Essentials con il tuo account di amministratore.  
  
2.  Nel Dashboard di Windows Server Essentials, fare clic su **impostazioni**.  
  
     Verrà visualizzata la configurazione guidata impostazioni.  
  
3.  Fare clic su **BranchCache**.  
  
4.  Nel **le impostazioni di BranchCache** pagina, fare clic su **attiva**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Utilizzare Windows PowerShell per attivare o disattivare la BranchCache  
 È possibile utilizzare Windows PowerShell per controllare lo stato di BranchCache (abilitato o disabilitato) e per attivare o disattivare BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Per attivare o disattivare BranchCache con Windows PowerShell  
  
1.  Nel server, aprire Windows PowerShell come amministratore. Nel **Start** pagina, fare doppio clic su **Windows PowerShell**, fare clic su **Esegui come amministratore**, quindi fare clic su **Sì**.  
  
2.  Immettere uno dei cmdlet seguenti al prompt dei comandi.  
  
    -   Per controllare lo stato di BranchCache (abilitato o disabilitato), immettere:  
  
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
