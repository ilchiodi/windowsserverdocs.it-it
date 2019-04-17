---
title: Risolvere i problemi relativi al monitoraggio di computer in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 72fe309e0e7ce6d7227cce8b7f2c5dbf018eb4a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Risolvere i problemi relativi al monitoraggio di computer in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questo argomento contiene informazioni sulla risoluzione dei problemi riscontrati durante il monitoraggio dello stato del computer nel Visualizzatore avvisi e tramite le notifiche di posta elettronica in Windows Server Essentials.  
  
> [!NOTE]
>  Per informazioni aggiornate sulla risoluzione dei problemi dalla community Windows Server Essentials, si consiglia di visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Forum di Windows Server Essentials è un ottimo punto per cercare informazioni o per porre una domanda.  
  
##  <a name="BKMK_TS"></a>Risoluzione dei problemi relativi a notifiche di posta elettronica per gli avvisi  
 Questa sezione sono elencati i vari problemi che possono verificarsi quando si utilizzano le notifiche di posta elettronica per gli avvisi.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>Impossibile inviare la posta elettronica di prova per l'avviso  
 **Problema** viene visualizzato un errore messaggio, non possono inviare la posta elettronica di prova per avviso.  
  
 **Causa** questo errore potrebbe essere dovuto a uno dei seguenti problemi nelle impostazioni per le notifiche di avviso:  
  
-   Un nome server SMTP non corretto o numero di porta.  
  
-   È stato specificato erroneamente che il server SMTP richiede una connessione Single Sockets Layer (SSL).  
  
-   Il server SMTP richiede l'autenticazione e sono state immesse credenziali errate.  
  
 **Soluzioni** correggere eventuali errori nelle impostazioni delle notifiche di posta elettronica.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Per identificare i problemi nelle impostazioni delle notifiche di posta elettronica  
  
-   Chiedi a provider di servizi Internet (ISP) per il nome corretto del server SMTP, numero di porta e l'uso di SSL.  
  
-   Esaminare i file di log per le notifiche di posta elettronica per l'avviso sul server, in questo percorso:  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Per visualizzare la cartella ProgramData, sono stati nascosti gli elementi visualizzati. Se si tenere sempre presente la cartella ProgramData, sulla barra multifunzione s **visualizzazione** nella scheda il **Mostra/Nascondi** gruppo, selezionare il **gli elementi nascosti** casella di testo.  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Per aggiornare la configurazione delle notifiche di posta elettronica per gli avvisi  
  
1.  Nel Dashboard, fare clic su qualsiasi icona di avviso nell'angolo superiore destro per aprire il Visualizzatore avvisi.  
  
2.  Nella parte inferiore del Visualizzatore avvisi, fare clic su **configura notifiche di posta elettronica per avvisi**.  
  
3.  Nel **configura notifiche di posta elettronica per avvisi** la finestra di dialogo, fare clic su **abilitare**.  
  
4.  Nel **impostazioni SMTP** aggiornare le impostazioni SMTP, nella finestra di dialogo e quindi fare clic su **OK**.  
  
5.  Per testare le impostazioni aggiornate, fare clic su **applica e invia posta elettronica**.  
  
6.  Dopo avere verificato che il messaggio di prova è stato completato, fare clic su **OK** per salvare le impostazioni aggiornate.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>Notifica di posta elettronica di prova non elenca tutti gli avvisi  
 **Problema** le notifiche di posta elettronica di test per gli avvisi non contengono avvisi, anche se sono presenti avvisi elencati nel Visualizzatore avvisi.  
  
 **Soluzione** non tutti gli avvisi segnalati nel Visualizzatore avvisi generano una notifica di posta elettronica. Solo gli avvisi che sono configurati per eseguire l'escalation come notifica di posta elettronica entro i file di definizione dello stato vengono inviati come messaggi di posta elettronica ai destinatari di posta elettronica specificati.  
  
 Quando si fa clic **applica e invia posta elettronica**, in genere si riceverà una notifica di posta elettronica di esempio con senza che siano elencati avvisi. Tuttavia, se durante il processo di test viene identificato un avviso di stato configurato per inviare notifiche tramite posta elettronica, tale avviso sarà incluso nell'e-mail di test.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Vengono visualizzati avvisi attivi per un'applicazione disinstallata  
 **Problema** vengono visualizzati avvisi attivi per un'applicazione, anche se l'applicazione e il relativo file di definizione di integrità sono stati disinstallati.  
  
 **Soluzione** è necessario eliminare manualmente gli avvisi attivi dell'applicazione disinstallata. Per eliminare un avviso, eseguire le operazioni seguenti.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Per eliminare un avviso dal server tramite il Dashboard  
  
1.  Dal server, aprire il Dashboard.  
  
2.  Nel riquadro di spostamento, fare clic su qualsiasi icona di avviso visualizzata (critico, avviso o informativo). Verrà avviato il Visualizzatore avvisi.  
  
3.  Nel Visualizzatore avvisi, fare doppio clic sull'avviso che si desidera eliminare e quindi fare clic su **Elimina avviso**.
