---
title: Supporto per distribuzioni estese
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c60e5f73c88a225fbd1067992894f9d20da745ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860482"
---
#<a name="support-for-larger-deployments"></a>Supporto per distribuzioni estese

>Si applica a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Le funzionalità descritte in questo argomento funzionano solo in Windows Server 2016 con il ruolo esperienza Essentials abilitato e non con lo SKU di Windows Server 2016 Essentials.


Windows Server Essentials supporta ora le distribuzioni di grandi dimensioni con:

- più domini
- più controller di dominio
- possibilità di specificare un controller di dominio designata
- supporto per 500 dispositivi e fino a 500 utenti

##<a name="support-for-multiple-domains"></a>Supporto di più domini

Windows server 2012 R2 Essentials supporta solo un dominio per ogni server, che è necessario, e il server Essentials deve essere la radice della foresta. Anche se un dominio e foresta sono ancora necessari, il ruolo esperienza Windows Server 2016 Essentials può ora essere distribuito in Windows Server 2016 Standard o Datacenter per supportare più domini.

## <a name="support-for-multiple-domain-controllers"></a>Supporto per più controller di dominio

 Windows Server Essentials 2012 R2 consente di bloccare tutti i servizi che usano Azure Active Directory, ad esempio Office 365, in cui viene distribuito più di un controller di dominio. Il motivo è che la sincronizzazione di account e una password tra i controller di dominio locale e Azure Active Directory può causare le credenziali dell'account con password che non sono sincronizzate. Questa limitazione è stata rimossa in Windows Server 2016 Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Possibilità di specificare un controller di dominio designata

È ora possibile scegliere un controller di dominio designato che verrà migliorare i tempi di recupero di oggetti di dominio Active Directory, nonché per coordinare la sincronizzazione della modifica dell'account in altri controller di dominio nel dominio.

Predefinito definito Controller di dominio sarà il server stesso in cui è in esecuzione il ruolo server esperienza Windows Server Essentials. Se tale server è un server membro, vale a dire non è un controller di dominio e quindi il valore predefinito di controller di dominio designata verrà determinato automaticamente in base alla verifica controller di dominio nel dominio con la minore latenza di rete nel server che esegue il Ruolo Server esperienza Windows Server. Se si desidera modificare manualmente il server è il controller di dominio designato, è possibile eseguire questa operazione nel **le impostazioni** nel **dashboard di Windows Server Essentials** come illustrato di seguito.

![Screenshot che mostra le impostazioni di controllo pannello in primo piano e il dashboard di Windows Server Essentials in background. Il Controller di dominio designata le impostazioni del Pannello di controllo è attualmente selezionata.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Supporto per 500 dispositivi e 500 utenti
-------------------------------------

Il numero massimo di utenti supportati e i dispositivi in Windows Server 2012 R2 Essentials è 25 e 50 caratteri, rispettivamente. Con l'introduzione del ruolo del server esperienza Windows Server Essentials, tale limite è stato aumentato a 100 utenti e 200 dispositivi.

Windows Server 2016 Essentials supporta 500 dispositivi e 500 utenti. Rende questo possibile è un aggiornamento per il framework del provider e controlla l'elenco di oggetti in modo che memorizza nella cache e rapidamente il rendering di grandi dimensioni elenchi di oggetti utente e dispositivo. Inoltre, una funzionalità di ricerca e filtro è stato aggiunto per trovare rapidamente l'utente o il dispositivo che potrebbero cercare (vedere Figura 5-2). Sono disponibili funzionalità di ricerca e filtro il **dashboard di Windows Server Essentials**, **Remote Web Access**e l'archivio di Windows e Windows Phone **My Server** applicazioni.

![Screenshot che illustra l'uso della funzionalità di ricerca di dashboard di Windows Server Essentials per cercare la stringa "d5c." I risultati della ricerca includono due file e cartelle e due utenti.](media/larger-deployments-2.PNG)

Screenshot che illustra l'uso della funzionalità di ricerca di dashboard di Windows Server Essentials per cercare la stringa "d5c". I risultati della ricerca includono due file e cartelle e due utenti.

> [!NOTE]  
> Mentre è aumentato il limite di utenti e dispositivi supportato per il ruolo Server Windows Server Essentials, il limite supportato per mantiene backup client 75.

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)