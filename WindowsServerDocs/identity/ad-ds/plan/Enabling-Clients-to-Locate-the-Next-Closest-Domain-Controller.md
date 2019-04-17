---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: "Consentendo ai client di individuare il Controller di dominio più vicino successivo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39b1b79bba944c10b0c74c4bb18f6dcf80f8230e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Consentendo ai client di individuare il Controller di dominio più vicino successivo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si dispone di un controller di dominio che esegue Windows Server 2008 o Windows Server 2008 R2, è possibile renderlo possibili per i computer client che eseguono Windows Vista, Windows 7, Windows Server 2008 o Windows Server 2008 R2 per individuare i controller di dominio in modo più efficiente, consentendo il **prova sito più vicino successivo** impostazione criteri di gruppo. Questa impostazione consente di migliorare il localizzatore di Controller di dominio (DC Locator) quanto contribuiscono a semplificare il traffico di rete, soprattutto nelle grandi imprese che dispongono di molte succursali e siti.  
  
Questa nuova impostazione può influire sulle modalità di configurazione i costi di collegamento di sito poiché influisce l'ordine in cui si trovano i controller di dominio. Per le aziende con molti siti hub e succursali, è possibile ridurre notevolmente il traffico di Active Directory nella rete assicurando che i client eseguire il failover al sito hub più vicino successivo quando non riescono a trovare un controller di dominio nel sito hub più vicino.  
  
Come procedura consigliata generale, è necessario semplificare la topologia del sito e costi di collegamento di sito per quanto possibile se si abilita il **prova sito più vicino successivo** impostazione. Nelle aziende con molti siti hub, ciò consente di semplificare i piani che per la gestione delle situazioni in cui i client in un sito è necessario eseguire il failover a un controller di dominio in un altro sito.  
  
Per impostazione predefinita, il **prova sito più vicino successivo** impostazione non è abilitata. Quando l'impostazione non è abilitata, DC Locator utilizza l'algoritmo seguente per individuare un controller di dominio:  
  
-   Provare a trovare un controller di dominio nello stesso sito.  
  
-   Se nello stesso sito è disponibile alcun controller di dominio, provare a trovare qualsiasi controller di dominio nel dominio.  
  
> [!NOTE]  
> Questo è lo stesso algoritmo che DC Locator utilizzato nelle versioni precedenti di Active Directory. Per ulteriori informazioni, vedere come supporto DNS per Active Directory ([https://go.microsoft.com/fwlink/?LinkId=108587](https://go.microsoft.com/fwlink/?LinkId=108587)).  
  
Se si abilita il **prova sito più vicino successivo** impostazione DC Locator utilizza l'algoritmo seguente per individuare un controller di dominio:  
  
-   Provare a trovare un controller di dominio nello stesso sito.  
  
-   Se nello stesso sito è disponibile alcun controller di dominio, provare a trovare un controller di dominio nel sito più vicino successivo. Un sito è più vicino se dispone di un collegamento di sito basso costo rispetto a un altro sito con un collegamento di sito superiore dei costi.  
  
-   Se nel sito più vicino successivo è disponibile alcun controller di dominio, provare a trovare qualsiasi controller di dominio nel dominio.  
  
Per impostazione predefinita, DC Locator non considera tutti i siti che contiene un controller di dominio di sola lettura (RODC) quando si determina il successivo sito più vicino. Inoltre, poiché solo Windows Server 2008 e Windows Server 2008 R2 dominio controller supporto le funzionalità del sito più vicino successivo, quando il client riceve una risposta da un controller di dominio che esegue una versione precedente di Windows Server, il comportamento di DC Locator è identico quando l'impostazione non è abilitato.  
  
Ad esempio, si supponga che una topologia del sito disponga di quattro siti con i valori di collegamento di sito nella figura seguente. In questo esempio, tutti i controller di dominio sono controller di dominio scrivibili che eseguono Windows Server 2008 o Windows Server 2008 R2.  
  
![Consentendo ai client di individuare i controller di dominio](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)  
  
Quando il **prova sito più vicino successivo** impostazione di criteri di gruppo è abilitata in questo esempio, se un computer client che esegue Windows Vista, Windows 7, Windows Server 2008 o Windows Server 2008 R2 in Site_B tenta di individuare un controller di dominio, esso tenta prima di trovare un controller di dominio nel proprio Site_B. Se non è disponibile in Site_B, tenta di trovare un controller di dominio in Sito_A sarà.  
  
Se l'impostazione non è abilitata, il client tenta di trovare un controller di dominio in Sito_A sarà, Site_C o Site_D se è disponibile in Site_B alcun controller di dominio.  
  
> [!NOTE]  
> Il **prova sito più vicino successivo** impostazione funziona in coordinamento con la copertura automatica del sito. Ad esempio, se il sito più vicino successivo non dispone di alcun controller di dominio, DC Locator tenta di trovare il controller di dominio che esegue copertura automatica del sito per tale sito.  
  
Per applicare il **prova sito più vicino successivo** impostazione, è possibile creare un oggetto Criteri di gruppo (GPO) e collegarlo all'oggetto appropriato per l'organizzazione, oppure è possibile modificare il criterio di dominio predefinito in modo che influiscono su tutti i client che eseguono Windows Vista, Windows 7, Windows Server 2008 o Windows Server 2008 R2 nel dominio. Per ulteriori informazioni su come impostare il **prova sito più vicino successivo** impostazione, vedere [consentire ai client di individuare un Controller di dominio nel sito più vicino successivo](https://technet.microsoft.com/library/cc772592.aspx).  
  


