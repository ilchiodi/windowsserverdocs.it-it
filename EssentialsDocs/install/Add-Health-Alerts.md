---
title: "Aggiunta di avvisi di integrità"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf0c062b92c687f5f7b33b419eafdca2dd3bbbfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-health-alerts"></a>Aggiunta di avvisi di integrità

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Un componente aggiuntivo di stato fornisce le definizioni per avvisi, controlli di integrità e correzioni per problemi di rete. Un componente aggiuntivo di integrità costituito da file xml che annotano il codice o i dati utilizzati per valutare le informazioni sull'integrità per una funzionalità specifica. Componenti aggiuntivi di integrità sono creati dagli sviluppatori e installati nel computer server e client dall'amministratore.  
  
 Consultare il [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648) per informazioni dettagliate sulla creazione di un componente aggiuntivo di integrità.  
  
## <a name="installing-health-add-in-files"></a>Installazione dello stato dei file di componenti aggiuntivi  
 Dopo che lo sviluppatore ha creato i file xml, è necessario inserire una copia dei file nella posizione appropriata nel server e i computer client.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Per installare i file xml nel server di  
  
1.  Nel **%ProgramFiles%\Windows definizioni myhealthaddin** cartella, creare una nuova cartella denominata **Server\bin\feature**. È possibile assegnare a questa cartella qualsiasi nome. È consigliabile che il nome della cartella di essere lo stesso nome della funzione.  
  
2.  Copiare il Definition.xml e i file Definition.xml. config nella nuova cartella.  
  
3.  Se hai creato un file binari per condizioni o azioni, è inoltre necessario copiare tali file disponibili per **%ProgramFiles%\Windows Server\Bin**.  
  
 I computer client eseguono un'attività pianificata ogni 6 ore che inserisce i file XML nella posizione appropriata. È possibile forzare la sincronizzazione tra il computer client e il server eseguendo manualmente l'attività.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Per installare i file xml nel computer client  
  
1.  Aprire l'utilità di pianificazione.  
  
2.  Eseguire il **HealthDefintionUpdateTask** nell'utilità di pianificazione.  
  
    > [!NOTE]
    >  Questa attività non installa i file binari. È necessario copiare manualmente i file binari di **%ProgramFiles%\Windows Server\Bin** cartella nel computer client.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)