---
title: Installare il server Web WEB1
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15da16094a47a2492dc9054e0671c3709fe23362
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446450"
---
# <a name="install-the-web-server-web1"></a>Installare il server Web WEB1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Il ruolo Server Web (IIS) in Windows Server 2016 fornisce una piattaforma sicura, facile da gestire, modulare ed estensibile per l'hosting affidabile di siti Web, servizi e applicazioni. Con IIS, è possibile condividere informazioni con utenti su Internet, intranet o extranet. IIS è una piattaforma web unificata che integra IIS, ASP.NET, servizi FTP, PHP e Windows Communication Foundation (WCF).  

Quando si distribuiscono i certificati del server, il server Web offre una posizione in cui è possibile pubblicare l'elenco di revoche di certificati (CRL) per l'autorità di certificazione (CA). Dopo la pubblicazione, il CRL è accessibile a tutti i computer sulla rete in modo da poter usare questo elenco durante il processo di autenticazione per verificare che non vengono revocati i certificati presentati da altri computer.   

Se un certificato nel CRL come revocato, le operazioni che l'autenticazione ha esito negativo e il computer viene protetto da considerare attendibile un'entità che dispone di un certificato che non è più valido.  

Prima di installare il ruolo Server Web (IIS), assicurarsi di aver configurato il nome del server e l'indirizzo IP e avere aggiunto il computer al dominio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Per installare il ruolo server Server Web (IIS)  
Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.  

>[!NOTE]  
>Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell, digitare il comando seguente e quindi premere INVIO.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.  
2.  In **Prima di iniziare** fare clic su **Avanti**.  

**Nota**   
Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzata se è stato eseguito in precedenza l'aggiunta guidata ruoli e funzionalità sono stati selezionati **Ignora questa pagina per impostazione predefinita** in quel momento.  

3. Nella pagina **Tipo di installazione** fare clic su **Avanti**.  
4. Nel **selezione del Server** pagina, fare clic su **successivo**.  
5. Nel **ruoli predefiniti del Server** pagina, selezionare **Server Web (IIS)** e quindi fare clic su **Next**.  
6. Fare clic su **Avanti** fino a quando non è stato accettato tutte l'impostazione predefinita le impostazioni del server web e quindi fare clic su **installare**.  
7. Verificare che tutte le installazioni hanno avuto esito positivo e quindi fare clic su **Chiudi**.
