---
title: Dividere una stazione utente
description: Informazioni su come dividere una visualizzazione in MultiPoint Services in modo che due utenti possano usare la stessa stazione
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: db828da06045bb884db138458f875af1d4d5b99d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853894"
---
# <a name="split-a-user-station"></a>Dividere una stazione utente
Qualsiasi monitoraggio della stazione MultiPoint Services con risoluzione superiore a 1024x768 può essere suddiviso in due stazioni usando l'attività **Split Station** nella scheda **stazioni** . Il desktop presente nel monitoraggio nel momento in cui si verifica la suddivisione si sposta sulla metà sinistra del monitoraggio e viene creata una nuova stazione nella metà destra dello stesso monitor. Per completarne la creazione, la nuova stazione deve essere mappata a una tastiera, un mouse e un hub USB. Dopo che la stazione è stata divisa, un utente può connettersi alla stazione sulla sinistra mentre un altro utente si connette alla stazione sulla destra.  
  
I vantaggi dell'uso di una stazione a schermo diviso possono includere:  
  
-   Riduzione del costo e dello spazio grazie alla possibilità di riunire più studenti su un sistema MultiPoint Services  
  
-   Consentire a due studenti di collaborare insieme, Side-by-side in un progetto  
  
-   Possibilità di consentire a un insegnante di illustrare una procedura su una stazione mentre uno studente segue sull'altra stazione  
   
> [!NOTE]  
> Quando si divide una stazione, la sessione attiva nella stazione viene sospesa. Dopo la divisione, l'utente dovrà riconnettersi alla stazione per riprendere il lavoro.  
  
**Per dividere una stazione:**  
  
1.  In Gestione MultiPoint, in modalità stazione, fare clic sulla scheda **stazioni** .  
  
2.  Nella colonna **Station** (Stazione) fare clic sul nome della stazione da dividere.  
  
3.  In **Stations Tasks** (Attività stazioni) fare clic su **Split station** (Dividi stazione).  
  
**Per restituire una stazione di divisione a un'unica stazione:**  
  
1.  In Gestione MultiPoint, in modalità stazione, fare clic sulla scheda **stazioni** .  
  
2.  Nella colonna **Station** (Stazione) fare clic sul nome della stazione di cui si vuole terminare la divisione.  
  
3.  In **Stations Tasks** (Attività stazioni) fare clic su **Unsplit station** (Annulla divisione stazione).  
  
## <a name="see-also"></a>Vedi anche  
[Gestire stazioni utente](Manage-User-Stations.md)