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
ms.openlocfilehash: ec367337318d12161a250572745d8f303d098ffe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849202"
---
# <a name="manage-vpn-in-windows-server-essentials"></a>Gestire la VPN in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Le connessioni VPN consentono agli utenti in trasferta o che lavorano da casa di accedere al server di una rete privata virtuale usando l'infrastruttura fornita da una rete pubblica, ad esempio Internet. Per utilizzare una rete VPN per accedere alle risorse del server, procedere come descritto di seguito:  
  
-   [Abilitare la VPN per l'accesso remoto nel server](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Impostare le autorizzazioni della VPN per gli utenti di rete](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Connettere i computer client al server](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Usare la VPN per connettersi a Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a> Abilitare la VPN per l'accesso remoto nel server  
 Completare la procedura seguente per configurare una VPN in Windows Server Essentials per l'abilitazione dell'accesso remoto.  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>Per abilitare la VPN in Windows Server Essentials  
  
1.  Aprire il dashboard.  
  
2.  Fare clic su **Impostazioni**, quindi fare clic sulla scheda **Accesso remoto via Internet** .  
  
3.  Fare clic su **configurare**. Verrà visualizzata la Configurazione guidata di Accesso remoto via Internet.  
  
4.  Nella pagina **Scegli le funzionalità di Accesso remoto via Internet da abilitare** selezionare la casella di controllo **Rete privata virtuale**.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_2"></a> Impostare le autorizzazioni della VPN per gli utenti di rete  
 È possibile usare una VPN per connettersi a Windows Server Essentials e accedere a tutte le risorse archiviate nel server. Ciò è particolarmente utile se si usa un computer client configurato con account di rete che possono essere usati per connettersi a un server Windows Server Essentials ospitato tramite una connessione VPN. Tutti i nuovi account utente creati nel server Windows Server Essentials ospitato devono usare la VPN per accedere al computer client per la prima volta.  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>Per impostare le autorizzazioni della VPN per gli utenti di rete  
  
1.  Aprire il dashboard.  
  
2.  Sulla barra di spostamento fare clic su **UTENTI**.  
  
3.  Nell'elenco di account utente selezionare quello a cui si vogliono concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel **< Account utente\> attività** riquadro, fare clic su **proprietà**.  
  
5.  Nel **< Account utente\> delle proprietà**, fare clic sui **accesso remoto via Internet** scheda.  
  
6.  Per permettere a un utente di connettersi al server tramite la VPN, nella scheda **Accesso remoto via Internet** selezionare la casella di controllo **Consenti rete privata virtuale (VPN)**  .  
  
7.  Fare clic su **Applica** e quindi su **OK**.  
  
##  <a name="BKMK_Connect"></a> Connettere i computer client al server  
 Dopo avere abilitato la VPN in un server che esegue Windows Server Essentials per l'accesso remoto, è possibile usare una connessione VPN per connettersi e accedere a tutte le risorse archiviate nel server. Tuttavia, è innanzitutto necessario connettere il computer al server. Quando si esegue la connessione di un computer al server mediante la procedura guidata Connessione del computer al server, nel computer client viene automaticamente generata una connessione di rete VPN che può essere usata per accedere alle risorse del server quando si è in trasferta o si lavora da casa. Per le istruzioni dettagliate sulla connessione del computer al server, vedere [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_3"></a> Usare la VPN per connettersi a Windows Server Essentials  
 Se è disponibile un computer client configurato con account di rete che possono essere usati per la connessione a un server ospitato che esegue Windows Server Essentials tramite una connessione tramite rete privata virtuale (VPN, Virtual Private Network), tutti gli account utente appena creati nel server ospitato dovranno usare la VPN per accedere al computer client per la prima volta. Completare la procedura seguente dal computer client connesso al server.  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Per usare la VPN per accedere in modalità remota alle risorse del server  
  
1.  Premere CTRL+ALT+CANC sul computer client.  
  
2.  Nella schermata di accesso fare clic su **Cambia utente**.  
  
3.  Fare clic sull'icona di accesso alla rete nell'angolo inferiore destro della schermata.  
  
4.  Accedere alla rete di Windows Server Essentials specificando nome utente di rete e password.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
