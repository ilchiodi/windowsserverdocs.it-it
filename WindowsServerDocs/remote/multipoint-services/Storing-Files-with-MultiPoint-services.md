---
title: Archiviazione di file con Servizi MultiPoint
description: Informazioni sull'archiviazione di file in Servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: bf31f5c582cffb5b38cff8cb15fcfdb4b3f76386
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389304"
---
# <a name="storing-files-with-multipoint-services"></a>Archiviazione di file con Servizi MultiPoint
MultiPoint Services supporta l'archiviazione dei file utente nei modi seguenti:  
  
-   **Sulla partizione del sistema operativo dell'unità disco rigido.** Per impostazione predefinita, MultiPoint Services archivia i file utente sull'unità disco rigido con il sistema operativo.  
  
-   **In una partizione separata dell'unità disco rigido.** Quando il sistema MultiPoint Services è configurato per la prima volta, è possibile *partizionare* l'unità disco rigido. Ovvero, è possibile configurare una sezione dell'unità in modo che funzioni come se fosse un'unità separata. Questo rende più semplice ripristinare o aggiornare il sistema operativo senza influire sui file utente. Per ulteriori informazioni, vedere la pagina relativa alla [creazione di una partizione o di un'unità logica](https://go.microsoft.com/fwlink/?LinkId=182618) nella raccolta di documentazione tecnica su Windows Server.  
  
-   **In un'unità disco rigido interna o esterna aggiuntiva.** È possibile aggiungere unità disco rigido interne o esterne a MultiPoint Services per salvare ed eseguire il backup dei dati.  
  
-   **In una cartella di rete condivisa.** Per rendere i file utente disponibili da qualsiasi stazione, è possibile creare una cartella condivisa in rete. Questa operazione richiede un altro computer o server oltre al computer che esegue MultiPoint Services. Si tratta del metodo consigliato per l'archiviazione di file se è disponibile un file server.  
  
    Per i sistemi di piccole dimensioni di 2-3 computer che eseguono Servizi MultiPoint senza file server disponibili, uno dei computer MultiPoint Services può fungere da file server per tutti i computer Servizi MultiPoint. Si creeranno quindi gli account utente per tutti gli utenti di MultiPoint Services che funge da file server.  
  
