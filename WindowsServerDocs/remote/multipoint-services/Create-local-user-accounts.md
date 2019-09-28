---
title: Creare account utente locali
description: Informazioni su Abou thte tre tipi di account utente in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: 874d9cbdb56c59693cb5843a898b9ebd6893d674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405107"
---
# <a name="create-local-user-accounts"></a>Creare account utente locali
Con gestione MultiPoint è possibile creare tre livelli di account utente locali: Account utente standard; Utenti del Dashboard MultiPoint, che hanno diritti amministrativi limitati; e account utente amministrativi completi.  
  
Usare la procedura seguente per creare un account utente locale in un server MultiPoint. Se l'ambiente include più server MultiPoint e si desidera che l'utente sia in grado di accedere a qualsiasi stazione in qualsiasi server, è necessario creare un account utente locale in ogni server. Il programma di installazione presenta alcune limitazioni. In un ambiente di dominio è anche possibile consentire agli utenti di usare i propri account di dominio. Per una panoramica delle opzioni, vedere [pianificare gli account utente per l'ambiente Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Accedere al server come amministratore e aprire Gestione MultiPoint.  
  
2.  Fare clic sulla scheda **utenti** e quindi su **Aggiungi account utente**.  
  
    Si apre la procedura guidata Aggiungi account utente.  
  
3.  Immettere un nome account e una password per il nuovo account utente e quindi fare clic su **Avanti**.  
  
4.  Selezionare il tipo di account utente che si desidera creare:  
  
    -   **Utente standard** : può accedere a una stazione ed eseguire attività utente, ma non ha accesso a gestione MultiPoint o al Dashboard MultiPoint Server e non è in grado di arrestare il sistema.  
  
    -   **Utente del Dashboard MultiPoint** : diritti amministrativi limitati. Un utente del dashboard può aprire il dashboard ed eseguire attività quali la registrazione degli utenti dal sistema o l'arresto del computer Server MultiPoint, ma l'utente non ha accesso a gestione MultiPoint.  
  
    -   **Utente amministratore** Dispone di diritti amministrativi completi in MultiPoint Server. Un utente amministratore può ad esempio eseguire Gestione MultiPoint, aggiungere ed eliminare utenti, modificare le impostazioni di sistema e aggiornare i driver.  
  
5.  Fare clic su **Avanti**e quindi su **fine** per creare l'account utente.