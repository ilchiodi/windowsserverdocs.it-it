---
title: Dividere una stazione utente
description: Informazioni su come dividere una visualizzazione in MultiPoint Services in modo che due utenti possano usare la stessa stazione
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 5067df3f5902570d56ee130264c751d66b5b0d3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394946"
---
# <a name="split-a-user-station"></a>Dividere una stazione utente
Qualsiasi monitor di stazione di MultiPoint Services con risoluzione superiore a 1024x768 può essere diviso in due stazioni tramite l'attività **Split station** (Dividi stazione) nella scheda **Stations** (Stazioni). Il desktop presente sul monitor al momento della divisione passa nella metà sinistra del monitor e la nuova stazione viene creata nella metà destra dello stesso monitor. Per completarne la creazione, la nuova stazione deve essere mappata a una tastiera, un mouse e un hub USB. Dopo che la stazione è stata divisa, un utente può connettersi alla stazione sulla sinistra mentre un altro utente si connette alla stazione sulla destra.  
  
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
  
## <a name="see-also"></a>Vedere anche  
[Gestire stazioni utente](Manage-User-Stations.md)