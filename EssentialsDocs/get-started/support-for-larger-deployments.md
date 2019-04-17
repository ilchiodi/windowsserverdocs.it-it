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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
#<a name="support-for-larger-deployments"></a>Supporto per distribuzioni estese

>Si applica a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Le funzionalità descritte in questo argomento funzionano solo in Windows Server 2016 con il ruolo esperienza Essentials abilitato e non con Windows Server 2016 Essentials SKU.


Windows Server Essentials supporta ora le distribuzioni di dimensioni maggiori con:

- più domini
- più controller di dominio
- possibilità di specificare un controller di dominio designato
- supporto per un massimo di 500 utenti e 500 dispositivi

##<a name="support-for-multiple-domains"></a>Supporto di più domini

Windows server 2012 R2 Essentials supporta un solo dominio per ogni server, è necessario, e il server Essentials deve essere la radice della foresta. Mentre un dominio e foresta sono ancora necessari, il ruolo esperienza Windows Server 2016 Essentials può essere distribuito in Windows Server 2016 Standard o Datacenter per supportare diversi domini.

## <a name="support-for-multiple-domain-controllers"></a>Supporto per più controller di dominio

 Windows Server Essentials 2012 R2 Blocca tutti i servizi che sfruttano Azure Active Directory, ad esempio Office 365, in cui viene distribuito più di un controller di dominio. Il motivo è che la sincronizzazione di account e password tra i controller di dominio locale e Azure Active Directory può comportare le credenziali dell'account con password che non sono sincronizzate. Questa limitazione è stata rimossa in Windows Server 2016 Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Possibilità di specificare un controller di dominio designato

È ora possibile scegliere un controller di dominio designato che verrà migliorare i tempi di recupero di oggetti di dominio Active Directory, nonché per coordinare altri controller di dominio nel dominio di sincronizzazione di modifica dell'account.

Controller di dominio designato predefinito sarà stesso server che esegue il ruolo del server esperienza Windows Server Essentials. Se il server è un server membro, vale a dire che non è un controller di dominio, il valore predefinito di controller di dominio designato sarà determinato automaticamente in base a test il controller di dominio nel dominio ha la minor latenza di rete al server che esegue il ruolo Server esperienza Windows Server. Se si desidera modificare manualmente il server è il controller di dominio designato, è possibile farlo nella **impostazioni** nel **dashboard di Windows Server Essentials** come illustrato di seguito.

![Screenshot che mostra le impostazioni del pannello in primo piano e il dashboard di Windows Server Essentials in background. La pagina di Controller di dominio designato le impostazioni del Pannello di controllo attualmente selezionata.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Supporto per 500 utenti e 500 dispositivi
-------------------------------------

Il numero massimo di utenti supportati e i dispositivi in Windows Server 2012 R2 Essentials è 25 e 50, rispettivamente. Con l'introduzione del ruolo del server esperienza Windows Server Essentials, tale limite è stato aumentato a 100 utenti e 200 dispositivi.

Windows Server 2016 Essentials supporta 500 utenti e 500 dispositivi. Questo possibili è un aggiornamento per il framework di provider e controlla l'elenco di oggetti in modo che nella cache e rapidamente il rendering di grandi dimensioni elenchi di oggetti utente e dispositivo. Inoltre, una funzionalità di ricerca e filtro è stato aggiunto per trovare rapidamente l'utente o il dispositivo che potrebbe essere cercando (vedi figura 5-2). Puoi trovare funzionalità di ricerca e filtro il **dashboard di Windows Server Essentials**, **accesso Web remoto**e l'archivio di Windows e Windows Phone **My Server** applicazioni.

![Screenshot che mostra l'uso della funzionalità di ricerca del dashboard di Windows Server Essentials per cercare la stringa "d5c". I risultati della ricerca includono due file e cartelle e due utenti.](media/larger-deployments-2.PNG)

Screenshot che mostra l'uso della funzionalità di ricerca del dashboard di Windows Server Essentials per cercare la stringa "d5c". I risultati della ricerca includono due file e cartelle e due utenti.

> [!NOTE]  
> Mentre per ruolo del Server Windows Server Essentials, il limite supportato di rimarrà backup client a 75 è aumentato il limite di utenti e dispositivi supportato.

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)