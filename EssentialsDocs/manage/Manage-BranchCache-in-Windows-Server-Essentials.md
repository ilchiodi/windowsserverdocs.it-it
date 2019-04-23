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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828802"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gestire BranchCache in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache consente di ottimizzare l'utilizzo di Internet, migliorare le prestazioni delle applicazioni di rete e ridurre il traffico di rete WAN (WAN) quando il server di Windows Server Essentials si trova in modalità remota dal tuo ufficio, o quando i computer client connettersi alle risorse di basata sul cloud Usa server locale, ad esempio le raccolte di SharePoint Online.  
  
 Con BranchCache abilitata, quando un computer client richiede contenuto da un server Windows Server Essentials remoto, il contenuto viene memorizzato nella cache nell'ufficio locale. In seguito, altri computer nello stesso ufficio potranno ottenere il contenuto localmente invece di scaricarlo di nuovo dal server sulla rete WAN. Questo comportamento può migliorare le prestazioni delle applicazioni in rete e ridurre il consumo di larghezza di banda sula rete WAN.  
  
 Se il server di Windows Server Essentials è locale o remoto, BranchCache può migliorare i tempi di risposta per le cartelle condivise di server e contenuto Web ospitato nel server (ad esempio le raccolte di SharePoint Online).  
  
 Dato che BranchCache non richiede nuovo hardware o modifiche alla topologia di rete, questa funzionalità offre un modo semplice per ottimizzare l'utilizzo della larghezza di banda e migliorare i tempi di risposta per i servizi e le risorse a cui si accede sulla rete WAN.  
  
## <a name="branchcache-scenarios"></a>Scenari di BranchCache  
 Gli scenari di base per l'uso di BranchCache con un server remoto sono tre:  
  
-   Il server Windows Server Essentials è ospitato in Microsoft Azure.  
  
-   Il server Windows Server Essentials è ospitato in Data Center di un provider di servizi di terze parti.  
  
-   Il server Windows Server Essentials è in un altro ufficio in una posizione fisica diversa.  
  
## <a name="distributed-cache-mode"></a>Modalità cache distribuita  
 In Windows Server Essentials, BranchCache è implementata nella *modalità cache distribuita*, una delle due modalità di cache di BranchCache. In modalità cache distribuita, la cache di contenuti presso nella succursale viene distribuita tra i computer client. Dato che non è richiesto hardware aggiuntivo o modifiche della topologia, questa modalità è adatta per piccoli uffici che usano un server remoto o un server locale per accedere ai servizi basati sul cloud come SharePoint Online. Quando si attiva BranchCache in Windows Server Essentials, viene implementata la modalità cache distribuita.  
  
> [!NOTE]
>  Nelle succursali più grandi con più subnet o con un numero elevato di dipendenti che usano applicazioni di rete, può essere utile implementare BranchCache in *modalità cache ospitata*. In modalità cache ospitata la cache di contenuti è archiviata in uno o più server di cache ospitata presso la succursale.
  
## <a name="requirements"></a>Requisiti  
 Per usare BranchCache in Windows Server Essentials, i computer client e il server deve soddisfare i requisiti seguenti:  
  
-   Il server deve essere in esecuzione il sistema operativo Windows Server Essentials o di Windows Server 2012 R2 Standard o sistema operativo Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials.  
  
     In un server Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, BranchCache viene aggiunta quando si aggiunge il ruolo esperienza Windows Server Essentials. Per attivare BranchCache, dovrai accedere al Dashboard di Windows Server Essentials con le credenziali di amministratore di dominio.  
  
-   I computer client devono essere in esecuzione di Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise o sistema operativo Windows 8.1 Enterprise.  
  
-   In modalità cache distribuita tutti i computer client devono trovarsi nella stessa subnet.  
  
    > [!NOTE]
    >  Se i computer client sono connessi al server Windows Server Essentials senza essere stati aggiunti al dominio, saranno esclusi dalla memorizzazione nella cache per impostazione predefinita. Per includere un computer client che non è stato aggiunto al dominio per la memorizzazione nella cache, eseguire il cmdlet di Windows PowerShell [Enable BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) nel computer client. Per altre informazioni, vedere la pagina relativa ai [cmdlet BranchCache in Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Attivare BranchCache  
 Per attivare BranchCache in modalità cache distribuita, sufficiente fare clic su un pulsante nel Dashboard di Windows Server Essentials. La memorizzazione nella cache inizia immediatamente e viene eseguita in modo trasparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Per attivare BranchCache in Windows Server Essentials  
  
1.  Accedi a server di Windows Server Essentials con l'account amministratore.  
  
2.  Nel Dashboard di Windows Server Essentials, fare clic su **impostazioni**.  
  
     Verrà aperta la procedura guidata impostazioni.  
  
3.  Fare clic su **BranchCache**.  
  
4.  Nella pagina **Impostazioni BranchCache** fare clic su **Attiva**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Usare Windows PowerShell per attivare o disattivare la BranchCache  
 È possibile usare Windows PowerShell per controllare lo stato di BranchCache (Abilitato o Disabilitato) e per attivare o disattivare BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Per attivare o disattivare BranchCache con Windows PowerShell  
  
1.  Sul server aprire Windows PowerShell come amministratore. Nella pagina **Start** fare clic con il pulsante destro del mouse su **Windows PowerShell**, fare clic su **Esegui come amministratore**e quindi su **Sì**.  
  
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
