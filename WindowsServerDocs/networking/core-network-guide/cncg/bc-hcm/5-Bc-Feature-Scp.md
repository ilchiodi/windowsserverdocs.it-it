---
title: Installare la funzionalità BranchCache e configurare il server della cache ospitata in base al punto di connessione del servizio
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 987b46b1748d5a889aa69823d3492a707948a100
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318471"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Installare la funzionalità BranchCache e configurare il server della cache ospitata in base al punto di connessione del servizio

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per installare la funzionalità BranchCache nel server cache ospitata, HCS1 e per configurare il server per registrare un punto di connessione del servizio \(SCP\) in servizi di dominio Active Directory \(AD DS\).

Quando si registrano server cache ospitata con SCP in servizi di dominio Active Directory, SCP consente ai computer client configurati correttamente di individuare automaticamente i server cache ospitata eseguendo una query su servizi di dominio Active Directory per SCP. Le istruzioni su come configurare i computer client per l'esecuzione di questa azione sono fornite più avanti in questa guida.

>[!IMPORTANT]
>Prima di eseguire questa procedura, è necessario aggiungere il computer al dominio e configurare il computer con un indirizzo IP statico.

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Per installare la funzionalità BranchCache e configurare il server cache ospitata  

1. Nel computer del server, eseguire Windows PowerShell come amministratore. Digitare il comando seguente e quindi premere INVIO.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Per configurare il computer come server cache ospitata dopo l'installazione della funzionalità BranchCache e per registrare un punto di connessione del servizio in servizi di dominio Active Directory, digitare il comando seguente in Windows PowerShell e quindi premere INVIO.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Per verificare la configurazione del server cache ospitata, digitare il comando seguente e premere INVIO.

    ```  
    Get-BCStatus  
    ```  
  
    I risultati dello stato di visualizzazione del comando per tutti gli aspetti dell'installazione di BranchCache. Di seguito sono riportate alcune delle impostazioni di BranchCache e il valore corretto per ogni elemento:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Per preparare il passaggio della copia dei pacchetti di dati dai server di contenuti ai server cache ospitata, identificare una condivisione esistente nel server cache ospitata o creare una nuova cartella e condividere la cartella in modo che sia accessibile dai server di contenuti. Dopo aver creato i pacchetti di dati nei server di contenuti, i pacchetti di dati vengono copiati in questa cartella condivisa nel server cache ospitata.
  
5. Se si distribuisce più di un server cache ospitata, ripetere questa procedura in ogni server.

Per continuare con questa Guida, vedere [spostare e ridimensionare la Cache ospitata & #40; facoltativo & #41;](6-Bc-Move-Resize-Cache.md).
