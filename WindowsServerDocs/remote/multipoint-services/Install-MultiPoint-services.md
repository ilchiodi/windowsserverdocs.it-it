---
title: Installare MultiPoint Services
description: Informazioni su come installare e configurare i servizi MultiPoint in Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 52a824bbca3e9f2e1c7823601f6208ae19ae50ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866222"
---
# <a name="install-multipoint-services"></a>Installare MultiPoint Services
Se si installa un server da zero, seguire queste istruzioni di installazione di MultiPoint Services.  

Dopo aver installato Windows Server 2016 effettuare l'accesso come amministratore. Utilizzare Server Manager in cui è possibile abilitare Servizi MultiPoint. Server Manager viene aperta automaticamente all'avvio. Nella schermata selezionare Dashboard **Aggiungi ruoli e funzionalità** per abilitare i servizi MultiPoint e seguire le istruzioni della procedura guidata.

Nella sezione per il tipo di installazione è possibile passare con il 
- Installazione basata su ruoli o basata su funzionalità o
- Installazione di Servizi Desktop remoto

Per le distribuzioni di servizi MultiPoint standard è consigliabile selezionare l'installazione di Servizi Desktop remoto che consente di selezionare facilmente il ruolo di servizi MultiPoint in tipo di distribuzione. Per l'installazione basata su ruoli è necessario selezionare **MultiPoint Services** nell'elenco dei ruoli. Il server verrà riavviato al termine dell'installazione.  
  
## <a name="configure-your-primary-station"></a>Configurare la stazione principale  
  
1.  Nel **creare una stazione di MultiPoint Server** , digitare la lettera specificata usando la tastiera per tale monitoraggio. La voce chiave associa la tastiera e mouse per la stazione.  
2.  Accedere come amministratore.  