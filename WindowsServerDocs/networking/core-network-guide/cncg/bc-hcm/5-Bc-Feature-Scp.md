---
title: Installare la funzionalità BranchCache e configurare il Server Cache ospitata dal punto di connessione servizio
description: Questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata in computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 854ff9f80a2221a857fab4e6ea7f7c8e6d51f1ef
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Installare la funzionalità BranchCache e configurare il Server Cache ospitata dal punto di connessione servizio

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per installare la funzionalità BranchCache nel server cache ospitata, HCS1 e per configurare il server per registrare un punto di connessione del servizio in servizi di dominio Active Directory \(AD DS\) \(SCP\).

Quando si registra server cache ospitata con un punto di dominio Active Directory, SCP consente ai computer client che sono configurati correttamente per individuare automaticamente i server cache ospitata eseguendo una query di dominio Active Directory per SCP. Più avanti in questa guida vengono fornite istruzioni su come configurare i computer client per eseguire questa azione.

>[!IMPORTANT]
>Prima di eseguire questa procedura, è necessario aggiungere il computer al dominio e configurare il computer con un indirizzo IP statico.

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Per installare la funzionalità BranchCache e configurare il server cache ospitata  

1. Nel computer del server, eseguire Windows PowerShell come amministratore. Digitare il comando seguente e quindi premere INVIO.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Per configurare il computer come server cache ospitata, dopo aver installata la funzionalità BranchCache e per registrare un punto di connessione del servizio di dominio Active Directory, digitare il comando seguente in Windows PowerShell e quindi premere INVIO.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Per verificare la configurazione del server cache ospitata, digitare il comando seguente e premere INVIO.

    ```  
    Get-BCStatus  
    ```  
  
    I risultati del comando visualizzano lo stato per tutti gli aspetti dell'installazione di BranchCache. Di seguito sono alcune le impostazioni di BranchCache e il valore corretto per ogni elemento:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Per prepararsi per il passaggio di copiare i pacchetti di dati dai server di contenuti per i server cache ospitata, identificare una condivisione esistente nel server cache ospitata o creare una nuova cartella e condividere la cartella in modo che sia accessibile dai server. Dopo aver creato i pacchetti di dati sui server di contenuti, copiare i pacchetti di dati in questa cartella condivisa sul server cache ospitata.
  
5. Se si distribuisce uno o più server cache ospitata, ripetere questa procedura su ogni server.

Per continuare con questa Guida, vedere [spostare e ridimensionare la Cache ospitata & #40; facoltativo & #41; ](6-Bc-Move-Resize-Cache.md).
