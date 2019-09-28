---
title: Installare il server Web WEB1
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ad8106c9c8330dd1b8632b3672d6413c1a1faaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356214"
---
# <a name="install-the-web-server-web1"></a>Installare il server Web WEB1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il ruolo server Web (IIS) in Windows Server 2016 offre una piattaforma protetta, semplice da gestire, modulare ed estensibile per l'hosting affidabile di siti Web, servizi e applicazioni. Con IIS, è possibile condividere informazioni con utenti su Internet, intranet o extranet. IIS è una piattaforma web unificata che integra IIS, ASP.NET, servizi FTP, PHP e Windows Communication Foundation (WCF).  

Quando si distribuiscono i certificati del server, il server Web fornisce un percorso in cui è possibile pubblicare l'elenco di revoche di certificati (CRL) per l'autorità di certificazione (CA). Dopo la pubblicazione, il CRL è accessibile a tutti i computer della rete, in modo che possano utilizzare questo elenco durante il processo di autenticazione per verificare che i certificati presentati da altri computer non vengano revocati.   

Se un certificato si trova nel CRL come revocato, il tentativo di autenticazione non riesce e il computer è protetto da un'entità che dispone di un certificato che non è più valido.  

Prima di installare il ruolo server Web (IIS), verificare di aver configurato il nome del server e l'indirizzo IP e che il computer sia stato aggiunto al dominio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Per installare il ruolo server Server Web (IIS)  
Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.  

>[!NOTE]  
>Per eseguire questa procedura usando Windows PowerShell, aprire PowerShell, digitare il comando seguente e quindi premere INVIO.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.  
2.  In **Prima di iniziare** fare clic su **Avanti**.  

**Nota**   
La pagina **prima di iniziare** dell'aggiunta guidata ruoli e funzionalità non viene visualizzata se in precedenza è stata eseguita l'aggiunta guidata ruoli e funzionalità e si è selezionato **Ignora questa pagina per impostazione predefinita** in quel momento.  

3. Nella pagina **Tipo di installazione** fare clic su **Avanti**.  
4. Nella pagina **Selezione server** fare clic su **Avanti**.  
5. Nella pagina **ruoli server** selezionare **server Web (IIS)** e quindi fare clic su **Avanti**.  
6. Fare clic su **Avanti** fino a quando non è stato accettato tutte l'impostazione predefinita le impostazioni del server web e quindi fare clic su **installare**.  
7. Verificare che tutte le installazioni hanno avuto esito positivo e quindi fare clic su **Chiudi**.
