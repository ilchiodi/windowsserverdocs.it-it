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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-applications-in-windows-server-essentials"></a>Gestire le applicazioni in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Il Dashboard del server in Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato consente di eseguire comuni attività amministrative. Per eseguire queste attività, vedere gli argomenti seguenti:  
  
-   [Attività di gestione dell'applicazione nel Dashboard di](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Installare o rimuovere i componenti aggiuntivi tramite il Dashboard](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="BKMK_1"></a>Attività di gestione dell'applicazione nel Dashboard di  
 Il **applicazioni** gestione pagina del Dashboard contiene:  
  
-   Un elenco di componenti aggiuntivi installati, che visualizza:  
  
    -   Il nome del servizio online o del componente aggiuntivo  
  
    -   Lo stato di aggiornamento del componente aggiuntivo  
  
    -   Stato della sottoscrizione del componente aggiuntivo  
  
    -   Il nome della società o entità di pubblicazione che rende il componente aggiuntivo disponibile  
  
-   Un riquadro attività che include una serie di attività per la gestione di un componente aggiuntivo selezionato  
  
-   Un elenco di componenti aggiuntivi disponibili scaricare e installare in Microsoft Pinpoint  
  
 La tabella seguente descrive le varie attività di Gestione componenti aggiuntivi disponibili nel Dashboard del server. Alcune delle attività sono specifiche del componente aggiuntivo, in modo sono visibili solo quando si seleziona un componente aggiuntivo nell'elenco.  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|Rimuovere il componente aggiuntivo|Rimuove il componente aggiuntivo selezionato dal server e da tutti gli altri computer nella rete.|  
|Installare il componente aggiuntivo nel computer di rete|Consente di pianificare l'installazione del componente aggiuntivo selezionato in tutti gli altri computer nella rete.|  
|Ottenere assistenza per il componente aggiuntivo|Apre il browser Internet in un sito Web da cui si possono cercare le soluzioni ai problemi e ulteriori informazioni su un componente aggiuntivo selezionato.|  
|Aggiornare il componente aggiuntivo|Consente di scaricare e installare gli aggiornamenti per i componenti aggiuntivi installati nel computer server e rete.|  
|Rinnovare la sottoscrizione del componente aggiuntivo|Apre il browser Internet in un sito Web da cui è possibile rinnovare la sottoscrizione del componente aggiuntivo.|  
|Leggere l'informativa sulla privacy per il componente aggiuntivo|Apre il browser Internet in un sito Web da cui è possibile visualizzare l'informativa sulla privacy.|  
|Come installare o rimuovere i componenti aggiuntivi?|Apre il browser Internet in una pagina web che visualizza l'argomento della Guida.|  
  
##  <a name="BKMK_2"></a>Installare o rimuovere i componenti aggiuntivi tramite il Dashboard  
 Un componente aggiuntivo è un'applicazione software che fornisce funzionalità e caratteristiche aggiuntive per il server. Un numero crescente di componenti aggiuntivi è disponibile da Microsoft e altri fornitori di Software indipendenti (ISV).  
  
 Prima di trarre vantaggio dalle funzionalità estese che un componente aggiuntivo offre, è necessario prima installare il componente aggiuntivo nel server.  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Per installare un componente aggiuntivo da Microsoft Pinpoint  
  
1.  Nel Dashboard del server, fare clic su **applicazioni**, quindi fare clic su di **Microsoft Pinpoint** scheda.  Viene visualizzato un elenco dei componenti aggiuntivi disponibili.  
  
2.  Scegliere il componente aggiuntivo che si desidera installare. Viene visualizzata la pagina di informazioni di componente aggiuntivo.  
  
3.  Nella pagina delle informazioni di componente aggiuntivo, fare clic su Download e seguire le istruzioni visualizzate per scaricare e installare il componente aggiuntivo.  
  
4.  Seguire le istruzioni della procedura guidata per installare il componente aggiuntivo.  
  
5.  Al termine dell'installazione, riavviare il Dashboard, aprire il **applicazioni** pagina del Dashboard del server e verificare che il componente aggiuntivo viene visualizzato nella visualizzazione elenco.  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>Per installare un componente aggiuntivo da un altro provider  
  
1.  Aprire Esplora risorse e passare al percorso del file di installazione di componenti.  
  
2.  Fare doppio clic sul file per eseguire l'installazione guidata.  
  
3.  Seguire le istruzioni della procedura guidata per installare il componente aggiuntivo.  
  
4.  Al termine dell'installazione, riavviare il Dashboard, aprire il **applicazioni** pagina e quindi verificare che il componente aggiuntivo viene visualizzato nella visualizzazione elenco.  
  
#### <a name="to-remove-an-add-in"></a>Per rimuovere un componente aggiuntivo  
  
1.  Aprire il Dashboard del server.  
  
2.  Fare clic su di **applicazioni** scheda.  
  
3.  Nel **componenti aggiuntivi** scheda, selezionare il componente aggiuntivo che si desidera rimuovere, quindi fare clic su **rimuovere il componente aggiuntivo**.  
  
4.  Nel **rimuovere componenti** finestra, fare clic su **rimuovere**.  
  
    > [!NOTE]
    >  Si potrebbe essere necessario riavviare il Dashboard per rimuovere completamente il componente aggiuntivo.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica del dashboard](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
