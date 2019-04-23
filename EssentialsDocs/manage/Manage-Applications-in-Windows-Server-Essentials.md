---
title: Gestire le applicazioni in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e98d661ac71697bc0e38b6a25fe2f9d2b0b7254f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860172"
---
# <a name="manage-applications-in-windows-server-essentials"></a>Gestire le applicazioni in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Il Dashboard del server in Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato rende possibile eseguire attività amministrative comuni. Per eseguire queste attività, vedere gli argomenti seguenti:  
  
-   [Attività di gestione dell'applicazione nel Dashboard](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Installare o rimuovere componenti aggiuntivi tramite il Dashboard](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="BKMK_1"></a> Attività di gestione dell'applicazione nel Dashboard  
 La pagina di gestione **Applicazioni** del dashboard contiene:  
  
-   Un elenco di componenti aggiuntivi installati, che visualizza:  
  
    -   Il nome del servizio online o del componente aggiuntivo  
  
    -   Lo stato di aggiornamento del componente aggiuntivo  
  
    -   Lo stato della sottoscrizione del componente aggiuntivo  
  
    -   Il nome della società o dell'entità di pubblicazione che rende disponibile il componente aggiuntivo  
  
-   Un riquadro attività che include una serie di attività per la gestione di un componente aggiuntivo selezionato  
  
-   Un elenco di componenti aggiuntivi disponibili per il download e l'installazione in Microsoft Pinpoint  
  
 La tabella seguente descrive le varie attività di gestione dei componenti aggiuntivi disponibili nel dashboard del server. Alcune attività sono specifiche del componente aggiuntivo, quindi sono visibili solo se si seleziona un componente aggiuntivo nell'elenco.  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|Rimuovi il componente aggiuntivo|Rimuove il componente aggiuntivo selezionato dal server e da tutti gli altri computer della rete.|  
|Installa il componente aggiuntivo sui computer della rete|Consente di pianificare l'installazione del componente aggiuntivo selezionato in tutti i computer della rete.|  
|Ottieni assistenza per il componente aggiuntivo|Apre il browser Internet in un sito Web in cui è possibile cercare le soluzioni ai problemi e acquisire altre informazioni su un componente aggiuntivo selezionato.|  
|Aggiorna componente aggiuntivo|Consente di scaricare e installare aggiornamenti per i componenti aggiuntivi installati nel server e nei computer della rete.|  
|Rinnova la sottoscrizione del componente aggiuntivo|Apre il browser Internet in un sito Web in cui è possibile rinnovare la sottoscrizione del componente aggiuntivo.|  
|Leggere l'informativa sulla privacy per il componente aggiuntivo|Apre il browser Internet in un sito Web in cui è possibile visualizzare l'informativa sulla privacy.|  
|Installazione o rimozione dei componenti aggiuntivi|Apre il browser Internet in una pagina Web che visualizza l'argomento della Guida appropriato.|  
  
##  <a name="BKMK_2"></a> Installare o rimuovere componenti aggiuntivi tramite il Dashboard  
 Un componente aggiuntivo è un'applicazione software che fornisce funzionalità e caratteristiche aggiuntive per il server. È disponibile un numero crescente di componenti aggiuntivi di Microsoft e di altri fornitori di software indipendenti (ISV).  
  
 Per trarre vantaggio dalle funzionalità estese offerte da un componente aggiuntivo, è necessario prima installarlo nel server.  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Per installare un componente aggiuntivo da Microsoft Pinpoint  
  
1.  Nel dashboard del server fare clic su **Applicazioni** e quindi sulla scheda **Microsoft Pinpoint**.  Verrà visualizzato l'elenco di componenti aggiuntivi disponibili.  
  
2.  Fare clic sul componente aggiuntivo da installare. Verrà visualizzata la pagina di informazioni sul componente aggiuntivo.  
  
3.  In questa pagina fare clic su Download e seguire le istruzioni visualizzate per scaricare e installare il componente aggiuntivo.  
  
4.  Seguire le istruzioni della procedura guidata per installare il componente aggiuntivo.  
  
5.  Al termine dell'installazione, riavviare il dashboard, aprire la pagina **Applicazioni** del dashboard del server e verificare se il componente aggiuntivo è riportato nella visualizzazione elenco.  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>Per installare un componente aggiuntivo di un altro fornitore  
  
1.  Aprire Esplora risorse e individuare il percorso del file di installazione del componente aggiuntivo.  
  
2.  Fare doppio clic sul file per eseguire l'installazione guidata.  
  
3.  Seguire le istruzioni della procedura guidata per installare il componente aggiuntivo.  
  
4.  Al termine dell'installazione, riavviare il dashboard, aprire la pagina **Applicazioni** e verificare se il componente aggiuntivo è riportato nella visualizzazione elenco.  
  
#### <a name="to-remove-an-add-in"></a>Per rimuovere un componente aggiuntivo  
  
1.  Aprire il dashboard del server.  
  
2.  Fare clic sulla scheda **Applicazioni**.  
  
3.  Nella scheda **Componenti aggiuntivi** selezionare il componente aggiuntivo da rimuovere e quindi fare clic su **Rimuovi il componente aggiuntivo**.  
  
4.  Nella finestra **Rimuovi il componente aggiuntivo** fare clic su **Rimuovi**.  
  
    > [!NOTE]
    >  Può essere necessario riavviare il dashboard per rimuovere completamente il componente aggiuntivo.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Informazioni generali sul dashboard](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
