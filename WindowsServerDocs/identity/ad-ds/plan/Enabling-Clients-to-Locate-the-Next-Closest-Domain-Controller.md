---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Consentire ai client di individuare il controller di dominio più vicino
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7550bdcea4e7b06d31463744bfdc3319c012c62c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880362"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Consentire ai client di individuare il controller di dominio più vicino

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si dispone di un controller di dominio che esegue Windows Server 2008 o versione successiva, è possibile apportare viene possibile per i computer client che eseguono Windows Vista o versioni successive o Windows Server 2008 o versione successiva per individuare i controller di dominio in modo più efficiente abilitando il **provare successivo Sito più vicino** impostazione criteri di gruppo. Questa impostazione consente di migliorare il localizzatore di Controller di dominio (DC Locator) in quanto contribuiscono a semplificare il traffico di rete, soprattutto nelle grandi imprese che dispongono di molte succursali e siti.

Questa nuova impostazione può influire sulle modalità di configurazione i costi di collegamento di sito poiché influisce l'ordine in cui si trovano i controller di dominio. Per le aziende che dispongono di molti siti hub e succursali, è possibile ridurre notevolmente il traffico di Active Directory nella rete assicurando che i client eseguire il failover al sito hub più vicino successivo quando non riescono a trovare un controller di dominio nel sito hub più vicino.

Come procedura consigliata generale, è necessario semplificare la topologia del sito e costi di collegamento di sito per quanto possibile, se si abilita il **prova sito più vicino successivo** impostazione. Nelle aziende con molti siti hub, ciò consente di semplificare i piani che per la gestione delle situazioni in cui i client in un sito è necessario eseguire il failover a un controller di dominio in un altro sito.

Per impostazione predefinita, il **prova sito più vicino successivo** non è abilitata. Quando l'impostazione non è abilitato, DC Locator utilizza l'algoritmo seguente per individuare un controller di dominio:

- Provare a trovare un controller di dominio nello stesso sito.
- Se nello stesso sito è disponibile alcun controller di dominio, provare a trovare qualsiasi controller di dominio nel dominio.

> [!NOTE]
> Questo è lo stesso algoritmo che DC Locator utilizzato nelle versioni precedenti di Active Directory. Per altre informazioni, vedere l'articolo [come supporto DNS per Active Directory](https://go.microsoft.com/fwlink/?LinkId=108587).

Se si abilita il **prova sito più vicino successivo** impostazione DC Locator utilizza l'algoritmo seguente per individuare un controller di dominio:

- Provare a trovare un controller di dominio nello stesso sito.
- Se nello stesso sito è disponibile alcun controller di dominio, provare a trovare un controller di dominio nel sito più vicino successivo. Un sito è più vicino se dispone di un collegamento di sito basso costo rispetto a un altro sito con un collegamento di sito superiore dei costi.
- Se nel sito più vicino successivo è disponibile alcun controller di dominio, provare a trovare qualsiasi controller di dominio nel dominio.

Il **prova sito più vicino successivo** impostazione funziona in coordinamento con la copertura automatica del sito. Ad esempio, se il sito più vicino successivo non dispone di alcun controller di dominio, DC Locator tenta di trovare il controller di dominio che esegue copertura automatica del sito per tale sito.

Per impostazione predefinita, DC Locator non considera tutti i siti che contiene un controller di dominio di sola lettura (RODC) quando si determina il successivo sito più vicino. Inoltre, quando il client riceve una risposta da un controller di dominio che esegue una versione precedente a Windows Server 2008, il comportamento di DC Locator è identico quando l'impostazione quindi non è abilitato.

Si supponga, ad esempio, una topologia costituita da quattro siti con i valori di collegamento di sito nella figura seguente. In questo esempio, tutti i controller di dominio sono controller di dominio scrivibili che eseguono Windows Server 2008 o versione successiva.

![consentendo ai client di individuare i controller di dominio](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Quando la **prova sito più vicino successivo** impostazione di criteri di gruppo è abilitata in questo esempio, se un computer client in Site_B tenta di individuare un controller di dominio, innanzitutto tenta di trovare un controller di dominio nel proprio Site_B. Se non è disponibile in Site_B, tenta di trovare un controller di dominio in Sito_A sarà.

Se l'impostazione non è abilitato, il client tenta di trovare un controller di dominio in Sito_A sarà, Site_C o Site_D se è disponibile in Site_B alcun controller di dominio.

> [!NOTE]
> Il **prova sito più vicino successivo** impostazione funziona in coordinamento con la copertura automatica del sito. Ad esempio, se il sito più vicino successivo non dispone di alcun controller di dominio, DC Locator tenta di trovare il controller di dominio che esegue copertura automatica del sito per tale sito.

Per applicare la **prova sito più vicino successivo** impostazione, è possibile creare un oggetto Criteri di gruppo (GPO) e collegarla all'oggetto appropriato per l'organizzazione oppure è possibile modificare il criterio dominio predefinito per fare in modo che influiscono su tutti i client che eseguono Windows Vista o versioni successive e Windows Server 2008 o versione successiva nel dominio. Per ulteriori informazioni su come impostare il **prova sito più vicino successivo** impostazione, vedere [consentire ai client di individuare un Controller di dominio nel sito più vicino successivo](https://technet.microsoft.com/library/cc772592.aspx).
