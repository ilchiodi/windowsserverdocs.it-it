---
title: Creare account utente locali
description: Informazioni su Abou thte tre tipi di account utente in MultiPoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: b07c88e4961544f5854f6e9d829b8d4b97adf7e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859764"
---
# <a name="create-local-user-accounts"></a>Creare account utente locali
È possibile creare tre livelli di account utente locali usando Gestione MultiPoint: account utente standard; Utenti del Dashboard MultiPoint, che hanno diritti amministrativi limitati; e account utente amministrativi completi.  
  
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