---
title: Gestire i dispositivi in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 288d62a3fe4d9073ba2c0e3fdff385d8317f20d4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-devices-in-windows-server-essentials"></a>Gestire i dispositivi in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Le sezioni seguenti illustrare le funzionalità di Gestione periferiche di un server e viene illustrato come configurare e utilizzare i dispositivi nella rete:  
  
-   [Gestire i dispositivi tramite il Dashboard](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Assegnare agli account utente l'autorizzazione per accedere a specifici computer della rete](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Rimuovere un computer dal server di](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Configurare le impostazioni di criteri di gruppo per la sicurezza e reindirizzamento cartelle](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Connettersi a un computer di rete tramite una sessione Desktop remoto](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Visualizzare le proprietà di computer](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>Gestire i dispositivi tramite il Dashboard  
 Windows Server Essentials consente di eseguire attività amministrative comuni tramite il Dashboard di Windows Server Essentials. Il **dispositivi** pagina del Dashboard contiene quanto segue:  
  
-   Un elenco dei computer di rete, che visualizza:  
  
    -   Il nome del computer  
  
    -   Lo stato del computer, entrambi **Online** o **Offline**  
  
    -   La descrizione del computer  
  
    -   Lo stato del backup del computer  
  
    -   Lo stato di aggiornamento del computer  
  
    -   Lo stato di protezione del computer  
  
    -   Lo stato degli avvisi del computer  
  
    -   Informazioni sui criteri di gruppo per il computer  
  
-   Un riquadro dei dettagli con altre informazioni su un computer selezionato  
  
-   Un riquadro attività che include una serie di attività amministrative del dispositivo, ad esempio la visualizzazione di avvisi e le proprietà del computer, configurazione del backup di computer e ripristino di file e cartelle da un backup  
  
#### <a name="to-view-the-status-of-network-computers"></a>Per visualizzare lo stato dei computer di rete  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **dispositivi**.  
  
3.  Visualizzare lo stato di tutti i computer nella rete nel riquadro elenco.  
  
 La tabella seguente descrive i vari computer e le attività di backup sono disponibili nel Dashboard di Windows Server Essentials. Alcune delle attività sono specifiche del computer e sono visibili solo quando si seleziona un computer nell'elenco.  
  
### <a name="computer-tasks-in-the-dashboard"></a>Attività di computer nel Dashboard  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|Visualizza proprietà del computer|Visualizza informazioni generali per un computer selezionato e consente di visualizzare i dettagli di backup del computer.|  
|Configurare il backup del computer|Esegue la procedura guidata imposta Backup.|  
|Personalizzare un backup per il computer|Apre le proprietà di backup, in cui è possibile apportare modifiche alle impostazioni del backup per il computer selezionato.|  
|Avviare un backup del computer|Avvia un backup per un computer selezionato.|  
|Arresta backup del computer|Arresta il backup del computer selezionato.|  
|Ripristinare file o cartelle per il computer|Esegue il ripristino di file e cartelle guidata, che consente di ripristinare specifici file, cartelle o unità.|  
|Visualizzare gli avvisi per il computer|Visualizza critici e avvisi informativi e consente di intraprendere azioni correttive, laddove possibile.|  
|Desktop remoto al computer|Apre connessione Desktop remoto al computer selezionato.|  
|Rimuovere il computer|Esegue l'installazione guidata un Computer, che consente di scollegare il computer dal Dashboard di Windows Server Essentials.|  
|Personalizza backup del computer e impostazioni di cronologia File|Apre la pagina Impostazioni di backup, da cui è possibile apportare modifiche alla pianificazione di backup e le impostazioni di cronologia File per i client computer.|  
|Come collegare i computer al server?|Apre un argomento della Guida che descrive i passaggi da eseguire per aggiungere un computer alla rete.|  
|Implementa criteri di gruppo|Applica le impostazioni di criteri ai computer Windows 8 e Windows 7 aggiunti al dominio.|  
  
##  <a name="BKMK_2"></a>Assegnare agli account utente l'autorizzazione per accedere a specifici computer della rete  
 È possibile assegnare autorizzazioni agli account utente in modo che gli utenti possano accedere ai soli computer di rete specifici durante l'accesso alla rete di Windows Server Essentials da una posizione remota.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Per modificare l'accesso al computer per un account utente  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera modificare.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **Visualizza proprietà account**. Il **proprietà** pagina per l'account utente viene visualizzato.  
  
5.  Nel **accesso Computer** scheda, selezionare il computer che l'utente può accedere in remoto e quindi fare clic su **OK**.  
  
##  <a name="BKMK_3"></a>Rimuovere un computer dal server di  
 Quando si rimuove un computer da un server che esegue Windows Server Essentials tramite il Dashboard, non è più gestito dal server. Di conseguenza, il server verrà interrompere i backup del computer o monitorare l'integrità dopo la rimozione dalla rete.  
  
> [!NOTE]
>  Rimozione di un computer dal server di scollegare il computer dalla rete. Il computer può comunque accedere alle risorse della rete nello stesso modo che è possibile prima di essere connessi al server. Per impedire al computer di accedere alle risorse di server e per disconnetterlo dal server, è necessario rimuovere il computer dal dominio. Inoltre, la rimozione del computer dal server di non Disinstalla automaticamente il software connettore o la finestra di avvio dal computer in cui è stato rimosso. È necessario rimuovere manualmente il software connettore dal computer. Per ulteriori informazioni, vedere la sezione di disinstallare il software connettore in [connessione](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Per rimuovere un computer dalla rete tramite il Dashboard  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su di **dispositivi** scheda.  
  
3.  Nell'elenco dei computer, fare doppio clic su computer che si desidera rimuovere dalla rete, quindi fare clic su **rimuovere il computer**.  
  
##  <a name="BKMK_5"></a>Configurare le impostazioni di criteri di gruppo per la sicurezza e reindirizzamento cartelle  
 È possibile configurare criteri di gruppo e distribuirlo ai computer della rete di Windows Server Essentials tramite il Dashboard di Windows Server Essentials. Criteri di gruppo in Windows Server Essentials includono le impostazioni per la sicurezza e reindirizzamento cartelle che influiscono su Windows Update, Windows Defender e il firewall di rete.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Per configurare criteri di gruppo in Windows Server Essentials  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento, fare clic su **dispositivi**.  
  
3.  Per Windows Server Essentials: In globale **attività utenti** riquadro, fare clic su **implementa criteri di gruppo**.  
  
     Per Windows Server Essentials: In globale **attività dispositivi** riquadro, fare clic su **implementa criteri di gruppo**.  
  
4.  Apre la procedura guidata criteri di gruppo implementa.  
  
5.  Nel **Abilita criteri di gruppo reindirizzamento cartelle** pagina della procedura guidata, è possibile scegliere le cartelle degli utenti che si desidera reindirizzare.  
  
6.  Nel **attiva le impostazioni di criteri di sicurezza** pagina della procedura guidata, è possibile scegliere di abilitare le impostazioni di criteri di gruppo per **Windows Update**, **Windows Defender**e **Firewall di rete**.  
  
7.  Fare clic su **fine** per implementare le impostazioni di criteri di gruppo.  
  
##  <a name="BKMK_7"></a>Connettersi a un computer di rete tramite una sessione Desktop remoto  
 Per accedere in remoto il computer di rete di Windows Server Essentials e si è fuori sede, usare il Web browser per accedere al sito Web accesso Web remoto dell'organizzazione s e scegliere il **computer** scheda, fare clic sul nome del computer.  
  
 Il **stato** colonna Mostra se è possibile connettersi a un computer della rete e può contenere i valori seguenti:  
  
-   **Disponibile**  
  
     Il computer è attivato ed è disponibile per una connessione remota. Anche se questo stato è visualizzato, è comunque potrebbe non essere in grado di connettersi al computer, se un firewall di terze parti che blocca la connessione.  
  
-   **Offline o inattivo**  
  
     Il computer è disattivato o si trova in modalità sospensione o ibernazione. Se un computer è offline o inattivo, lo stato viene aggiornato in tempo reale in modo da sapere quando il computer diventa disponibile.  
  
-   **Sistema operativo non supportato**  
  
     Il sistema operativo nel computer non supporta desktop remoto. Può richiedere fino a 6 ore per essere aggiornati nel server se è presente una modifica di questo stato.  
  
-   **Connessione disattivata**  
  
     La connessione al computer è bloccata da un firewall o il desktop remoto è disabilitato nel computer o tramite criteri di gruppo. Può richiedere fino a 6 ore per essere aggiornati nel server se è presente una modifica di questo stato.  
  
##  <a name="BKMK_8"></a>Visualizzare le proprietà di computer  
 Il **dispositivi** sezione del Dashboard di Windows Server Essentials Visualizza un elenco dei computer di rete. L'elenco include anche informazioni aggiuntive su ogni computer.  
  
#### <a name="to-view-a-list-of-computers"></a>Per visualizzare un elenco di computer  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento principale fare clic su **dispositivi**.  
  
3.  Il Dashboard visualizza un elenco aggiornato dei computer.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Per visualizzare o modificare le proprietà di un computer  
  
1.  Nell'elenco dei computer, selezionare l'account per cui si desidera visualizzare o modificare le proprietà.  
  
2.  Nel **< Computername\ > attività** riquadro, fare clic su **Visualizza proprietà del computer**. Il **proprietà** verrà visualizzata la pagina per i computer.  
  
3.  Fare clic su una scheda per visualizzare le proprietà per tale computer.  
  
4.  Per salvare le modifiche apportate alle proprietà del computer, fare clic su **applica**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire gli account utente tramite il Dashboard](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
