---
title: Configurare una stazione
description: Informazioni su come configurare un una stazione MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dce05b6c-795e-43b2-9920-026550b873c5
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2bba32f27ae01052a693d78f152d4487a04bd9bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880682"
---
# <a name="set-up-a-station"></a>Configurare una stazione
Una *stazione* di MultiPoint Services in genere è costituita da un *hub di stazione*, un mouse, una tastiera e un monitor video. Questo argomento descrive come collegare i dispositivi hardware all'hub di stazione per creare una stazione di MultiPoint Services.  
  
L'hub di stazione è un dispositivo hardware che collega altri dispositivi a un computer in un sistema MultiPoint Services. MultiPoint Services supporta due tipi di hub di stazione:  
  
-   **Hub USB:** Un hub USB multiporta generico espansione conforme alle specifiche USB 2.0 o versione successiva. Tali hub hanno di norma due, quattro o più porte USB che consentono la connessione di più dispositivi USB a una singola porta USB del computer. Hub USB sono dispositivi comunemente separati che possono essere alimentati o basati su bus esternamente. Se impiegato come hub di stazione con MultiPoint Services, è consigliabile usare un hub con almeno quattro porte.  
  
    > [!IMPORTANT]  
    > Se si prevede di connettere all'hub dispositivi USB diversi da tastiera e mouse, per ottenere le prestazioni migliori è consigliabile usare un hub alimentato esternamente.  
  
-   **Hub multifunzione:** Hub di espansione che si connette al computer tramite una porta USB e consente la connessione di un'ampia gamma di dispositivi non USB all'hub, tra cui un monitor video. Hub multifunzione vengono prodotte dai produttori di hardware specifico e possono richiedere l'installazione di un driver specifico del dispositivo.  
  
Se si vuole aggiungere una stazione al sistema MultiPoint Services, assicurarsi innanzitutto di avere un numero di porte di connessione sufficiente per l'hardware della stazione che si vuole usare. Inoltre, è necessario proteggere il numero appropriato di *licenze di accesso client (CAL)* per il sistema di servizi MultiPoint.  
  
## <a name="setting-up-station-hardware"></a>Impostazione dell'hardware della stazione  
Le procedure riportate in questa sezione descrivono come collegare l'hardware della stazione di MultiPoint Services ai vari tipi di hub di stazione.  
  
### <a name="to-set-up-a-station-with-a-usb-hub"></a>Per configurare una stazione con un hub USB  
  
1.  Prima di collegare una nuova stazione, *chiudere* tutte le *sessioni* utente e quindi arrestare il computer e altri dispositivi alimentati nel sistema MultiPoint Services.  
  
2.  Collegare il cavo del nuovo monitor video alla porta video del computer, come mostrato nell'illustrazione seguente:  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif)  
  
3.  Collegare il nuovo hub USB a una porta USB aperta del computer:  
  
    ![Immagine della connessione hub USB di MultiPoint Server](./media/WMSUSBHubConnection.gif)  
  
4.  Collegare una tastiera e un mouse all'hub USB:  
  
    ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Collegare il cavo di alimentazione del monitor video a una presa elettrica.  
  
6.  Accendere il computer.  
  
7.  MultiPoint Services viene avviato. Seguire le istruzioni visualizzate sul monitor video della nuova stazione per associare i dispositivi alla stazione.  
  
### <a name="to-set-up-a-station-with-a-multifunction-hub"></a>Per configurare una stazione con un hub multifunzione  
  
1.  Prima di collegare una nuova stazione, chiudere tutte le sessioni utente e quindi arrestare il computer e altri dispositivi alimentati nel sistema MultiPoint Services.  
  
2.  Collegare il cavo del nuovo monitor video alla porta video DVI o VGA dell'hub multifunzione, come mostrato nell'illustrazione seguente:  
  
    ![Immagine di un hub multifunzione e connessione video](./media/WMSMultifunctionHubVideoConnection.gif)  
  
3.  Collegare il nuovo hub multifunzione a una porta USB aperta del computer:  
  
    ![Immagine di una connessione hub multifunzione](./media/WMSMultifunctionHubConnection.gif)  
  
4.  Collegare una tastiera e un mouse ai connettori PS2 o USB sull'hub multifunzione:  
  
    ![Immagine di un hub multifunzione e connettori PS2](./media/WMSMultifunctionHubPS2Connection.gif)  
  
5.  Collegare il cavo di alimentazione del monitor video a una presa elettrica.  
  
6.  Accendere il computer.  
  
7.  MultiPoint Services viene avviato. Se richiesto, seguire le istruzioni visualizzate sul monitor video della nuova stazione per *associare* i dispositivi alla stazione.  
  
## <a name="see-also"></a>Vedere anche  
[Chiudere una sessione utente](End-a-User-Session.md)  
[Riavviare o arrestare](Restart-or-Shut-Down.md)  
[Gestire l'Hardware delle stazioni](Manage-Station-Hardware.md)  
[Utilizzare dispositivi USB](Work-with-USB-Devices.md)