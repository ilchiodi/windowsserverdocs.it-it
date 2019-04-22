---
title: Archiviazione di file con Servizi MultiPoint
description: Informazioni sull'archiviazione di file in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b432ca793b156997761f9fadab7340c394e3b553
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817342"
---
# <a name="storing-files-with-multipoint-services"></a>Archiviazione di file con Servizi MultiPoint
Servizi multiPoint supporta l'archiviazione di file utente nei modi seguenti:  
  
-   **Nella partizione di sistema operativo dell'unità disco rigido.** Per impostazione predefinita, MultiPoint Services archivia i file utente nell'unità disco rigido con il sistema operativo.  
  
-   **In una partizione separata dell'unità disco rigido.** Quando il sistema MultiPoint Services è configurato per la prima volta, è possibile *partizione* l'unità disco rigido. Vale a dire, è possibile configurare una sezione dell'unità in modo che funzioni come se fosse un'unità separata. Questo rende più semplice ripristinare o aggiornare il sistema operativo senza modificare i file dell'utente. Per altre informazioni, vedere [creare una partizione o l'unità logica](https://go.microsoft.com/fwlink/?LinkId=182618) nella libreria tecnica di Windows Server.  
  
-   **Un aggiuntive all'interno o esterno unità disco rigido.** È possibile collegare altre all'interni o esterni unità disco rigido a MultiPoint Services per il salvataggio e il backup dei dati.  
  
-   **In una cartella di rete condivisa.** Per rendere disponibili i file dell'utente da qualsiasi stazione, è possibile creare una cartella condivisa in rete. È richiesto un altro computer o server oltre a computer che esegue MultiPoint Services. Questo è il metodo consigliato per l'archiviazione dei file se è disponibile un file server.  
  
    Per piccoli sistemi di computer 2 e 3 che esegue servizi MultiPoint con nessun server di file disponibile, uno dei computer MultiPoint Services può agire come server di file per tutti i computer di MultiPoint Services. Creare quindi gli account utente per tutti gli utenti in MultiPoint Services che funge da file server.  
  
