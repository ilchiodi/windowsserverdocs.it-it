---
title: Utilizzare dispositivi video
description: Informazioni su come video consente di monitorare e lavorano insieme con le stazioni MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d828ea911aaff27a1df79d0380dfe92987c3d2aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844202"
---
# <a name="work-with-video-devices"></a>Utilizzare dispositivi video
Questo argomento descrive il funzionamento di dispositivi video, ad esempio monitor o proiettori, collegati a un computer nel sistema MultiPoint Services o a una *stazione* MultiPoint Services.  
  
## <a name="working-with-video-monitors"></a>Utilizzo di monitor video  
A seconda dell'hardware del sistema MultiPoint Services, un monitor video può essere collegato in due modi:  
  
-   Per la *sistemi basati su hub USB*, connettere il cavo del monitor video a una porta video aperta sul computer, come illustrato nella figura seguente:  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif)  
  
-   Per la *sistemi basati su hub multifunzionali* con supporto video incorporato, collegare il cavo del monitor video alla porta video sull'hub multifunzione:  
  
    ![Immagine di un hub multifunzione e connessione video](./media/WMSMultifunctionHubVideoConnection.gif)  
  
Per altre informazioni, vedere l'argomento [Configurare una stazione](Set-Up-a-Station.md).  
  
## <a name="working-with-video-projectors"></a>Utilizzo di proiettori video  
È possibile collegare un proiettore video al sistema MultiPoint Services quando si vuole proiettare un'immagine di grandi dimensioni affinché venga visualizzata da altri, ad esempio in un laboratorio. Per entrambi USB basato su hub multifunzione basato su hub stazioni e, sono disponibili due opzioni per collegare un proiettore a una stazione:  
  
-   Sostituire un monitor con un proiettore e usare il proiettore come dispositivo di visualizzazione per la stazione, come mostrato nell'illustrazione seguente:  
  
    ![Immagine di un proiettore connesso al computer](./media/WMSVideoProjectorConnection.gif)  
  
-   Munirsi di un dispositivo video splitter per collegare un proiettore e un monitor alla porta video della stazione.  
  
    MultiPoint Services visualizzerà la stessa immagine su entrambi i dispositivi di visualizzazione. Quando non si esegue la proiezione, è possibile spegnere il proiettore e usare solo il monitor video.  
  
Quando si usa una delle due opzioni, tenere presente quanto segue:  
  
-   Per il collegamento di uno schermo può essere necessaria una nuova *associazione della stazione* affinché MultiPoint Services possa riconoscere correttamente il nuovo display. Seguire le istruzioni visualizzate sul dispositivo video della stazione.  
  
-   Può essere necessario un adattatore o un convertitore per convertire le prese DVI e VGA.  
  
-   L'uso di un cavo ripartitore a "Y" può ridurre la qualità video su entrambi i dispositivi.  
  
-   Quando si usa un proiettore e un monitor tramite un cavo splitter a "Y", MultiPoint Services regola la risoluzione dello schermo di entrambi i dispositivi sulla risoluzione più bassa possibile di uno dei dispositivi, generalmente il proiettore.  
  
-   MultiPoint Services non supporta l'estensione della visualizzazione di una singola stazione su più monitor.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire l'Hardware delle stazioni](Manage-Station-Hardware.md)  
[Configurare una stazione](Set-Up-a-Station.md) 