---
title: Panoramica della finestra di avvio in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 198d16cb-3d07-4706-be89-ad14a5f7dc47
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 63a161057f7068dcb9e02faa353270f0150200b4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310660"
---
# <a name="overview-of-the-launchpad-in-windows-server-essentials"></a>Panoramica della finestra di avvio in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

La finestra di avvio di Windows Server Essentials è una piccola applicazione installata in un computer al primo tentativo di connessione al server da parte del computer. La finestra di avvio offre agli utenti autenticati l'accesso a funzionalità chiave di Windows Server Essentials, inclusi i backup dei computer, file e file multimediali condivisi e il sito di Accesso Web remoto. Gli utenti possono accedere a queste funzionalità da computer appartenenti al dominio o computer non aggiunti al dominio. La finestra di avvio offre anche informazioni e notifiche in tempo reale sullo stato del computer. Gli amministratori possono usare la finestra di avvio per accedere al dashboard del server, anche se il computer non è connesso alla rete.  
  
 I produttori OEM e i fornitori di software indipendenti che sviluppano componenti aggiuntivi per Windows Server Essentials possono usare la finestra di avvio per estendere le funzionalità dei componenti aggiuntivi ai computer disponibili nella rete.  
  
 I sistemi operativi Windows seguenti supportano l'uso della finestra di avvio di Windows Server Essentials:  
  
- **Windows 8**: Tutte le edizioni.  
  
- **Windows 7**: Tutte le edizioni.  
- **Windows 10**: tutte le edizioni. 
  
  I sistemi operativi seguenti non supportano l'uso della finestra di avvio di Windows Server Essentials:  
  
- **Server aggiuntivi**: Non è possibile eseguire la finestra di avvio di Windows Server Essentials in computer aggiuntivi che eseguono un sistema operativo Windows Server.  
  
  Contenuto dell'argomento:  
  
- [Usare la finestra di avvio](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Launchpad)  
  
- [Usare la finestra di avvio con un computer Mac](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
##  <a name="use-the-launchpad"></a><a name="BKMK_Launchpad"></a>Usare la finestra di avvio  
 Nella finestra di avvio di Windows Server Essentials sono disponibili i collegamenti e le informazioni seguenti.  
  
### <a name="backup"></a>Backup  
 Fare clic su **Backup** per aprire le **Proprietà backup** per il computer. Nella pagina **Proprietà backup** è possibile eseguire le operazioni seguenti:  
  
- Avviare o arrestare un backup.  
  
- Visualizzare lo stato e i dettagli del backup più recente.  
  
- Specificare come gestire il risparmio energia del computer quando è in esecuzione il backup.  
  
  Per informazioni su come usare Launchpad per eseguire il backup del computer, vedere [Manage client backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
### <a name="remote-web-access"></a>Accesso Web remoto  
 Fare clic su **Accesso Web remoto** per aprire il Web browser e visualizzare il sito Accesso Web remoto. Il sito Accesso Web remoto permette di connettersi ad altri computer e di accedere ad alcune risorse di rete quando ci si trova in ufficio o da qualsiasi posizione remota con un computer abilitato a Internet. Per ulteriori informazioni sulle Accesso Web remote, vedere [gestire accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Cartelle condivise  
 Fare clic su **Cartelle condivise** per aprire Esplora risorse e visualizzare il percorso delle cartelle condivise sul server. Per informazioni sulla condivisione di file e cartelle, vedere l'argomento [Manage Server Folders](Manage-Server-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="dashboard"></a>Dashboard  
 Fare clic su  **Dashboard** per aprire la pagina **Accedi** e accedere al dashboard di Windows Server Essentials. Dopo l'accesso, sarà visualizzata una connessione di Desktop remoto al dashboard del server. Per altre informazioni sul dashboard, vedere [Cenni preliminari sul dashboard](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Per usare questa funzionalità, è necessario avere l'accesso o le autorizzazioni corrette per l'accesso al server.  
  
### <a name="microsoft-office-365"></a>Microsoft Office 365  
 Il collegamento **Microsoft Office 365** è visualizzato nella finestra di avvio solo se l'utente ha un account Office 365. Fare clic su  **Microsoft Office 365** per accedere a collegamenti aggiuntivi alle risorse di Office 365. Per ulteriori informazioni, vedere [Guida introduttiva all'utilizzo Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Avvisi sullo stato del computer  
 Gli avvisi visualizzati nella finestra di avvio offrono un riepilogo sullo stato immediato del computer. Per visualizzare le informazioni su un avviso di stato, fare clic su un indicatore di avviso per aprire il Visualizzatore avvisi. Gli avvisi di stato sono visualizzati nel visualizzatore in base al livello di gravità. Gli avvisi più gravi sono visualizzati per primi nell'elenco, mentre gli avvisi meno gravi sono visualizzati nella parte inferiore dell'elenco. Per ulteriori informazioni sugli avvisi di integrità del computer, vedere [Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
##  <a name="use-the-launchpad-with-a-mac-computer"></a><a name="BKMK_Mac"></a>Usare la finestra di avvio con un computer Mac  
 È possibile connettere un computer Mac® che esegue Mac OS X® 10,5 o versione successiva a Windows Server Essentials, Windows Server Essentials o Windows Server 2012 R2 oppure scaricando e installando il software connettore. Al termine dell'installazione del software connettore, è possibile scegliere di avviare automaticamente la finestra di avvio all'avvio del computer.  
  
 La finestra di avvio è una piccola applicazione che offre agli utenti autenticati l'accesso a funzionalità chiave del server, inclusi file e file multimediali condivisi, componenti aggiuntivi e Accesso Web remoto. La finestra di avvio offre anche informazioni e notifiche in tempo reale sullo stato del computer.  
  
> [!NOTE]
>  Gli amministratori di server non possono usare la finestra di avvio o Accesso Web remoto in un computer Mac per aprire il dashboard del server e gestire il server.  
  
### <a name="backup"></a>Backup  
 Fare clic su **Backup** per configurare Time Machine per l'esecuzione del backup del computer e per modificare le impostazioni di Time Machine. Per altre informazioni su Time Machine, vedere la documentazione fornita dal produttore del computer.  
  
### <a name="remote-web-access"></a>Accesso Web remoto  
 Fare clic su **accesso Web remoto** per aprire il Web browser al sito accesso Web remoto. La Accesso Web remota consente di accedere ai file e alle cartelle condivise nel server da qualsiasi posizione remota con un computer abilitato per Internet. È possibile caricare file, riprodurre musica e video con il lettore multimediale basato sul Web, visualizzare immagini e riprodurre presentazioni. Per ulteriori informazioni, vedere [utilizzare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Cartelle condivise  
 Fare clic su **Cartelle condivise** per aprire lo strumento di ricerca e visualizzare il percorso delle cartelle condivise sul server. Per informazioni sulla condivisione di file e cartelle, vedere [use Shared Folders](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Avvisi sullo stato del computer  
 Gli avvisi visualizzati nella finestra di avvio offrono un riepilogo sullo stato immediato del computer. Per visualizzare le informazioni su un avviso di stato, fare clic su un indicatore di avviso per aprire il Visualizzatore avvisi. Gli avvisi di stato sono visualizzati nel visualizzatore in base al livello di gravità. Gli avvisi più gravi sono visualizzati per primi nell'elenco. Gli avvisi meno gravi sono visualizzati nella parte inferiore dell'elenco.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Connessione](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
