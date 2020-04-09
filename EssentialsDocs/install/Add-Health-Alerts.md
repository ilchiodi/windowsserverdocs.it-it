---
title: Aggiunta di avvisi di integrità
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2540c4af032d35f5be71338513030c59cb18953d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817625"
---
# <a name="add-health-alerts"></a>Aggiunta di avvisi di integrità

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Un componente aggiuntivo per lo stato del sistema fornisce le definizioni di avvisi, controlli di integrità e soluzioni ai problemi di rete. Tale componente aggiuntivo è composto da file XML che annotano il codice o i dati utilizzati per valutare le informazioni sulle condizioni di una specifica funzione. I componenti aggiuntivi per lo stato del sistema vengono creati dagli sviluppatori, quindi installati nel server e nei computer client dall'amministratore.  
  
 Per altre informazioni sulla creazione di un componente aggiuntivo per l'integrità del sistema, fare riferimento a [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648) .  
  
## <a name="installing-health-add-in-files"></a>Installazione dei file del componente aggiuntivo per lo stato del sistema  
 Dopo che lo sviluppatore ha creato i file XML, è necessario copiare tali file nella posizione appropriata sul server e sui computer client.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Per installare i file XML nel server  
  
1. Creare una nuova cartella denominata **MyHealthAddIn** nella cartella **%ProgramFiles%\Windows Server\Bin\Feature Definitions**. È possibile assegnare a tale cartella un nome qualsiasi. Si consiglia di denominare la cartella utilizzando lo stesso nome della funzione.  
  
2. Copiare i file Definition.xml e Definition.xml.config nella nuova cartella.  
  
3. Se sono stati creati file binari per condizioni o azioni, è inoltre necessario copiare tali file in **%ProgramFiles%\Windows Server\Bin**.  
  
   I computer client eseguono ogni 6 ore un'attività pianificata che inserisce i file XML nella posizione appropriata. È possibile forzare la sincronizzazione tra il computer client e il server eseguendo manualmente l'attività.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Per installare i file XML nel computer client  
  
1.  Aprire l'Utilità di pianificazione.  
  
2.  Eseguire **HealthDefintionUpdateTask** nell'Utilità di pianificazione.  
  
    > [!NOTE]
    >  Tale attività non esegue l'installazione dei file binari. È necessario copiare manualmente i file binari nella cartella **%ProgramFiles%\Windows Server\Bin** sul computer client.  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)