---
title: Aggiungere Windows Server Essentials come server membro
description: Viene descritto come usare Windows Server Essentials
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
ms.openlocfilehash: 09943f9708af3839ff21717316853fab9ba0283b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865051"
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Aggiungere Windows Server Essentials come server membro

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo argomento si applica a un server che esegue Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter o Windows Server 2016 con il ruolo esperienza Windows Server Essentials installato. Nella parte restante del documento, il ruolo Esperienza Windows Server Essentials verrà denominato Windows Server Essentials.  
  
> [!NOTE]
>   Windows Server Essentials può essere distribuito solo come controller di dominio. In questo documento Windows Server Essentials non include Windows Server Essentials.  
  
 Non è necessario che Windows Server Essentials sia un server primario all'interno di un dominio Windows. È possibile aggiungere Windows Server Essentials come server membro a un ambiente di dominio Active Directory esistente e sfruttarne le semplici funzionalità di protezione dei dati, accesso remoto sicuro e integrazione del cloud. Inoltre, è possibile distribuire Windows Server Essentials in un ambiente Active Directory esistente senza configurarlo come controller di dominio. In questo modo è possibile estendere le risorse di archiviazione oppure usare una succursale per l'amministrazione e l'archiviazione locale.  
  
 È possibile aggiungere Windows Server Essentials negli scenari seguenti:  
  
-   Aggiungere Windows Server Essentials in una succursale e aggiungerlo al controller di dominio situato nell'ufficio principale in un'ubicazione diversa usando gli strumenti nativi. È possibile attivare le funzionalità BranchCache per l'uso ottimale della larghezza di banda in questo server membro.  
  
-   Aggiungere Windows Server Essentials come server membro all'interno di una rete di Windows Server Essentials per estendere l'archiviazione nella rete aggiungendo altre cartelle server nel server membro.  
  
-   Aggiungere Windows Server Essentials come server membro nell'ufficio locale se il server primario che esegue Windows Server Essentials è ospitato in Microsoft Azure o ospitato da un host di terze parti. Disporre di Windows Server Essentials come server membro presso l'ufficio locale consente di ottimizzare l'utilizzo della larghezza di banda.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Aggiungere Windows Server Essentials come server membro  
 Per aggiungere Windows Server Essentials come server membro a un server primario che esegue Windows Server 2012 R2 o Windows Server Essentials in un ambiente di Active Directory esistente, è necessario completare i passaggi seguenti:  
  
1.  Aggiungere il server che esegue Windows Server Essentials a un gruppo di lavoro.  
  
2.  Aggiungere il server che esegue Windows Server Essentials al dominio di un server primario di Windows Server Essentials.  
  
3.  Configurare l'esperienza Windows Server Essentials da Server Manager.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Per aggiungere Windows Server Essentials a un dominio o un gruppo di lavoro  
  
1. Dopo aver completato l'installazione di Windows Server Essentials nel secondo server, chiudere la procedura guidata Configurazione di Windows Server Essentials.  
  
2. Nella casella **Cerca** digitare **System Settings**e nei risultati della ricerca fare clic su **Visualizza impostazioni di sistema avanzate**.  
  
3. In **Proprietà del sistema** fare clic sulla scheda **Nome computer**.  
  
4. In **Nome computer**, nella sezione **Dominio** , fare clic su **Cambia**.  
  
5. In **modifiche dominio/nome computer**, nella sezione **membro** , scegliere se si vuole aggiungere il server che esegue Windows Server Essentials a un **gruppo** di lavoro o a un **dominio**.  
  
   -   Per aggiungere il server a un gruppo di lavoro, digitare **workgroup** e fare clic su **OK**.  
  
   -   Per aggiungere il server a un dominio Active Directory esistente, digitare il nome del dominio e fare clic su **OK**.  
  
6. Riavviare il server per rendere effettive le modifiche.  
  
   Dopo aver aggiunto il server al dominio del server primario, è possibile continuare a configurare Windows Server Essentials eseguendo la configurazione guidata di Windows Server Essentials da Server Manager.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Per configurare Esperienza Windows Server Essentials in un server membro  
  
1.  (Facoltativo) Se necessario, modificare il nome del server.  
  
    > [!IMPORTANT]
    >  Non è possibile modificare il nome del server dopo aver configurato esperienza Windows Server Essentials.  
  
2.  Accedere al server usando l'account amministratore di dominio.  
  
3.  Aprire Server Manager.  
  
4.  Nell'area di notifica flag in **Server Manager** fare clic sul flag e quindi su **Configura Windows Server Essentials**.  
  
5.  Scegliere di configurare il server come server membro e quindi fare clic su **Avanti**.  
  
6.  Fare clic su **Configura** per avviare la configurazione. Il processo di configurazione richiede circa 10 minuti.  
  
7.  Sul desktop fare clic sull'icona del dashboard per avviare il dashboard del server. Nella Home page completare le **Attività iniziali** elencate nella scheda **Configura** .  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Installare Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

