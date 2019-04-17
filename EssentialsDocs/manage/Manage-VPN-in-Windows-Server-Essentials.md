---
title: Gestire la VPN in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 08a08a13d696371420bdfdf89f54320c787636b0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-vpn-in-windows-server-essentials"></a>Gestire la VPN in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Le connessioni di rete privata virtuale (VPN) consentono agli utenti che lavorano da casa o in viaggio accedere a un server in una rete privata tramite l'infrastruttura fornita da una rete pubblica, ad esempio Internet. Per usare VPN per accedere alle risorse del server, è necessario completare le operazioni seguenti:  
  
-   [Abilitare la VPN per l'accesso remoto nel server di](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Impostare le autorizzazioni della VPN per gli utenti di rete](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Connettere i computer client al server](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Usare la VPN per connettersi a Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a>Abilitare la VPN per l'accesso remoto nel server di  
 Completare la procedura seguente per configurare una VPN in Windows Server Essentials per abilitare l'accesso remoto.  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>Per abilitare la VPN in Windows Server Essentials  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **impostazioni**, quindi fare clic su di **accesso remoto via Internet** scheda.  
  
3.  Fare clic su **configurare**. Verrà visualizzata la configurazione ovunque guidata di accesso.  
  
4.  Nel **funzionalità scegliere accesso remoto via Internet da abilitare** pagina, selezionare il **rete privata virtuale** casella di controllo.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_2"></a>Impostare le autorizzazioni della VPN per gli utenti di rete  
 È possibile utilizzare VPN per connettersi a Windows Server Essentials e accedere a tutte le risorse che sono archiviate nel server. Ciò è particolarmente utile se si dispone di un computer client che viene configurato con account di rete che può essere utilizzato per connettersi a un server Windows Server Essentials ospitato tramite una connessione VPN. Tutti gli account utente appena creati nel server Windows Server Essentials ospitato devono usare VPN per accedere al computer client per la prima volta.  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>Per impostare le autorizzazioni della VPN per gli utenti di rete  
  
1.  Aprire il Dashboard.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **proprietà**.  
  
5.  Nel **< utente Account\ > proprietà**, fare clic su di **accesso remoto via Internet** scheda.  
  
6.  Nel **accesso remoto via Internet** scheda, per consentire all'utente di connettersi al server tramite la VPN, selezionare il **Consenti privata rete virtuale (VPN)** casella di controllo.  
  
7.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
##  <a name="BKMK_Connect"></a>Connettere i computer client al server  
 Dopo aver abilitato la VPN in un server che esegue Windows Server Essentials per l'accesso remoto, è possibile utilizzare una connessione VPN per connettersi e accedere a tutte le risorse che sono archiviate nel server. Tuttavia, prima è necessario collegare il computer al server. Quando si connette un computer al server tramite la connessione del Computer per la creazione guidata Server, una connessione di rete VPN viene generata automaticamente nel computer client e può essere utilizzata per accedere alle risorse del server si lavora da casa o in viaggio. Per istruzioni dettagliate sulla connessione del computer al server, vedere [connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_3"></a>Usare la VPN per connettersi a Windows Server Essentials  
 Se si dispone di un computer client che viene configurato con account di rete che può essere utilizzato per connettersi a un server ospitato che esegue Windows Server Essentials tramite una connessione VPN, tutti gli account utente appena creati server ospitato devono utilizzare VPN per accedere al computer client per la prima volta. Completare la procedura seguente dal computer client connesso al server.  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Per usare la VPN per accedere in modalità remota alle risorse del server  
  
1.  Premere Ctrl + Alt + Canc sul computer client.  
  
2.  Fare clic su **Cambia utente** nella schermata di accesso.  
  
3.  Fare clic sull'icona di accesso di rete nell'angolo inferiore destro dello schermo.  
  
4.  Accedere alla rete di Windows Server Essentials tramite il proprio nome utente e la password.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
