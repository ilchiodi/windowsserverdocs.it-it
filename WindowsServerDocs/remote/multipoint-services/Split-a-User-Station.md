---
title: Dividere una stazione utente
description: Informazioni su come suddividere una visualizzazione in MultiPoint Services in modo che due utenti possono usare la stessa stazione
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 60d8f58a4e76a9e1ed5d6794e87a054d88628e1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823602"
---
# <a name="split-a-user-station"></a>Dividere una stazione utente
Qualsiasi monitor di stazione di MultiPoint Services con risoluzione superiore a 1024x768 può essere diviso in due stazioni tramite l'attività **Split station** (Dividi stazione) nella scheda **Stations** (Stazioni). Il desktop presente sul monitor al momento della divisione passa nella metà sinistra del monitor e la nuova stazione viene creata nella metà destra dello stesso monitor. Per completarne la creazione, la nuova stazione deve essere mappata a una tastiera, un mouse e un hub USB. Dopo che la stazione è stata divisa, un utente può connettersi alla stazione sulla sinistra mentre un altro utente si connette alla stazione sulla destra.  
  
Vantaggi dell'uso di una stazione con schermo diviso possono includere:  
  
-   Riduzione del costo e dello spazio grazie alla possibilità di riunire più studenti su un sistema MultiPoint Services  
  
-   Che consente due studenti di collaborare tra loro, side-by-side in un progetto  
  
-   Possibilità di consentire a un insegnante di illustrare una procedura su una stazione mentre uno studente segue sull'altra stazione  
   
> [!NOTE]  
> Quando si divide una stazione, la sessione attiva nella stazione viene sospesa. Dopo la divisione, l'utente dovrà riconnettersi alla stazione per riprendere il lavoro.  
  
**Per dividere una stazione:**  
  
1.  Selezionare Gestione MultiPoint in modalità stazione, scegliere il **stazioni** scheda.  
  
2.  Nella colonna **Station** (Stazione) fare clic sul nome della stazione da dividere.  
  
3.  In **Stations Tasks** (Attività stazioni) fare clic su **Split station** (Dividi stazione).  
  
**Per restituire una stazione divisa nuovamente una stazione singola:**  
  
1.  Selezionare Gestione MultiPoint in modalità stazione, scegliere il **stazioni** scheda.  
  
2.  Nella colonna **Station** (Stazione) fare clic sul nome della stazione di cui si vuole terminare la divisione.  
  
3.  In **Stations Tasks** (Attività stazioni) fare clic su **Unsplit station** (Annulla divisione stazione).  
  
## <a name="see-also"></a>Vedere anche  
[Gestire stazioni utente](Manage-User-Stations.md)