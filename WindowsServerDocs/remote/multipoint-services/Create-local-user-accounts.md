---
title: Creare account utente locali
description: Informazioni su informa graduazione tre tipi di account utente in servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e2e5a6c79e6dcd603d19ca868df1d11fce2bf746
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823672"
---
# <a name="create-local-user-accounts"></a>Creare account utente locali
È possibile creare tre livelli di account utente locali con la gestione MultiPoint: Account utente standard. Utenti di Dashboard multiPoint, che non dispongono di diritti amministrativi; e gli account utente con privilegi amministrativi completi.  
  
Utilizzare la procedura seguente per creare un account utente locale in un server MultiPoint. Se l'ambiente include più server MultiPoint e si vuole che l'utente sia in grado di accedere a qualsiasi stazione su qualsiasi server, è necessario creare un account utente locale in ogni server. Dal programma di installazione presenta alcune limitazioni. In un ambiente di dominio, è anche possibile consentire agli utenti usano i propri account di dominio. Per una panoramica delle opzioni, vedere [pianificare gli account utente per l'ambiente di Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Accedere al server come amministratore e aprire Gestione MultiPoint.  
  
2.  Scegliere il **gli utenti** scheda e quindi fare clic su **aggiungere account utente**.  
  
    Verrà visualizzata la procedura guidata Aggiungi Account utente.  
  
3.  Immettere un nome account e password per il nuovo account utente e quindi fare clic su **successivo**.  
  
4.  Selezionare il tipo di account utente che si desidera creare:  
  
    -   **Utente standard** : è possibile accedere a una stazione ed eseguire attività utente, ma non ha accesso a gestione MultiPoint o Dashboard MultiPoint Server e non è possibile arrestare il sistema.  
  
    -   **Utente di Dashboard multiPoint** -con diritti amministrativi limitati. Un utente di Dashboard è possibile aprire il Dashboard ed eseguire attività quali l'accesso agli utenti di disattivare il sistema o l'arresto del computer MultiPoint Server, ma l'utente non ha accesso a gestione MultiPoint.  
  
    -   **Utente con privilegi amministrativi** disponga dei diritti amministrativi completi in MultiPoint Server. Ad esempio, un utente amministratore può eseguire Gestione MultiPoint, aggiungere ed eliminare gli utenti, modificare le impostazioni di sistema e aggiornare i driver.  
  
5.  Fare clic su **successivo**, quindi fare clic su **fine** per creare l'account utente.