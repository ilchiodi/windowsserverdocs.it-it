---
title: Modifica dell'ordine e il raggruppamento delle schede
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 578c5619cfdf076bb2735254494f393d56d35713
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Modifica dell'ordine e il raggruppamento delle schede

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile modificare l'ordine delle schede nel Dashboard in modo che la scheda è la prima (a sinistra) nella riga delle schede. A tale scopo aggiungere una voce al Registro di sistema. È inoltre possibile modificare il raggruppamento delle schede aggiungendo voci al Registro di sistema. L'ordine delle schede può essere la scheda principale seguita dalle schede predefinite Microsoft, seguita da eventuali schede aggiuntive dell'utente e infine dalle schede di terze parti.  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Modificare l'ordine delle schede nel Dashboard di  
 È necessario aggiungere l'identificatore del componente aggiuntivo che visualizza la propria scheda nel Registro di sistema per definire l'ordine.  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Per visualizzare la propria scheda prima nell'elenco delle schede  
  
1.  Nel computer di riferimento, fare clic su **Start**, immettere **regedit**, quindi premere **invio**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, quindi espandere **Windows Server**. Se il **OEM** chiave non esiste, è necessario completare i passaggi seguenti per crearla:  
  
    1.  Fare doppio clic su **Windows Server**, scegliere **New**, quindi fare clic su **chiave**.  
  
    2.  Tipo **OEM** per il nome della chiave.  
  
3.  Fare doppio clic su **OEM**, scegliere **New**, quindi fare clic su **valore stringa**.  
  
4.  Tipo **DashboardMainTabID** come nome della stringa, quindi premere **invio**.  
  
5.  Fare clic sulla nuova stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  
6.  Digitare il GUID definito per la scheda di livello superiore e quindi premere **invio**.  
  
     Per ulteriori informazioni sulla creazione e l'identificazione delle schede di livello superiore, vedere [creare una scheda di livello superiore](https://msdn.microsoft.com/library/gg513957) in Windows Server Solutions SDK.  
  
7.  Salvare le modifiche al Registro di sistema.  
  
8.  È inoltre necessario includere il GUID per la scheda principale di livello superiore nell'elenco degli identificatori per il raggruppamento delle schede. A tale scopo, eseguire i passaggi elencati in **modifica del raggruppamento delle schede nel Dashboard**.  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Modifica del raggruppamento delle schede nel Dashboard  
 È possibile garantire che le schede siano raggruppate e incluso nell'elenco delle schede predefinite Microsoft aggiungendo gli identificatori al Registro di sistema.  
  
#### <a name="to-change-the-grouping-of-tabs"></a>Per modificare il raggruppamento delle schede  
  
1.  Se regedit non è aperta, fare clic su **Start**, immettere **regedit**, quindi premere **invio**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, quindi espandere **Windows Server**.  
  
3.  Fare doppio clic su **OEM**, scegliere **New**, quindi fare clic su **chiave**.  
  
4.  Tipo **DashboardAddins** come nome della chiave, quindi premere **invio**.  
  
5.  Fare doppio clic su **DashboardAddins**, scegliere **New**, quindi fare clic su **valore stringa**.  
  
6.  Digitare l'identificatore GUID della propria scheda come nome della stringa. Un valore non è necessaria.  
  
7.  Ripetere i passaggi 5 e 6 per ciascuna delle schede e sottoschede.  
  
8.  Salvare le modifiche del Registro di sistema.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)