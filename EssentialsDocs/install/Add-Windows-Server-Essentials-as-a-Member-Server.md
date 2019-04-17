---
title: Aggiungere Windows Server Essentials come Server membro
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8fb73f8186d3984c9e93f7a6e39cb72a54db1e58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Aggiungere Windows Server Essentials come Server membro

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo argomento si applica a un server che esegue Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter o Windows Server 2016 con il ruolo esperienza Windows Server Essentials installato. Nella parte restante di questo documento, il ruolo esperienza Windows Server Essentials verrà considerato come Windows Server Essentials.  
  
> [!NOTE]
>   Windows Server Essentials può essere distribuito solo come controller di dominio. In questo documento, Windows Server Essentials non include Windows Server Essentials.  
  
 Windows Server Essentials non è necessario essere un server primario all'interno di un dominio di Windows. Puoi aggiungere Windows Server Essentials come server membro a un ambiente di dominio Active Directory esistente e sfruttare i vantaggi delle funzionalità di integrazione di cloud che offre, protezione dei dati semplice e accesso remoto sicuro. Inoltre, Windows Server Essentials può essere distribuito in un ambiente Active Directory esistente senza dover essere un controller di dominio. Ciò consente di estendere l'archiviazione o di usare una succursale per l'amministrazione e l'archiviazione locale.  
  
 È possibile aggiungere Windows Server Essentials negli scenari seguenti:  
  
-   Aggiungere Windows Server Essentials in una succursale e aggiungerlo al controller di dominio che si trova nella sede principale in un'ubicazione diversa usando gli strumenti nativi. È possibile attivare le funzionalità di BranchCache per l'utilizzo della larghezza di banda ottimale su questo server membro.  
  
-   Aggiungere Windows Server Essentials come server membro all'interno di una rete di Windows Server Essentials per estendere le risorse di archiviazione la rete aggiungendo altre cartelle server nel server membro.  
  
-   Aggiungere Windows Server Essentials come server membro nell'ufficio locale se il server primario che esegue Windows Server Essentials è ospitato in Microsoft Azure o da un provider di terze parti. Con Windows Server Essentials come server membro presso l'ufficio locale consente di ottimizzare l'utilizzo della larghezza di banda.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Aggiunta di Windows Server Essentials come server membro  
 Per aggiungere Windows Server Essentials come server membro a un server primario che esegue Windows Server 2012 R2 o Windows Server Essentials in un ambiente Active Directory esistente, è necessario completare i passaggi seguenti:  
  
1.  Aggiungere il server che esegue Windows Server Essentials a un gruppo di lavoro.  
  
2.  Aggiungere il server che esegue Windows Server Essentials al dominio di un server primario di Windows Server Essentials.  
  
3.  Configurare l'esperienza Windows Server Essentials da Server Manager.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Per aggiungere Windows Server Essentials a un dominio o gruppo di lavoro  
  
1.  Dopo aver completato l'installazione di Windows Server Essentials nel secondo server, chiuderla configurare Windows Server Essentials.  
  
2.  Nel **ricerca** digitare **le impostazioni di sistema**, nei risultati della ricerca, fare clic su **Visualizza impostazioni di sistema avanzate**.  
  
3.  In **proprietà del sistema**, fare clic su di **nome Computer** scheda.  
  
4.  In **nome Computer**, nel **dominio** fare clic su **modifica**.  
  
5.  In **cambiamenti dominio/nome Computer**, nel **membro** sezione, scegliere se si desidera aggiungere il server che esegue Windows Server Essentials per un **gruppo di lavoro** o a un **dominio**.  
  
    -   Per aggiungere il server a un gruppo di lavoro, digitare **gruppo di lavoro**, quindi fare clic su **OK**.  
  
    -   Per aggiungere il server a un dominio di Active Directory esistente, digitare il nome del dominio e quindi fare clic su **OK**.  
  
6.  Riavviare il server per rendere effettive le modifiche.  
  
 Dopo aver aggiunto il server al dominio s server primario, è possibile continuare a configurare Windows Server Essentials eseguendo guidata di configurare Windows Server Essentials da Server Manager.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Per configurare esperienza Windows Server Essentials in un server membro  
  
1.  (Facoltativo) Se necessario, modificare il nome del server.  
  
    > [!IMPORTANT]
    >  Dopo aver configurato l'esperienza Windows Server Essentials, è possibile modificare il nome del server.  
  
2.  Accedere al server usando l'account amministratore di dominio.  
  
3.  Aprire Server Manager.  
  
4.  Nell'area di notifica flag in **Server Manager**, fare clic sul contrassegno e quindi fare clic su **configurare Windows Server Essentials**.  
  
5.  Scegliere di configurare il server come server membro e quindi fare clic su **Avanti**.  
  
6.  Fare clic su **configura** per avviare la configurazione. Il processo di configurazione richiede circa 10 minuti.  
  
7.  Sul desktop, fare clic sull'icona del Dashboard per avviare il Dashboard del server. Nella Home page, completare la **Introduzione** le attività elencate nel **installazione** scheda.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Installare Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

