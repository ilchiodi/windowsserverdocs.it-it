---
title: Distribuire i server cache ospitata (facoltativo)
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f66e7e728f8e5ca657d002c3c5afcd2d9d23e9ff
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319158"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Distribuire i server cache ospitata (facoltativo)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per installare e configurare i server cache ospitata BranchCache che si trovano nelle filiali in cui si desidera distribuire la modalità cache ospitata di BranchCache. Con BranchCache in Windows Server 2016, è possibile distribuire più server cache ospitata in una succursale.  
  
> [!IMPORTANT]  
> Questo passaggio è facoltativo in quanto la modalità cache distribuita non richiede un server cache ospitata nelle succursali. Se non si intende distribuire cache ospitata in ogni succursale, non è necessario distribuire un server cache ospitata e non è necessario eseguire i passaggi di questa procedura.  
  
È necessario essere un membro di **amministratori**, o equivalente a eseguire questa procedura.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Per installare e configurare un server cache ospitata  
  
1.  Nel computer che si desidera configurare come server cache ospitata, eseguire il comando seguente al prompt di Windows PowerShell per installare la funzionalità BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configurare il computer come server cache ospitata utilizzando uno dei seguenti comandi:  
  
    -   Per configurare un computer aggiunto non di dominio come server cache ospitata, digitare il comando seguente al prompt di Windows PowerShell e quindi premere INVIO.  
  
        `Enable-BCHostedServer`  
  
    -   Per configurare un dominio aggiunto come server cache ospitata e per registrare un punto di connessione del servizio in Active Directory per l'individuazione di server cache ospitata automaticamente dai computer client, digitare il comando seguente al prompt di Windows PowerShell e quindi premere INVIO.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Per verificare la corretta configurazione del server cache ospitata, digitare il comando seguente al prompt di Windows PowerShell e quindi premere INVIO.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Dopo aver eseguito questo comando, nella sezione **HostedCacheServerConfiguration**, il valore per **HostedCacheServerIsEnabled** è **True**. Se è stato configurato un server cache ospitata unite in join di dominio per registrare un punto di connessione del servizio (SCP) in Active Directory, il valore per **HostedCacheScpRegistrationEnabled** è **True**.  
  

