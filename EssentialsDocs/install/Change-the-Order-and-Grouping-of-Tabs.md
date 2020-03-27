---
title: Modifica dell'ordine e del raggruppamento delle schede
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: abb443994b413f35f6d70510191bc543fad418f5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312234"
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Modifica dell'ordine e del raggruppamento delle schede

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile modificare l'ordine delle schede nel dashboard in modo tale che la propria scheda risulti la prima (a sinistra) nell'apposita riga. A tale scopo, è necessario aggiungere una voce al Registro di sistema. È inoltre possibile modificare il raggruppamento delle schede aggiungendo voci al Registro di sistema. L'ordine delle schede può essere: per prima la scheda principale dell'utente seguita dalle schede predefinite Microsoft, seguite a loro volta da eventuali schede aggiuntive dell'utente e infine dalle schede di terze parti.  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Modifica dell'ordine delle schede nel dashboard  
 Per definire l'ordine, è necessario aggiungere nel Registro di sistema l'identificare del componente aggiuntivo che visualizza la propria scheda.  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Per visualizzare la propria scheda per prima nell'apposito elenco  
  
1.  Nel computer di riferimento fare clic su **Start**, immettere **regedit** e quindi premere **INVIO**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, quindi **Windows Server**. Se la chiave **OEM** non esiste, è necessario effettuare le seguenti operazioni per crearla:  
  
    1.  Fare clic con il pulsante destro del mouse su **Windows Server**, scegliere **Nuovo**, quindi fare clic su **Chiave**.  
  
    2.  Digitare **OEM** come nome della chiave.  
  
3.  Fare clic con il pulsante destro del mouse su **OEM**, scegliere **Nuovo**, quindi fare clic su **Valore stringa**.  
  
4.  Digitare **DashboardMainTabID** come nome della stringa, quindi premere **Invio**.  
  
5.  Fare clic con il pulsante destro del mouse sulla nuova stringa nel riquadro a destra, quindi fare clic su **Modifica**.  
  
6.  Digitare il GUID definito per la scheda di livello superiore, quindi premere **Invio**.  
  
     Per altre informazioni sulla creazione e sull'identificazione delle schede di livello superiore, vedere [Creare una scheda di livello superiore](https://msdn.microsoft.com/library/gg513957) in Windows Server Solutions SDK.  
  
7.  Salvare le modifiche apportate al Registro di sistema.  
  
8.  È inoltre necessario includere il GUID della scheda principale di livello superiore nell'elenco degli identificatori per il raggruppamento delle schede. A tale scopo, effettuare i passaggi elencati nella sezione **Modifica del raggruppamento delle schede nel dashboard**.  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Modifica del raggruppamento delle schede nel dashboard  
 È possibile fare in modo che le schede dell'utente siano raggruppate assieme e incluse nell'elenco delle schede predefinite Microsoft aggiungendo gli identificatori al Registro di sistema.  
  
#### <a name="to-change-the-grouping-of-tabs"></a>Per modificare il raggruppamento delle schede  
  
1.  Se regedit non è aperto, fare clic su **Start**, immettere **regedit** e quindi premere **INVIO**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, quindi **Windows Server**.  
  
3.  Fare clic con il pulsante destro del mouse su **OEM**, scegliere **Nuovo**, quindi fare clic su **Chiave**.  
  
4.  Digitare **DashboardAddins** come nome della chiave, quindi premere **Invio**.  
  
5.  Fare clic con il pulsante destro del mouse su **DashboardAddins**, scegliere **Nuovo**, quindi fare clic su **Valore stringa**.  
  
6.  Digitare l'identificatore GUID della propria scheda come nome della stringa. Non è necessario immettere un valore.  
  
7.  Ripetere i passaggi 5 e 6 per ciascuna delle schede e sottoschede.  
  
8.  Salvare le modifiche apportate al Registro di sistema.  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)