---
title: Modificare le impostazioni del server
description: Informazioni sulle impostazioni di MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 105b10428835d11a0ea0661fe2fa7d57f80a1aba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885182"
---
# <a name="edit-server-settings"></a>Modificare le impostazioni del server
Al momento dell'installazione di MultiPoint Services sono state configurate le impostazioni per il sistema, incluse quelle riguardanti determinati programmi. Questo argomento descrive le impostazioni disponibili per il sistema MultiPoint Services e spiega in che modo è possibile modificarle.  
  
## <a name="about-multipoint-services-settings"></a>Informazioni sulle impostazioni di MultiPoint Services  
La tabella seguente descrive le diverse impostazioni che è possibile modificare per il sistema MultiPoint Services.  
  
|Impostazione MultiPoint Services|Descrizione|  
|-----------------------------------------------------------------------------------------|---------------|  
|Consentire a un account di avere più sessioni|Consente a un singolo account utente di collegarsi contemporaneamente a più stazioni. Questo può essere utile, ad esempio, nel caso di un'aula in cui tutti gli studenti usano un singolo account condiviso. Se si usa questa impostazione, tutte le modifiche apportate alle risorse dell'account, ad esempio le cartelle di documenti o il desktop, sono disponibili per tutti gli utenti collegati mediante lo stesso account.|  
|Allow this computer to be managed remotely (Consenti al computer di essere gestito da postazione remota)|Consente al computer che esegue servizi MultiPoint da gestire con altri sistemi MultiPoint sulla rete. Se questa opzione è selezionata e il computer di gestione si trova nella stessa subnet, il computer viene visualizzato nell'elenco dei server che possono essere gestiti. Se questa opzione è selezionata e il computer di gestione si trova in una subnet diversa, il computer di gestione può comunque gestire questo computer, ma è necessario specificare l'indirizzo IP del computer.|
|Allow monitoring of this computer’s desktops (Consenti il monitoraggio dei desktop di questo computer)|Consente di controllare se i desktop possono essere monitorati nel sistema MultiPoint Services. Se questa impostazione è disattivata (non selezionato), desktop di stazioni (locali e remote) che sono connessi al computer in cui è in esecuzione servizi MultiPoint non verranno visualizzati nella scheda Home della gestione MultiPoint (incluso in un computer diverso se il computer è gestito in modalità remota).|  
|Always start in console mode (Avvia sempre in modalità console)|Abilita la tecnologia RemoteFX, progettata per consentire sessioni di desktop remoto più veloci ed efficienti grazie all'offload dell'elaborazione sulla CPU e sulla GPU. Se ci si connette ai servizi MultiPoint utilizzando un client con supporto RemoteFX, sarà possibile ottenere prestazioni migliori utilizzando questa opzione. I vantaggi dipendono dalle funzionalità del server e della rete. Questo, ad esempio, dipende in parte dalla possibilità che il tempo impiegato per un'ulteriore elaborazione allo scopo di comprimere il flusso di dati sia inferiore al tempo risparmiato trasmettendo una quantità di dati inferiore.|  
|Do not show privacy notification at first user logon (Non visualizzare l'informativa sulla privacy al primo accesso)|Quando un utente accede a una stazione MultiPoint per la prima volta, viene visualizzata una notifica per informarlo che le attività della stazione possono essere monitorate.|  
|Assign a unique IP to each station (Assegna un IP univoco a ogni stazione)|Assegna un indirizzo IP univoco a ogni stazione. Per impostazione predefinita, MultiPoint Services dispone di un solo indirizzo IP, condiviso con tutte le sessioni in esecuzione nel sistema. Questa configurazione, tuttavia, può causare alcuni problemi di compatibilità delle applicazioni. Se, ad esempio, un'applicazione richiede un indirizzo IP univoco, è possibile che tale applicazione non venga eseguita correttamente in MultiPoint Services. La selezione di questa opzione, altrimenti nota come virtualizzazione IP, può risolvere il problema.<br /><br />La virtualizzazione IP è utile anche per il monitoraggio delle sessioni attive in MultiPoint Services. Alcuni strumenti di monitoraggio riportano l'utilizzo in base all'indirizzo IP. Per abilitare il monitoraggio delle sessioni, è possibile usare la virtualizzazione IP per assegnare un indirizzo IP univoco a ogni sessione. Si noti che, se si seleziona questa opzione, ogni nuova sessione riceve un indirizzo IP univoco. Tutte le sessioni esistenti continuano a usare l'indirizzo IP condiviso fino a quando non vengono scollegate e ricollegate.|  
|Allow IM between MultiPoint Dashboard and a user session on this computer (Consenti messaggistica immediata tra Dashboard MultiPoint e una sessione utente in questo computer)|Abilita la chat tra MultiPoint Manager e la sessione utente nel computer. Per altre informazioni, vedere [Use IM](Use-IM.md) (Usare la messaggistica istantanea).|  
|Allow orchestration of administrator and MultiPoint Dashboard user sessions (Consenti orchestrazione di amministratore e sessioni utente Dashboard MultiPoint)|Quando abilitata, consente agli amministratori di usare Dashboard MultiPoint per l'orchestrazione delle sessioni. Queste sessioni vengono visualizzate come anteprime.|  
|Allow stations to use GPU hardware rendering (Consenti alle stazioni di utilizzare il rendering hardware GPU)|Controlla se le stazioni possono usare l'unità di elaborazione grafica (GPU) del sistema.|   
  
## <a name="editing-the-computer-settings"></a>Modifica delle impostazioni del computer  
  
1.  Aprire Gestione MultiPoint in [modalità stazione](Switch-Between-Modes.md), quindi fare clic sui **Home** scheda.  
  
2.  Nel **Computer** colonna, fare clic sul nome del computer e quindi selezionare la *nome computer* **attività**, fare clic su **modificare le impostazioni del computer**.  
  
3.  Selezionare o deselezionare gli elementi che si desidera modificare e quindi fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire le attività di sistema tramite Gestione MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
