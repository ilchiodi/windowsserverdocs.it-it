---
title: Installare WEB1 il Server Web
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e818eac49719b394a2c73cc125a2e7ba9ea80c82
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-web-server-web1"></a>Installare WEB1 il Server Web

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Il ruolo Server Web (IIS) in Windows Server 2016 offre una piattaforma sicura, facile da gestire, modulare ed estensibile per l'hosting affidabile di siti Web, servizi e applicazioni. Con IIS, è possibile condividere informazioni con utenti su Internet, intranet o extranet. IIS è una piattaforma web unificata che integra IIS, ASP.NET, servizi FTP, PHP e Windows Communication Foundation (WCF).  

Quando si distribuiscono i certificati del server, server Web offre una posizione in cui è possibile pubblicare l'elenco di revoche di certificati (CRL) per l'autorità di certificazione (CA). Dopo la pubblicazione, il CRL è accessibile a tutti i computer della rete in modo da poter usare questo elenco durante il processo di autenticazione per verificare che i certificati presentati da altri computer non vengono revocati.   

Se un certificato nel CRL come revocato, l'impegno di autenticazione non riesce e il computer è protetto da trusting un'entità che dispone di un certificato che non è più valido.  

Prima di installare il ruolo Server Web (IIS), assicurarsi di aver configurato il nome del server e l'indirizzo IP e sono stati aggiunti computer al dominio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Per installare il ruolo server Server Web (IIS)  
Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.  

>[!NOTE]  
>Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell, digitare il comando seguente e quindi premere INVIO.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.  
2.  In **prima di iniziare**, fare clic su **Avanti**.  

**Nota**   
Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzata se è stato eseguito in precedenza l'aggiunta guidata ruoli e funzionalità e selezionati **Ignora questa pagina per impostazione predefinita** in quel momento.  

3.  Nel **tipo di installazione** pagina, fare clic su **Avanti**.  
4.  Nel **selezione dei Server** pagina, fare clic su **Avanti**.  
5.  Nel **ruoli Server** pagina, selezionare **Server Web (IIS)**, quindi fare clic su **Avanti**.  
6.  Fare clic su **Avanti** fino a quando non è stata accettata tutte l'impostazione predefinita le impostazioni del server web e quindi fare clic su **installare**.  
7.  Verificare che tutte le installazioni hanno avuto esito positivo, quindi fare clic su **Chiudi**.
