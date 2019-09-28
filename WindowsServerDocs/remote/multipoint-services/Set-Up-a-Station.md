---
title: Configurare una stazione
description: Informazioni su come configurare una stazione in MultiPoint Services
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 95a332a3d15e82047b46cc19f168f945cdb334d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389451"
---
# <a name="set-up-a-station"></a>Configurare una stazione
Una *stazione* di MultiPoint Services in genere è costituita da un *hub di stazione*, un mouse, una tastiera e un monitor video. Questo argomento descrive come collegare i dispositivi hardware all'hub di stazione per creare una stazione di MultiPoint Services.  
  
L'hub di stazione è un dispositivo hardware che collega altri dispositivi a un computer in un sistema MultiPoint Services. MultiPoint Services supporta due tipi di hub di stazione:  
  
-   **Hub USB:** Hub di espansione USB multiporta generico conforme alle specifiche del bus seriale universale 2,0 o versioni successive. Tali hub hanno di norma due, quattro o più porte USB che consentono la connessione di più dispositivi USB a una singola porta USB del computer. Hub USB sono dispositivi comunemente separati che possono essere alimentati o basati su bus esternamente. Se impiegato come hub di stazione con MultiPoint Services, è consigliabile usare un hub con almeno quattro porte.  
  
    > [!IMPORTANT]  
    > Se si prevede di connettere all'hub dispositivi USB diversi da tastiera e mouse, per ottenere le prestazioni migliori è consigliabile usare un hub alimentato esternamente.  
  
-   **Hub multifunzione:** Un hub di espansione che si connette al computer tramite una porta USB e consente la connessione di un'ampia gamma di dispositivi non USB all'hub, incluso un monitor video. Hub multifunzione vengono prodotte dai produttori di hardware specifico e possono richiedere l'installazione di un driver specifico del dispositivo.  
  
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
  
7.  MultiPoint Services viene avviato. Seguire le istruzioni visualizzate sul monitor video della nuova stazione per associare i dispositivi alla nuova stazione.  
  
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
  
7.  MultiPoint Services viene avviato. Se richiesto, seguire le istruzioni visualizzate sul monitor video della nuova stazione per *associare* i dispositivi alla nuova stazione.  
  
## <a name="see-also"></a>Vedere anche  
[Terminare una sessione utente](End-a-User-Session.md)  
[Eseguire il riavvio o l'arresto](Restart-or-Shut-Down.md)  
[Gestire l'hardware delle stazioni](Manage-Station-Hardware.md)  
[Utilizzare dispositivi USB](Work-with-USB-Devices.md)