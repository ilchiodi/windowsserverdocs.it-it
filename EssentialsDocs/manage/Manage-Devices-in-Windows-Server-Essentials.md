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
ms.openlocfilehash: a66f98b0896e706f520aa057b91cce2fe662d22d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433327"
---
# <a name="manage-devices-in-windows-server-essentials"></a>Gestire i dispositivi in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Le sezioni seguenti descrivono le funzionalità di gestione dei dispositivi di un server e illustrano come configurare e usare i dispositivi nella rete:  
  
-   [Gestire i dispositivi tramite il Dashboard](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Assegnare account utente l'autorizzazione per accedere a specifici computer della rete](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Rimuovere un computer dal server](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Configurare le impostazioni di criteri di gruppo per Reindirizzamento cartelle e sicurezza](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Connettersi a un computer di rete tramite una sessione Desktop remoto](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Visualizzare le proprietà di computer](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a> Gestire i dispositivi tramite il Dashboard  
 Windows Server Essentials permette di eseguire attività amministrative comuni tramite il dashboard di Windows Server Essentials. La pagina **Dispositivi** del dashboard contiene quanto segue:  
  
-   Un elenco dei computer di rete, che visualizza:  
  
    -   Il nome del computer  
  
    -   Lo stato del computer, ossia **Online** o **Offline**  
  
    -   La descrizione del computer  
  
    -   Lo stato di backup del computer  
  
    -   Lo stato di aggiornamento del computer  
  
    -   Lo stato di sicurezza del computer  
  
    -   Lo stato degli avvisi del computer  
  
    -   Informazioni su Criteri di gruppo per il computer  
  
-   Un riquadro dei dettagli con altre informazioni su un computer selezionato  
  
-   Un riquadro attività che include una serie di attività amministrative dei dispositivi, ad esempio la visualizzazione di proprietà e avvisi del computer, la configurazione del backup del computer e il ripristino di file e cartelle da un backup  
  
#### <a name="to-view-the-status-of-network-computers"></a>Per visualizzare lo stato dei computer di rete  
  
1. Aprire il dashboard di Windows Server Essentials.  
  
2. Sulla barra di spostamento fare clic su **Dispositivi**.  
  
3. Visualizzare lo stato di tutti i computer della rete nel riquadro elenco.  
  
   La tabella seguente descrive le varie attività relative a computer e backup disponibili nel dashboard di Windows Server Essentials. Alcune attività sono specifiche del computer e sono visibili solo se si seleziona un computer nell'elenco.  
  
### <a name="computer-tasks-in-the-dashboard"></a>Attività relative ai computer nel dashboard  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|Visualizza proprietà del computer|Visualizza informazioni generali su un computer selezionato e consente di visualizzare i dettagli sui backup del computer.|  
|Imposta backup del computer|Esegue la procedura guidata Imposta backup.|  
|Personalizza backup del computer|Apre le proprietà di backup, in cui è possibile apportare modifiche alle impostazioni del backup per il computer selezionato.|  
|Avvia un backup del computer|Avvia un backup del computer selezionato.|  
|Arresta backup del computer|Arresta il backup del computer selezionato.|  
|Ripristino file o cartelle del computer|Esegue la procedura guidata Ripristino file o cartelle, che consente di ripristinare specifici file, cartelle o unità.|  
|Visualizza avvisi del computer|Visualizza avvisi critici e informativi e consente di intraprendere le azioni correttive, laddove possibile.|  
|Desktop remoto per il computer|Apre Connessione Desktop remoto per il computer selezionato.|  
|Rimuovi il computer|Esegue la procedura guidata Rimuovi un computer, che scollega il computer dal dashboard di Windows Server Essentials.|  
|Personalizza le impostazioni di Backup computer e Cronologia file|Apre la pagina di impostazioni del backup, in cui è possibile apportare modifiche alla pianificazione dei backup e alle impostazioni di Cronologia file per i computer client.|  
|Connessione dei computer al server|Apre un argomento della Guida che descrive i passaggi da eseguire per aggiungere un computer alla rete.|  
|Implementa criteri di gruppo|Applica le impostazioni di criteri ai computer Windows 8 e Windows 7 aggiunti al dominio.|  
  
##  <a name="BKMK_2"></a> Assegnare account utente l'autorizzazione per accedere a specifici computer della rete  
 È possibile assegnare agli account utente le autorizzazioni per consentire agli utenti di accedere solo a specifici computer di rete quando accedono alla rete di Windows Server Essentials da un percorso remoto.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Per cambiare l'accesso di un account utente ai computer  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente selezionare quello da modificare.  
  
4.  Nel **< Account utente\> attività** riquadro, fare clic su **Visualizza proprietà account**. Sarà visualizzata la pagina **Proprietà** per l'account utente.  
  
5.  Nella scheda **Accesso computer** selezionare il computer a cui l'utente può accedere in remoto e quindi fare clic su **OK**.  
  
##  <a name="BKMK_3"></a> Rimuovere un computer dal server  
 I computer rimossi da un server che esegue Windows Server Essentials tramite il dashboard non vengono più gestiti dal server. Di conseguenza, il server non creerà più i backup del computer e non ne monitorerà l'integrità dopo la rimozione dalla rete.  
  
> [!NOTE]
>  Con la rimozione dal server, il computer non viene disconnesso dalla rete ma può ancora accedere alle risorse della rete come quando era connesso al server. Per impedire al computer di accedere alle risorse del server e per disconnetterlo dal server, è necessario rimuovere il computer dal dominio. Inoltre, la rimozione del computer dal server non comporta la disinstallazione automatica del software Connettore o della finestra di avvio dal computer rimosso. È necessario rimuovere manualmente il software Connettore dal computer. Per altre informazioni, vedere la sezione di disinstallare il software connettore nel [Connettiti](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Per rimuovere un computer dalla rete tramite il dashboard  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic sulla scheda **Dispositivi** .  
  
3.  Nell'elenco di computer fare clic con il pulsante destro del mouse sul computer da rimuovere dalla rete e quindi scegliere **Rimuovi il computer**.  
  
##  <a name="BKMK_5"></a> Configurare le impostazioni di criteri di gruppo per Reindirizzamento cartelle e sicurezza  
 È possibile configurare Criteri di gruppo e distribuirli nei computer della rete di Windows Server Essentials tramite il dashboard di Windows Server Essentials. Criteri di gruppo in Windows Server Essentials includono le impostazioni per il reindirizzamento e la sicurezza delle cartelle che influiscono su Windows Update, su Windows Defender e sul firewall della rete.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Per configurare Criteri di gruppo in Windows Server Essentials  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **DISPOSITIVI**.  
  
3.  Per Windows Server Essentials: Nel riquadro **Attività utente** globale fare clic su **Implementa criteri di gruppo**.  
  
     Per Windows Server Essentials: Nel riquadro **Attività dispositivi** globale fare clic su **Implementa criteri di gruppo**.  
  
4.  Verrà aperta la procedura guidata Implementa criteri di gruppo.  
  
5.  Nella pagina **Abilita criteri di gruppo per reindirizzamento cartelle** della procedura guidata è possibile scegliere le cartelle utente da reindirizzare.  
  
6.  Nella pagina **Abilita impostazioni dei criteri di sicurezza** della procedura guidata è possibile scegliere di abilitare le impostazioni di Criteri di gruppo per **Windows Update**, **Windows Defender**e **Firewall di rete**.  
  
7.  Fare clic su **Fine** per implementare le impostazioni di Criteri di gruppo.  
  
##  <a name="BKMK_7"></a> Connettersi a un computer di rete tramite una sessione Desktop remoto  
 Per accedere in remoto il computer di rete di Windows Server Essentials quando si è fuori sede, usare il Web browser accedere al sito Web accesso Web remoto l'organizzazione e nella **computer** scheda, fare clic sul nome del computer.  
  
 La colonna **Stato** mostra se è possibile connettersi a un computer nella rete e può includere i valori seguenti:  
  
-   **Disponibile**  
  
     Il computer è attivato e disponibile per una connessione remota. Anche se questo stato è visualizzato, è possibile che non si riesca comunque a connettersi a questo computer se la connessione è bloccata da un firewall di terze parti.  
  
-   **Offline o inattivo**  
  
     Il computer è disattivato o si trova in modalità sospensione o ibernazione. Se un computer è offline o inattivo, lo stato sarà aggiornato in tempo reale, per permettere di sapere quando il computer diventa disponibile.  
  
-   **Sistema operativo non supportato**  
  
     Il sistema operativo del computer non supporta Desktop remoto. In caso di modifica, potrebbero essere necessarie fino a sei ore per l'aggiornamento di questo stato sul server.  
  
-   **Connessione disabilitata**  
  
     La connessione al computer è bloccata da un firewall o da Criteri di gruppo oppure il desktop remoto è disabilitato nel computer. In caso di modifica, potrebbero essere necessarie fino a sei ore per l'aggiornamento di questo stato sul server.  
  
##  <a name="BKMK_8"></a> Visualizzare le proprietà di computer  
 La sezione **Dispositivi** del dashboard di Windows Server Essentials visualizza un elenco di computer della rete. L'elenco include anche informazioni aggiuntive su ogni computer.  
  
#### <a name="to-view-a-list-of-computers"></a>Per visualizzare un elenco di computer  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento principale fare clic su **Dispositivi**.  
  
3.  Il dashboard visualizza l'elenco corrente di computer.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Per visualizzare o modificare le proprietà di un computer  
  
1.  Nell'elenco di computer selezionare l'account per cui visualizzare o modificare le proprietà.  
  
2.  Nel **< Computername\> attività** riquadro, fare clic su **Visualizza proprietà del computer**. Verrà visualizzata la pagina **Proprietà** dei computer.  
  
3.  Fare clic su una scheda per visualizzare le proprietà del computer specifico.  
  
4.  Per salvare eventuali modifiche apportate alle proprietà del computer, fare clic su **Applica**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire gli account utente usando il Dashboard](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
