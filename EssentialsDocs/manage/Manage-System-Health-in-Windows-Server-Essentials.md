---
title: Gestire l'integrità del sistema in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d9002a1530e114f490ddf1cfb0e5706ddec52431
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371215"
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Gestire l'integrità del sistema in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Questo argomento illustra come visualizzare e rispondere a tutti gli avvisi della rete tramite il dashboard.  
  
> [!NOTE]
>  In Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, gli avvisi di integrità per i computer server e client nella rete non sono più visualizzati nel Visualizzatore avvisi, ma possono essere visualizzati nella scheda rapporti di **stato** della **Home** page.  
  
 Windows Server Essentials monitora attivamente ogni computer connesso al server e avvisa l'amministratore dei problemi relativi all'integrità del sistema, inclusi gli aggiornamenti critici, la mancanza di malware Protection, le definizioni di virus non aggiornate sul client computer e altri problemi importanti che richiedono un'azione. Questi problemi vengono visualizzati come avvisi nel Visualizzatore avvisi, che può essere avviato dal dashboard del server o dalla finestra di avvio del computer client in Windows Server Essentials o nella scheda **rapporti di stato** in Windows Server Essentials. Per impostazione predefinita, gli avvisi sono aggiornati ogni trenta minuti, ma è possibile verificare la presenza di avvisi di rete in qualsiasi momento facendo clic su **Aggiorna** nel Visualizzatore avvisi oppure nella scheda **Rapporti di stato**.  
  
 Gli argomenti seguenti permettono di comprendere, visualizzare e rispondere agli avvisi del Visualizzatore avvisi e offrono anche istruzioni per configurare il server per la ricezione di notifiche di avviso tramite posta elettronica:  
  
-   [Informazioni sul componente aggiuntivo rapporto di stato](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Visualizzare gli avvisi tramite il Visualizzatore avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organizzare gli avvisi nel Visualizzatore avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Rispondere agli avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurare le notifiche di posta elettronica per gli avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Avvisi potenziali del computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>Informazioni sul componente aggiuntivo rapporto di stato  
 Il componente aggiuntivo Rapporto di stato per Windows Server Essentials offre informazioni consolidate sulla rete di Windows Server Essentials e permette di distribuire queste informazioni ad altri utenti. Le informazioni possono essere visualizzate nella scheda **Rapporti** del dashboard. Nella scheda **Rapporti** è possibile eseguire le operazioni seguenti:  
  
-   [Generare un report su richiesta o in base a una pianificazione](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personalizzare il contenuto del report](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [Invia tramite posta elettronica il report](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:** È possibile scaricare il componente aggiuntivo rapporto di stato per Windows Server Essentials dall' [area download Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials:** Per impostazione predefinita, il componente aggiuntivo rapporto di stato è integrato con Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato e i report sull'integrità vengono visualizzati nella scheda **rapporti di stato** della **Home** page del dashboard.  
  
###  <a name="BKMK_Generate"></a>Generare un report su richiesta o in base a una pianificazione  
 Dopo l'installazione del componente aggiuntivo Rapporto di stato e il riavvio del dashboard, una nuova scheda **Rapporti** sarà aggiunta al dashboard. È possibile generare un rapporto di stato su richiesta in qualsiasi momento, facendo clic sull'attività **Genera rapporto di stato** nella scheda **Rapporti**.  
  
 Dopo la generazione di un rapporto di stato, un nuovo elemento sarà creato nel riquadro elenco, identificato dalla data e dall'ora di generazione del rapporto. Per aprire un elemento, farvi doppio clic nel riquadro elenco oppure selezionarlo e fare clic su **Apri il rapporto di stato** nel riquadro attività. Il rapporto sarà visualizzato in una nuova finestra in formato HTML.  
  
 Oltre a generare manualmente un rapporto, è possibile che si voglia che il rapporto sia generato automaticamente in base a una pianificazione giornaliera oppure oraria. A tale scopo, nel riquadro attività fare clic su **Personalizza impostazioni rapporto di stato**, quindi fare clic sulla scheda **Pianifica e invia tramite posta elettronica** . La funzionalità **pianificazione** è disattivata per impostazione predefinita ed è possibile attivarla selezionando la casella di controllo **Genera rapporto di stato alla relativa ora pianificata** .  
  
###  <a name="BKMK_Customize"></a>Personalizzare il contenuto del report  
 Il rapporto di stato include gli elementi seguenti:  
  
- **Avvisi critici e normali** Si tratta degli stessi elementi disponibili nel Visualizzatore avvisi nel dashboard. Gli avvisi informativi non sono inclusi nel rapporto di stato.  
  
- **Errori critici dei registri eventi** Le applicazioni e i registri dei servizi vengono analizzati e gli errori registrati nelle ultime 24 ore saranno inclusi nella sezione **Dettagli** del rapporto.  
  
- **Backup del server** Le informazioni sul backup più recente del server sono disponibili nella sezione **Dettagli** del rapporto.  
  
- **Servizi ad avvio automatico non in esecuzione** Se un servizio ad avvio automatico non è in esecuzione nel momento in cui è generato il rapporto, le informazioni su questo servizio saranno disponibili nella sezione **Dettagli** del rapporto.  
  
- **Aggiornamenti** È possibile visualizzare lo stato di aggiornamento del server e di tutti i computer client nella sezione **Dettagli**.  
  
- **Archiviazione** L'elenco di unità e delle rispettive capacità è disponibile nella sezione **Dettagli**.  
  
  Nel Rapporto di stato è necessario visualizzare prima di tutto il **Riepilogo**, quindi fare clic sul collegamento **Dettagli** sulla stessa riga degli elementi associati a un'icona di errore rossa o a un'icona di avviso gialla per visualizzare i dettagli specifici.  
  
  Se non si è interessati ad alcuni punti dati inclusi nel report per impostazione predefinita, è possibile personalizzare il contenuto del report facendo clic su **Personalizza le impostazioni del rapporto di stato** nel riquadro attività, quindi facendo clic sulla scheda **contenuto** . deselezionare le caselle di controllo per il contenuto che non si desidera visualizzare nel report. Se, ad esempio, si dispone di un piano di backup del server e non si desidera visualizzare gli avvisi relativi ai backup del server, è possibile escludere i backup del server dal report deselezionando la casella di controllo **backup server** .  
  
###  <a name="BKMK_emailreport"></a>Invia tramite posta elettronica il report  
 Accedere al dashboard per leggere i rapporti risulta scomodo per alcuni amministratori, in particolare se devono gestire più server. Se si attiva la funzionalità relativa alla posta elettronica, un messaggio di posta elettronica che include il contenuto del rapporto sarà inviato a un elenco di indirizzi di posta elettronica specificati dopo la generazione di un rapporto. L'amministratore può visualizzare con facilità questo rapporto da qualsiasi dispositivo o applicazione client, in modo da verificare che il server sia in esecuzione con lo stato ottimale.  
  
 Dopo l'abilitazione della posta elettronica, nella finestra di dialogo **Personalizza le impostazioni del rapporto di stato** modificare le impostazioni SMTP e specificare un elenco di destinatari di posta elettronica. Nel riquadro attività comparirà una nuova attività: **Invia il rapporto di stato tramite posta elettronica**. Per altre informazioni sulle impostazioni SMTP, vedere [Configurare le notifiche di posta elettronica per gli avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 È possibile selezionare un rapporto esistente, quindi fare clic su **Invia il rapporto di stato tramite posta elettronica**. È anche possibile generare un nuovo rapporto e fare in modo che sia inviato automaticamente alla Posta in arrivo. Se è stata configurata una pianificazione per la generazione automatica del rapporto, il rapporto sarà recapitato automaticamente nella Posta in arrivo dopo la generazione ogni giorno (oppure ogni ora), in base alla pianificazione.  
  
##  <a name="BKMK_View"></a>Visualizzare gli avvisi tramite il Visualizzatore avvisi  
 Questa sezione illustra come usare il dashboard o la finestra di avvio per aprire il Visualizzatore avvisi per verificare lo stato di integrità di tutti i computer nella rete di server.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Per aprire il Visualizzatore avvisi tramite il dashboard  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento fare clic su una delle icone di avviso visualizzate (messaggio critico, di avviso o informativo). Verrà aperto il Visualizzatore avvisi.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Per aprire il Visualizzatore avvisi dalla Finestra di avvio  
  
1.  Aprire la finestra di avvio da un computer connesso al server. Se richiesto, accedere alla Finestra di avvio specificando nome utente e password.  
  
2.  Fare clic su una delle icone di avviso visualizzate (messaggio critico, di avviso o informativo) nella parte inferiore della finestra di avvio per aprire il Visualizzatore avvisi, quindi seguire le istruzioni nel riquadro dei dettagli del Visualizzatore avvisi per risolvere l'avviso.  
  
##  <a name="BKMK_Organize"></a>Organizzare gli avvisi nel Visualizzatore avvisi  
 È possibile organizzare gli avvisi nel Visualizzatore avvisi e visualizzarli in base alla gravità (messaggio critico, di avviso o informativo) oppure in base al nome computer.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Per organizzare gli avvisi nel Visualizzatore avvisi  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento fare clic su una delle icone di avviso visualizzate (messaggio critico, di avviso o informativo). Verrà aperto il Visualizzatore avvisi.  
  
3.  Espandere l'elenco a discesa **Organizza**, quindi eseguire una delle operazioni seguenti:  
  
    1.  Selezionare **Filtra per computer**, quindi fare clic sul nome del computer per cui si vogliono visualizzare gli avvisi. Nel Visualizzatore avvisi saranno mostrati solo gli avvisi per il computer selezionato.  
  
    2.  Selezionare **Filtra per tipo avviso** e fare clic sul tipo di avviso (messaggio critico, di avviso o informativo) da visualizzare. Nel Visualizzatore avvisi saranno mostrati solo gli avvisi del tipo selezionato.  
  
##  <a name="BKMK_Respond"></a>Rispondere agli avvisi  
 In caso di avviso, è possibile eseguire una delle operazioni seguenti:  
  
-   [Risolvere un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Ignorare un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Abilitare un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Eliminare un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>Risolvere un avviso  
 Per risolvere un avviso, seguire le istruzioni per la risoluzioni disponibili nel Visualizzatore avvisi. Dopo la risoluzione, l'avviso sarà ancora visualizzato nel Visualizzatore avvisi, fino all'aggiornamento successivo.  
  
###  <a name="BKMK_3"></a>Ignorare un avviso  
 È possibile scegliere di ignorare un avviso, se si preferisce rispondere in un secondo momento. Quando lo si ignora, l'avviso rimane elencato nel Visualizzatore avvisi, ma sarà disabilitato e visualizzato in grigio. Un avviso ignorato non sarà incluso nella valutazione complessiva dello stato del computer. Per risolvere un avviso ignorato, è prima di tutto necessario abilitarlo.  
  
##### <a name="to-ignore-an-alert"></a>Per ignorare un avviso  
  
1. Aprire la finestra di avvio nel computer connesso al server di Windows Server Essentials.  
  
2. Nella finestra di avvio fare clic su una delle icone di avviso visualizzate (messaggio critico, di avviso o informativo). Verrà aperto il Visualizzatore avvisi.  
  
3. Nel Visualizzatore avvisi selezionare l'avviso da ignorare, quindi nella sezione **Attività** fare clic su **Ignora avviso**.  
  
   Per rispondere a un avviso disabilitato, sarà prima di tutto necessario abilitarlo.  
  
###  <a name="BKMK_5"></a>Abilitare un avviso  
 È possibile abilitare un avviso ignorato in precedenza. Dopo l'abilitazione, sarà possibile risolvere l'avviso o eliminarlo, in base alla necessità. Un avviso è visualizzato come disabilitato quando lo si contrassegna in modo che sia ignorato. Quando si abilita un avviso disabilitato in precedenza, l'avviso tornerà attivo e sarà incluso di nuovo nella valutazione complessiva dello stato dei computer.  
  
##### <a name="to-enable-an-alert"></a>Per attivare un avviso  
  
1.  Aprire la finestra di avvio dal computer connesso al server.  
  
2.  Nella finestra di avvio fare clic su una delle icone di avviso visualizzate (messaggio critico, di avviso o informativo) per aprire il Visualizzatore avvisi.  
  
3.  Nel Visualizzatore avvisi fare clic con il pulsante destro del mouse sull'avviso da abilitare, quindi scegliere **Attiva avviso**.  
  
###  <a name="BKMK_4"></a>Eliminare un avviso  
 È possibile eliminare un avviso se non si vuole risolverlo o ignorarlo. Per eliminare gli avvisi generati per il computer, è possibile usare il Visualizzatore avvisi nella finestra di avvio. Se si elimina un avviso e il server rileva di nuovo il problema nel ciclo di valutazione dell'integrità di rete successivo, sarà generato un nuovo avviso.  
  
##### <a name="to-delete-an-alert"></a>Per eliminare un avviso  
  
1.  Aprire la finestra di avvio dal computer connesso al server.  
  
2.  Nella finestra di avvio fare clic su una delle icone di avviso visualizzate (messaggio critico, di avviso o informativo) per aprire il Visualizzatore avvisi.  
  
3.  Nel Visualizzatore avvisi fare clic con il pulsante destro del mouse sull'avviso da eliminare, quindi scegliere **Elimina avviso**.  
  
##  <a name="BKMK_Email"></a>Configurare le notifiche di posta elettronica per gli avvisi  
 È possibile configurare il server per l'invio di notifiche tramite posta elettronica in merito al verificarsi degli avvisi. Le notifiche di posta elettronica per questi avvisi includono informazioni sui problemi di rete e le relative procedure di risoluzione. Le stesse informazioni sono visualizzate nel Visualizzatore avvisi. Alcune valutazioni dell'integrità della rete sono eseguite a livello di codice.  
  
 Quando si configura il server per inviare notifiche sugli avvisi tramite posta elettronica, un messaggio di posta elettronica sarà inviato per gli avvisi rilevati durante la valutazione dell'integrità della rete. Tuttavia, non tutti gli avvisi disponibili nel Visualizzatore avvisi sono inclusi nel messaggio di posta elettronica.  
  
 L'attività Valutazione messaggio di posta elettronica avvisi è eseguita sul server ogni trenta minuti, per verificare la presenza di avvisi relativi alla rete. Se viene rilevato un avviso per cui è stata configurata la notifica di posta elettronica, sarà inviata la notifica corrispondente. Se l'avviso è ancora attivo al secondo ciclo di valutazione, non sarà inviato un secondo messaggio di posta elettronica, in modo da non sovraccaricare la cassetta postale dell'utente. Se tuttavia un nuovo avviso viene rilevato in un ciclo di valutazione degli avvisi successivo, sarà inviata una notifica di posta elettronica, che include i nuovi avvisi e gli avvisi precedenti.  
  
###  <a name="BKMK_list"></a>Avvisi che generano notifiche tramite posta elettronica  
 Gli avvisi seguenti nel Visualizzatore avvisi generano notifiche di posta elettronica quando si configura il server per l'invio di notifiche di posta elettronica per gli avvisi:  
  
-   Errori in un backup del computer client.  
  
-   Il server è stato riavviato.  
  
-   Uno o più servizi non sono in esecuzione.  
  
-   Il servizio di archiviazione server Windows non è in esecuzione.  
  
-   Il periodo di valutazione è terminato.  
  
-   Attiva.  
  
-   Chiusura del server di origine.  
  
-   I risultati dell'analisi BPA contengono degli errori.  
  
-   I risultati dell'analisi BPA contengono degli avvisi.  
  
-   Non è disponibile un certificato per Accesso remoto via Internet.  
  
-   Il certificato per Accesso remoto via Internet è scaduto.  
  
-   Router non configurato correttamente.  
  
-   Il server Web non è configurato correttamente.  
  
-   Servizi Desktop remoto non configurati correttamente.  
  
-   Firewall non configurato correttamente.  
  
-   Nome di dominio Internet scaduto.  
  
-   Impossibile aggiornare il nome di dominio Internet  
  
-   Errore licenza: Verifica trust tra foreste.  
  
-   Errore licenza: Verifica controller di dominio.  
  
-   Errore licenza: Verifica ruolo FSMO.  
  
-   Errore licenza: Criteri FSMO di imposizione.  
  
-   Errore licenza: Criteri caricamento imposizione.  
  
-   Errore licenza: errore Servizi di dominio Active Directory.  
  
-   L'abbonamento a Office 365 è scaduto.  
  
-   Autenticazione di Office 365 non riuscita.  
  
-   Il criterio password non è corretto.  
  
-   Il servizio di sincronizzazione password non riesce a sincronizzare una password utente con Office 365.  
  
-   Cambiare la password di Windows.  
  
-   La password di Office 365 non corrisponde alla password di Windows.  
  
-   Impossibile connettersi a Exchange Server.  
  
-   Microsoft Exchange Server ha un problema.  
  
-   Una o più unità disco rigido incluse nel backup del server non sono collegate.  
  
-   Il disco rigido di backup non dispone di spazio libero sufficiente per il backup del server.  
  
-   Il backup del server non è stato eseguito correttamente perché non è stato possibile creare lo snapshot di un'unità.  
  
-   Un backup pianificato non è stato completato correttamente.  
  
-   Una o più cartelle server predefinite sono mancanti.  
  
-   Spazio disponibile in esaurimento su una o più unità disco rigido del server.  
  
-   Il processo di scrittura del servizio Copia shadow del volume non è in esecuzione.  
  
-   Capacità insufficiente sulle unità disco rigido.  
  
-   Uno o più unità non funzionano e sono offline.  
  
###  <a name="BKMK_SMTP"></a>Configurazione di SMTP sul server per l'invio di notifiche di avviso tramite posta elettronica in Windows Server Essentials  
 Questa sezione illustra come configurare il server per l'invio di notifiche di posta elettronica per gli avvisi.  
  
> [!NOTE]
>  È possibile scaricare il componente aggiuntivo rapporto di stato per Windows Server Essentials dall' [area download Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Per configurare le notifiche di posta elettronica per gli avvisi  
  
1.  Nel **Dashboard** aprire il **Visualizzatore avvisi**.  
  
2.  Nel **Visualizzatore avvisi** fare clic s **Configura notifiche di posta elettronica per gli avvisi**.  
  
3.  Nella finestra **Configura notifiche di posta elettronica per gli avvisi** fare clic su **Abilita**.  
  
4.  Nella finestra **Impostazioni SMTP** eseguire le operazioni seguenti:  
  
    1.  Per **Indirizzo di posta elettronica mittente** digitare l'indirizzo di posta elettronica da usare per inviare gli avvisi tramite posta elettronica. Questo indirizzo di posta elettronica verrà visualizzato come indirizzo del mittente nella notifica di avviso.  
  
    2.  Per **Nome server SMTP** digitare nella casella di testo **Indirizzo di posta elettronica mittente** il nome del server SMTP specificato nel passaggio 4a. Per un elenco di nomi server SMTP, vedere la Tabella 1.  
  
    3.  Per **Porta SMTP** digitare il numero di porta usato dal server SMTP per inviare e ricevere messaggi di posta elettronica. Per i numeri di porta usati da alcuni server SMTP, vedere la Tabella 1.  
  
    4.  Selezionare **Il server necessita di una connessione protetta (SSL)** se il server SMTP usa SSL (vedere la Tabella 1).  
  
    5.  Selezionare **Il server richiede l'autenticazione** se il server SMTP necessita di informazioni su nome utente e password (vedere la Tabella 1). Se si seleziona questa casella di controllo, digitare il nome utente e la password dell'indirizzo di posta elettronica immesso nel campo **Indirizzo di posta elettronica mittente** nel passaggio 4a, quindi fare clic su **OK**.  
  
    > [!NOTE]
    >  È possibile ottenere le informazioni su nome server SMTP, numero di porta e utilizzo di SSL dal provider di servizi Internet.  
  
     **Tabella 1** Esempi di nomi di server SMTP, requisiti di autenticazione e crittografia SSL e numeri di porta  
  
    |SMTP Server|SSL Required|Autenticazione necessaria|Numero porta|Nome account/Nome di accesso|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|Sì|Sì|587|Specificare l'indirizzo di posta elettronica completo con nome di dominio e password per l'autenticazione.|  
    |smtp.live.com|Sì|Sì|587|Specificare l'indirizzo di posta elettronica completo con nome di dominio e password per l'autenticazione.|  
    |smtp.comcast.net|Sì|No|587|Specificare l'indirizzo di posta elettronica completo con nome di dominio e password per l'autenticazione.|  
    |smtp.mail.yahoo.com|No|Sì|25|Specificare solo l'indirizzo di posta elettronica, senza nome di dominio per il nome utente.|  
  
5.  In **Configura notifiche per gli avvisi** digitare per **Destinatari di posta elettronica** gli indirizzi di posta elettronica degli utenti a cui si vogliono inviare notifiche di avviso tramite posta elettronica. Assicurarsi di separare ogni indirizzo di posta elettronica con un punto e virgola (;).  
  
6.  Per verificare che le impostazioni del server SMTP siano state configurate correttamente per l'invio di notifiche di posta elettronica per gli avvisi, fare clic su **Applica e invia posta elettronica**.  
  
    > [!NOTE]
    >  Quando si fa clic su **Applica e invia posta elettronica**, si riceve in genere una notifica di posta elettronica di esempio, senza che siano elencati avvisi. Se tuttavia durante il processo di test viene identificato un avviso di stato configurato per l'invio di una notifica di posta elettronica, l'avviso sarà incluso nel messaggio di posta elettronica di prova.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configurazione di SMTP nel server per l'invio di report sull'integrità in Windows Server Essentials  
 Questa sezione illustra come configurare le impostazioni di SMTP per il server, in modo che sia possibile ricevere rapporti di stato tramite posta elettronica.  
  
> [!NOTE]
>  Per impostazione predefinita, il componente aggiuntivo rapporto di stato è integrato con Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato e i report sull'integrità vengono visualizzati nella scheda **rapporti di stato** della **Home** page del dashboard.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Per configurare le notifiche di posta elettronica per i rapporti di stato  
  
1.  Nel **Dashboard** fare clic sulla scheda **Rapporti**.  
  
2.  Nel riquadro attività **Attività dei rapporti di stato** fare clic su **Personalizza le impostazioni del rapporto di stato**.  
  
3.  Nella finestra **Personalizza le impostazioni del rapporto di stato** fare clic sulla scheda **Pianifica e invia tramite posta elettronica**.  
  
4.  Nella sezione **Posta elettronica** della scheda **Pianifica e invia tramite posta elettronica** eseguire le operazioni seguenti:  
  
    1.  Fare clic su **Abilita**, quindi digitare l'indirizzo di posta elettronica da usare per l'invio dei rapporti di stato. Questo indirizzo di posta elettronica verrà visualizzato come indirizzo del mittente nei rapporti di stato inviati tramite posta elettronica.  
  
        1.  Per **Nome server SMTP** digitare il nome del server SMTP. Per un elenco di nomi server SMTP, vedere la Tabella 1.  
  
        2.  Per **Porta SMTP** digitare il numero di porta usato dal server SMTP per inviare e ricevere messaggi di posta elettronica. Per i numeri di porta usati da alcuni server SMTP, vedere la Tabella 1.  
  
        3.  Selezionare **Il server necessita di una connessione protetta (SSL)** se il server SMTP usa SSL (vedere la Tabella 1).  
  
        4.  Selezionare **Il server richiede l'autenticazione** se il server SMTP necessita di informazioni su nome utente e password (vedere la Tabella 1). Se si seleziona questa casella di controllo, digitare il nome utente e la password dell'indirizzo di posta elettronica immesso nel campo **Indirizzo di posta elettronica mittente** nel passaggio 4a, quindi fare clic su **OK**.  
  
            > [!NOTE]
            >  È possibile ottenere le informazioni su nome server SMTP, numero di porta e utilizzo di SSL dal provider di servizi Internet.  
  
            > [!NOTE]
            >  Microsoft consiglia di usare SSL, poiché il rapporto potrebbe contenere informazioni sullo stato del server che potrebbero essere usate da parti non autorizzate per rilevare vulnerabilità, ad esempio aggiornamenti di Windows mancanti. Se si abilita SSL, i dati in transito saranno crittografati e sarà ridotto il rischio di esposizione di vulnerabilità del server.  
  
5.  Dopo l'abilitazione della posta elettronica, sarà visualizzato il collegamento **Modifica impostazioni SMTP**. Anche la nuova attività **Invia il rapporto di stato tramite posta elettronica** sarà visualizzata in **Attività dei rapporti di stato**.  
  
     **Tabella 1** Esempi di nomi di server SMTP, requisiti di autenticazione e crittografia SSL e numeri di porta  
  
    |SMTP Server|SSL Required|Autenticazione necessaria|Numero porta|Nome account/Nome di accesso|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|Sì|Sì|587|Specificare l'indirizzo di posta elettronica completo con nome di dominio e password per l'autenticazione.|  
    |smtp.live.com|Sì|Sì|587|Specificare l'indirizzo di posta elettronica completo con nome di dominio e password per l'autenticazione.|  
    |smtp.comcast.net|Sì|No|587|Specificare l'indirizzo di posta elettronica completo con nome di dominio e password per l'autenticazione.|  
    |smtp.mail.yahoo.com|No|Sì|25|Specificare solo l'indirizzo di posta elettronica, senza nome di dominio per il nome utente.|  
  
6.  In **Personalizza le impostazioni del rapporto di stato** digitare per **Invia automaticamente rapporto di stato ai destinatari di posta elettronica seguenti** gli indirizzi di posta elettronica degli utenti a cui si vogliono inviare i rapporti di stato tramite posta elettronica. Assicurarsi di separare ogni indirizzo di posta elettronica con un punto e virgola (;).  
  
7.  Per verificare che le impostazioni del server SMTP siano state configurate correttamente per l'invio di rapporti di stato tramite posta elettronica, selezionare un rapporto nella scheda Rapporto di stato del dashboard e quindi fare clic su **Invia il rapporto di stato tramite posta elettronica** dal riquadro attività.  
  
##  <a name="BKMK_Potential"></a>Avvisi potenziali del computer  
 Questa sezione permette la comprensione e la gestione degli avvisi specifici del computer connesso al server e visualizzati nella finestra di avvio del computer.  
  
 Nella tabella seguente sono elencati alcuni avvisi del computer che possono essere generati e visualizzati nel Visualizzatore avvisi, se applicabili al computer specifico.  
  
|Titolo dell'avviso|Impatto e risoluzione dell'avviso|  
|-----------------|---------------------------------|  
|Lo stato corrente del firewall di rete offre protezione ridotta al computer.|È possibile che utenti o software non autorizzati siano in grado di accedere al computer, se Windows Firewall non è attivato.|  
|Protezione da virus è disattivata, non installata o non aggiornata.|I dati del computer sono a rischio, se l'impostazione di sicurezza **Protezione da virus** è disattivata o non è aggiornata. [Per proteggere il computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), eseguire la procedura indicata.|  
|Protezione da spyware e software indesiderato disattivata, non installata o non aggiornata.|I dati nel computer sono a rischio se l'opzione **Protezione da spyware e software indesiderato** è disattivata o non aggiornata. [Per proteggere il computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), eseguire la procedura indicata.|  
|Windows Update è disabilitato.|Se Windows Update non è attivato, non sarà possibile usufruire delle funzionalità nuove o corrette offerte dagli aggiornamenti. Per attivare Windows Update, nel Visualizzatore avvisi fare clic su **Apri Windows Update**.<br /><br /> Se l'attività **Apri Windows Update** non è visualizzata, non si è connessi al computer in cui è stato generato l'avviso. Per eseguire questa attività nel Visualizzatore avvisi, è necessario essere connessi al computer in cui è stato generato l'avviso.|  
|È necessario installare gli aggiornamenti importanti.|Se Windows Update non è attivato, non sarà possibile usufruire delle funzionalità nuove o corrette offerte dagli aggiornamenti. Per attivare Windows Update, nel Visualizzatore avvisi fare clic su **Apri Windows Update**.<br /><br /> Se l'attività **Apri Windows Update** non è visualizzata, non si è connessi al computer in cui è stato generato l'avviso. Per eseguire questa attività nel Visualizzatore avvisi, è necessario essere connessi al computer in cui è stato generato l'avviso.|  
|Riavviare il computer per applicare gli aggiornamenti.|Se non si applicano gli aggiornamenti, non sarà possibile usufruire delle funzionalità nuove o corrette offerte dagli aggiornamenti. Salvare tutti i dati e riavviare il computer per applicare gli aggiornamenti.|  
|Spazio sulle unità disco rigido insufficiente.|Se non si rende disponibile spazio, non sarà possibile salvare informazioni aggiuntive. Per aumentare lo spazio disponibile nel computer, considerare le opzioni seguenti:<br /><br /> -Aggiungere un nuovo disco rigido.<br /><br /> -Eseguire **Pulitura disco** per rimuovere i file obsoleti e temporanei.<br /><br /> -Spostare i file in una cartella condivisa in un altro computer.<br /><br /> -Archivia i file su supporti rimovibili, ad esempio un CD, un DVD o un disco rigido esterno.|  
|L'agente **Cronologia file** nel server non è configurato correttamente per l'esecuzione in questo computer.|Non è possibile creare i backup di Cronologia file.|  
|Uno o più servizi non sono in esecuzione.||  
|Cambiare la password di Windows.||  
|La password di Microsoft Office 365 non corrisponde alla password di Windows.||  
  
###  <a name="BKMK_Protect"></a>Per proteggere il computer  
  
1.  Aprire il Centro sicurezza PC.  
  
2.  Determinare lo stato della protezione da virus installata.  
  
3.  Eseguire una delle attività seguenti, in base allo stato di protezione:  
  
    -   Se non è abilitata, abilitarla.  
  
    -   Se non è aggiornata, completare il processo per aggiornare le firme.  
  
    -   Se la protezione da virus non è installata, considerarne l'installazione.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
