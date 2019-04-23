---
title: Installare la funzionalità BranchCache e configurare il server della cache ospitata in base al punto di connessione del servizio
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6619b09df0d4c161148d22091337a5039c7ea3af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849652"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Installare la funzionalità BranchCache e configurare il server della cache ospitata in base al punto di connessione del servizio

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per installare la funzionalità BranchCache nel server cache ospitata, HCS1 e per configurare il server per registrare un punto di connessione del servizio \(SCP\) in servizi di dominio Active Directory \(AD DS\).

Quando si registra server cache ospitata con un SCP in Active Directory Domain Services, SCP consente ai computer client che sono configurati correttamente per individuare automaticamente i server cache ospitata eseguendo una query Active Directory Domain Services per SCP. Più avanti in questa guida vengono fornite istruzioni su come configurare i computer client per eseguire questa azione.

>[!IMPORTANT]
>Prima di eseguire questa procedura, è necessario aggiungere il computer al dominio e configurare il computer con un indirizzo IP statico.

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Per installare la funzionalità BranchCache e configurare il server cache ospitata  

1. Nel computer del server, eseguire Windows PowerShell come amministratore. Digitare il comando seguente e quindi premere INVIO.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Per configurare il computer come server cache ospitata, dopo aver installata la funzionalità BranchCache e per registrare un punto di connessione del servizio in Active Directory Domain Services, digitare il comando seguente in Windows PowerShell e quindi premere INVIO.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Per verificare la configurazione del server cache ospitata, digitare il comando seguente e premere INVIO.

    ```  
    Get-BCStatus  
    ```  
  
    I risultati del comando vengono visualizzati lo stato per tutti gli aspetti dell'installazione di BranchCache. Di seguito sono alcune delle impostazioni di BranchCache e il valore corretto per ogni elemento:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Per preparare il passaggio di copia dei pacchetti di dati dai server al server cache ospitata, identificare una condivisione esistente nel server cache ospitata o creare una nuova cartella e condividere la cartella in modo che sia accessibile dai server. Dopo aver creato i pacchetti di dati sui server di contenuti, verranno copiati i pacchetti di dati in questa cartella condivisa sul server cache ospitata.
  
5. Se si distribuiscono più server cache ospitata, ripetere questa procedura su ogni server.

Per continuare con questa Guida, vedere [spostare e ridimensionare la Cache ospitata & #40; facoltativo & #41;](6-Bc-Move-Resize-Cache.md).
