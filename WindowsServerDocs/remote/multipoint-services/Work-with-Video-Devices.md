---
title: Utilizzare dispositivi video
description: Informazioni su come funzionano i monitor e i proiettori video con le stazioni in MultiPoint Services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 6b967d4523058fe1dfcb086e5918f84257bd51bf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820444"
---
# <a name="work-with-video-devices"></a>Utilizzare dispositivi video
Questo argomento descrive il funzionamento di dispositivi video, ad esempio monitor o proiettori, collegati a un computer nel sistema MultiPoint Services o a una *stazione* MultiPoint Services.  
  
## <a name="working-with-video-monitors"></a>Utilizzo di monitor video  
A seconda dell'hardware del sistema MultiPoint Services, un monitor video può essere collegato in due modi:  
  
-   Per i *sistemi basati su hub USB*, connettere il cavo del monitor video a una porta video aperta nel computer, come illustrato nella figura seguente:  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif)  
  
-   Per i *sistemi multifunzione basati su Hub* con supporto video incorporato, connettere il cavo del monitor video alla porta video nell'hub multifunzione:  
  
    ![Immagine di un hub multifunzione e connessione video](./media/WMSMultifunctionHubVideoConnection.gif)  
  
Per altre informazioni, vedere l'argomento [Configurare una stazione](Set-Up-a-Station.md).  
  
## <a name="working-with-video-projectors"></a>Utilizzo di proiettori video  
È possibile collegare un proiettore video al sistema MultiPoint Services quando si vuole proiettare un'immagine di grandi dimensioni affinché venga visualizzata da altri, ad esempio in un laboratorio. Per le stazioni basate su hub USB e multifunzione, sono disponibili due opzioni per la connessione di un proiettore a una stazione:  
  
-   Sostituire un monitor con un proiettore e usare il proiettore come dispositivo di visualizzazione per la stazione, come mostrato nell'illustrazione seguente:  
  
    ![Immagine di un proiettore connesso al computer](./media/WMSVideoProjectorConnection.gif)  
  
-   Ottenere un dispositivo Splitter video per connettere un proiettore e un monitor alla porta video della stazione.  
  
    MultiPoint Services visualizzerà la stessa immagine su entrambi i dispositivi di visualizzazione. Quando non si esegue la proiezione, è possibile spegnere il proiettore e usare solo il monitor video.  
  
Quando si usa una delle due opzioni, tenere presente quanto segue:  
  
-   Per il collegamento di uno schermo può essere necessaria una nuova *associazione della stazione* affinché MultiPoint Services possa riconoscere correttamente il nuovo display. Seguire le istruzioni visualizzate sul dispositivo di visualizzazione video della stazione.  
  
-   Può essere necessario un adattatore o un convertitore per convertire le prese DVI e VGA.  
  
-   L'uso di un cavo splitter "Y" può ridurre la qualità del video su entrambi i dispositivi video.  
  
-   Quando si usano sia un proiettore che un monitor tramite un cavo splitter "Y", MultiPoint Services regola la risoluzione dello schermo di entrambi i dispositivi sulla risoluzione massima più bassa di uno dei due dispositivi, in genere il proiettore.  
  
-   MultiPoint Services non supporta l'estensione della visualizzazione di una singola stazione su più monitor.  
  
## <a name="see-also"></a>Vedi anche  
[Gestire l'hardware delle stazioni](Manage-Station-Hardware.md)  
[Configurare una stazione](Set-Up-a-Station.md) 