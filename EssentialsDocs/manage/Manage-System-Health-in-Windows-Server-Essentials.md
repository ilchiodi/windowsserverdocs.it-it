---
title: "Gestire l'integrità di sistema in Windows Server Essentials"
description: Viene descritto come utilizzare Windows Server Essentials
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
ms.openlocfilehash: 91635a58c64fbf74d3b0139be7c9c36365487319
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Gestire l'integrità di sistema in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 In questo argomento viene illustrato come visualizzare e rispondere a tutti gli avvisi nella rete tramite il Dashboard.  
  
> [!NOTE]
>  In Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, gli avvisi sull'integrità per i computer client nella rete e server non sono più visualizzati nel Visualizzatore avvisi, ma invece possono essere visualizzati nel **rapporti di stato** scheda della finestra di **Home** pagina.  
  
 Windows Server Essentials monitora attivamente ogni computer in cui è connesso al server e avvisa l'amministratore i problemi relativi all'integrità del sistema s, inclusi gli aggiornamenti critici, mancante malware protection, le definizioni di virus non aggiornate nei computer client e altri importanti problemi che richiedono un'azione. Questi problemi sono visualizzati come avvisi nel Visualizzatore avvisi, che può essere avviato dal server s Dashboard o il computer client s in Windows Server Essentials o nella finestra di avvio di **rapporti di stato** scheda in Windows Server Essentials. Per impostazione predefinita, gli avvisi vengono aggiornati ogni trenta minuti, ma è possibile valutare la rete per gli avvisi in qualsiasi momento facendo **aggiornare** nel Visualizzatore avvisi o scegliere di **rapporti di stato** scheda.  
  
 Gli argomenti seguenti consentono di comprendere, visualizzare e rispondere agli avvisi nel Visualizzatore avvisi e offrono anche istruzioni per configurare il server per ricevere le notifiche di avviso tramite posta elettronica:  
  
-   [Sullo stato componente aggiuntivo rapporto](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Visualizzare gli avvisi tramite il Visualizzatore avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organizzare gli avvisi nel Visualizzatore avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Rispondere agli avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurare le notifiche di posta elettronica per gli avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Avvisi potenziali del computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>Sullo stato componente aggiuntivo rapporto  
 Il componente aggiuntivo rapporto di stato per Windows Server Essentials offre informazioni consolidate sulla rete di Windows Server Essentials e consente di distribuire queste informazioni ad altri utenti. Queste informazioni possono essere visualizzate nel **report** scheda del Dashboard. Con la **report** scheda, è possibile eseguire le operazioni seguenti:  
  
-   [Generare un rapporto su richiesta o in una pianificazione](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personalizzare il contenuto del report](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [Il report di posta elettronica](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:** è possibile scaricare il componente aggiuntivo rapporto di stato per Windows Server Essentials dal [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials:** per impostazione predefinita, il componente aggiuntivo rapporto di stato è integrato con Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, e i report sull'integrità vengono visualizzati nel **rapporti di stato** scheda del Dashboard s **Home** pagina.  
  
###  <a name="BKMK_Generate"></a>Generare un rapporto su richiesta o in una pianificazione  
 Dopo aver installato il componente aggiuntivo rapporto di stato e il riavvio del Dashboard, una nuova scheda, **report** viene aggiunto al Dashboard. È possibile generare un rapporto di stato su richiesta in qualsiasi momento facendo il **generare un rapporto di integrità** attività sul **report** scheda.  
  
 Dopo la generazione di un rapporto di stato, viene creato un nuovo elemento nel riquadro elenco, identificato dalla data e ora che è stato generato il report. Per aprire un elemento, farvi doppio clic nel riquadro elenco, oppure è possibile selezionarlo e quindi fare clic su **aprire il rapporto di stato** nel riquadro attività. Il report viene visualizzato in una nuova finestra in formato HTML.  
  
 Oltre a generare manualmente un rapporto, è anche il rapporto sia generato automaticamente in base a una pianificazione giornaliera oppure oraria. A tale scopo, nel riquadro attività, fare clic su **personalizzare le impostazioni di report di stato**, quindi fare clic su di **pianificazione e posta elettronica** scheda. Il **pianificazione** funzionalità è disattivata per impostazione predefinita ed è possibile attivarla selezionando il **generare un rapporto di integrità all'orario pianificato** casella di controllo.  
  
###  <a name="BKMK_Customize"></a>Personalizzare il contenuto del report  
 Il rapporto di stato contiene quanto segue:  
  
-   **Avvisi critici e avvisi** ciò è coerente con gli avvisi critici e gli avvisi visualizzati nel Visualizzatore avvisi nel Dashboard. Gli avvisi informativi non sono inclusi nel rapporto di stato.  
  
-   **Gli errori critici nei registri eventi** registri applicazioni e servizi vengono analizzati e gli errori registrati nelle ultime 24 ore saranno inclusi nel **dettagli** sezione del report.  
  
-   **Backup del server** vengono presentate le informazioni sull'ultimo backup del server nel **dettagli** sezione del report.  
  
-   **Servizi ad avvio automatico non in esecuzione** al momento è generato il report, se un servizio ad avvio automatico non è in esecuzione, le informazioni su questo servizio conterrà il **dettagli** sezione del report.  
  
-   **Gli aggiornamenti** è possibile visualizzare lo stato di aggiornamento del server e tutti i computer client il **dettagli** sezione.  
  
-   **Archiviazione** l'elenco di unità e delle rispettive capacità è visualizzato nel **dettagli** sezione.  
  
 Nel rapporto di stato, visualizzare il **riepilogo**, quindi fare clic su per tali elementi con un'icona rossa di errore o un'icona di avviso gialla, il **dettagli** collegamento sulla stessa riga per visualizzare i dettagli sull'elemento.  
  
 Se non si è interessati ad alcuni punti dati nel report vengono inclusi per impostazione predefinita, è possibile personalizzare il contenuto del report facendo **personalizzare le impostazioni di report di stato** nel riquadro attività e quindi fai clic su di **contenuto**scheda deselezionare le caselle di controllo per il contenuto che si don t desidera visualizzare nel report. Ad esempio, se si dispone di tuo piano di backup del server e tenere sempre desidera visualizzare gli avvisi relativi al backup del server, è possibile escluderli dal rapporto deselezionando la **backup Server** casella di controllo.  
  
###  <a name="BKMK_emailreport"></a>Il report di posta elettronica  
 Effettuare l'accesso al Dashboard per leggere i rapporti risulta scomodo per alcuni amministratori, in particolare se hanno uno o più server da gestire. Con la funzionalità di posta elettronica attivata, dopo la generazione di un report, un messaggio di posta elettronica verrà inviato a un elenco di indirizzi di posta elettronica specificato con il contenuto del report. L'amministratore può facilmente visualizzare il report da qualsiasi dispositivo o applicazione client e verificare che il server è in esecuzione con lo stato ottimale.  
  
 Nel **Personalizza le impostazioni del rapporto di integrità** nella finestra di dialogo dopo l'abilitazione della posta elettronica, modificare le impostazioni SMTP e specificare un elenco di destinatari di posta elettronica, si noterà che una nuova attività viene visualizzata nel riquadro attività: **il rapporto di stato di posta elettronica**. Per ulteriori informazioni sulle impostazioni SMTP, vedere [impostare le notifiche di posta elettronica per avvisi](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 È possibile selezionare un report esistente e quindi fare clic su **il rapporto di stato di posta elettronica**. È anche possibile generare un nuovo report e inviarlo automaticamente nella posta in arrivo. Se è stata configurata una pianificazione per il rapporto sia generato automaticamente, il rapporto sarà recapitato automaticamente nella posta in arrivo dopo la generazione ogni giorno (o ogni ora) come pianificato.  
  
##  <a name="BKMK_View"></a>Visualizzare gli avvisi tramite il Visualizzatore avvisi  
 Questa sezione illustra come usare il Dashboard o la finestra di avvio per aprire il Visualizzatore avvisi per visualizzare lo stato di integrità di tutti i computer nella rete di server.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Per aprire il Visualizzatore avvisi tramite il Dashboard  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento, fare clic su una delle icone di avviso visualizzate (critico, avviso o informativo). Verrà aperto il Visualizzatore avvisi.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Per aprire il Visualizzatore avvisi dalla finestra di avvio  
  
1.  Da un computer connesso al server, aprire la finestra di avvio. Se richiesto, accedere alla finestra di avvio con il nome utente e password.  
  
2.  Fare clic su una delle icone di avviso visualizzate (critiche, avviso e informative) nella parte inferiore della finestra di avvio per aprire il Visualizzatore avvisi e quindi segui le istruzioni nel riquadro dei dettagli del Visualizzatore avvisi per risolvere l'avviso.  
  
##  <a name="BKMK_Organize"></a>Organizzare gli avvisi nel Visualizzatore avvisi  
 È possibile organizzare gli avvisi nel Visualizzatore avvisi e visualizzarli in base alla gravità (critico, avviso o informativo) o in base al nome del computer.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Per organizzare gli avvisi nel Visualizzatore avvisi  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento, fare clic su una delle icone di avviso visualizzate (critico, avviso o informativo). Verrà aperto il Visualizzatore avvisi.  
  
3.  Espandere il **Organizza** elenco a discesa e quindi eseguire una delle operazioni seguenti:  
  
    1.  Selezionare **Filtra per computer**, fare clic sul nome del computer per cui si desidera visualizzare gli avvisi. Consente di visualizzare gli avvisi nel Visualizzatore avvisi solo per il computer selezionato.  
  
    2.  Selezionare **filtrare per tipo di avviso**, fare clic sul tipo di avviso (critico, avviso o informativo) per il quale si desidera visualizzare gli avvisi. Consente di visualizzare solo il tipo di avviso selezionato nel Visualizzatore avvisi.  
  
##  <a name="BKMK_Respond"></a>Rispondere agli avvisi  
 Quando si verifica un avviso, è possibile scegliere di effettuare una delle operazioni seguenti:  
  
-   [Risolvere un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Ignorare un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Abilitare un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Eliminare un avviso](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>Risolvere un avviso  
 Seguire le istruzioni di risoluzione nel Visualizzatore avvisi per risolvere l'avviso. Dopo che un avviso viene risolto, viene ancora visualizzato nel Visualizzatore avvisi fino a quando non vengono aggiornate.  
  
###  <a name="BKMK_3"></a>Ignorare un avviso  
 È possibile scegliere di ignorare un avviso se si preferisce rispondere in un secondo momento. Quando si ignora un avviso, è comunque elencato nel Visualizzatore avvisi, e ma è disabilitato e visualizzato in grigio. Un avviso ignorato non è incluso nella valutazione complessiva dello stato del computer. Per risolvere un avviso ignorato, è innanzitutto necessario attivare l'avviso.  
  
##### <a name="to-ignore-an-alert"></a>Per ignorare un avviso  
  
1.  Dal computer in cui è connesso al server di Windows Server Essentials, aprire la finestra di avvio.  
  
2.  Nella finestra di avvio, fare clic su una delle icone di avviso visualizzate (critiche, avviso e informative). Verrà aperto il Visualizzatore avvisi.  
  
3.  Nel Visualizzatore avvisi, selezionare l'avviso che si desidera ignorare, e quindi il **attività** fare clic su **ignorare l'avviso**.  
  
 Per rispondere a un avviso disabilitato, sarà necessario innanzitutto attivare l'avviso.  
  
###  <a name="BKMK_5"></a>Abilitare un avviso  
 È possibile abilitare un avviso che si è scelto di ignorare in precedenza. Dopo l'avviso è abilitato, è possibile risolvere il problema o eliminarlo in base alle esigenze. Un avviso viene visualizzato come disabilitato quando contrassegnate per essere ignorato. Quando si abilita un avviso che è stata disabilitata in precedenza, diventa attiva ed è nuovamente incluso nella valutazione complessiva dello stato dei computer.  
  
##### <a name="to-enable-an-alert"></a>Per abilitare un avviso  
  
1.  Dal computer in cui è connesso al server, aprire la finestra di avvio.  
  
2.  Nella finestra di avvio, fare clic su una delle icone di avviso visualizzate (critiche, avviso e informative) per aprire il Visualizzatore avvisi.  
  
3.  Nel Visualizzatore avvisi, fare doppio clic sull'avviso che si desidera abilitare, quindi fare clic su **abilitare l'avviso**.  
  
###  <a name="BKMK_4"></a>Eliminare un avviso  
 È possibile eliminare un avviso se non si desidera risolverlo o ignorarlo. È possibile utilizzare il Visualizzatore avvisi nella finestra di avvio per eliminare gli avvisi generati per il computer. Se si elimina un avviso e il server rileva il problema nuovamente durante il successivo ciclo di valutazione dello stato di rete, viene generato un nuovo avviso.  
  
##### <a name="to-delete-an-alert"></a>Per eliminare un avviso  
  
1.  Dal computer in cui è connesso al server, aprire la finestra di avvio.  
  
2.  Nella finestra di avvio, fare clic su una delle icone di avviso visualizzate (critiche, avviso e informative) per aprire il Visualizzatore avvisi.  
  
3.  Nel Visualizzatore avvisi, fare doppio clic sull'avviso che si desidera eliminare e quindi fare clic su **eliminare l'avviso**.  
  
##  <a name="BKMK_Email"></a>Configurare le notifiche di posta elettronica per gli avvisi  
 È possibile configurare il server per l'invio di notifiche tramite posta elettronica merito al verificarsi degli avvisi. Le notifiche di posta elettronica per gli avvisi contengono informazioni su problemi di rete e la risoluzione dei problemi, che è uguale alle informazioni che viene visualizzate nel Visualizzatore avvisi. Alcune delle versioni di valutazione di integrità di rete vengono apportate a livello di codice.  
  
 Quando si configura il server per inviare le notifiche di avviso tramite posta elettronica, viene inviata una notifica di posta elettronica per gli avvisi rilevati durante la valutazione di integrità della rete. Tuttavia, non tutti gli avvisi segnalati nel Visualizzatore avvisi sono indicati nel messaggio di posta elettronica.  
  
 Ogni trenta minuti, viene eseguita l'attività di valutazione di avviso tramite posta elettronica sul server, che restituisce la rete per gli avvisi. Se viene inviata una notifica di posta elettronica qualsiasi avviso è impostato per la notifica tramite posta elettronica si verifica. Un secondo messaggio di posta elettronica non verrà inviata se l'avviso è ancora attivo al successivo ciclo di valutazione di sovraccaricare la cassetta postale. Tuttavia, se viene rilevato un nuovo avviso all'interno di un ciclo di valutazione degli avvisi, una notifica di posta elettronica è inviato, che include gli avvisi nuovi e precedenti.  
  
###  <a name="BKMK_list"></a>Avvisi che generano notifiche di posta elettronica  
 Gli avvisi seguenti nel Visualizzatore avvisi generano notifiche di posta elettronica di quando si configura il server per inviare notifiche tramite posta elettronica per gli avvisi:  
  
-   Sono presenti errori in un backup del computer client.  
  
-   Il server è stato riavviato.  
  
-   Uno o più servizi non sono in esecuzione.  
  
-   Il servizio di archiviazione di Windows Server non è in esecuzione.  
  
-   Il periodo di valutazione è su.  
  
-   Attiva.  
  
-   Chiusura del Server di origine.  
  
-   Risultati dell'analisi BPA contengono degli errori.  
  
-   Risultati dell'analisi BPA contengono degli avvisi.  
  
-   Un certificato non è disponibile per l'accesso remoto via Internet.  
  
-   Il certificato per accesso remoto via Internet è scaduto.  
  
-   Il router non è configurato correttamente.  
  
-   Il Server Web non è configurato correttamente.  
  
-   Servizi Desktop remoto non è configurato correttamente.  
  
-   Il firewall non è configurato correttamente.  
  
-   Il nome di dominio Internet scaduto.  
  
-   Impossibile aggiornare il nome di dominio Internet.  
  
-   Errore licenza: Verifica di trust tra foreste.  
  
-   Errore licenza: Verifica Controller di dominio.  
  
-   Errore licenza: Verifica ruolo FSMO.  
  
-   Errore licenza: Criteri FSMO di imposizione.  
  
-   Errore licenza: Criteri caricamento imposizione.  
  
-   Errore licenza: Servizi di dominio Active Directory.  
  
-   L'abbonamento a Office 365 è scaduto.  
  
-   Autenticazione di Office 365 non riuscita.  
  
-   I criteri password non sono corretto.  
  
-   Il servizio di sincronizzazione Password non possono sincronizzare una password utente con Office 365.  
  
-   Modificare la password di Windows.  
  
-   La password di Office 365 non corrisponde alla password di Windows.  
  
-   Impossibile connettersi a Exchange Server.  
  
-   Microsoft Exchange Server ha un problema.  
  
-   Non sono connesse una o più unità disco rigida nel Backup del Server.  
  
-   Il disco rigido di backup non dispone di spazio libero sufficiente per il Backup del Server.  
  
-   Backup del server non riuscito perché non è stato da eseguire uno snapshot dell'unità.  
  
-   Un backup pianificato non completato correttamente.  
  
-   Uno o più cartelle server predefinite sono mancante.  
  
-   Spazio disponibile è insufficiente di uno o più unità disco rigido del server.  
  
-   Il Writer VSS per il servizio di archiviazione non è in esecuzione.  
  
-   Capacità insufficiente sulle unità disco rigido.  
  
-   Uno o più unità non funzionano e sono offline.  
  
###  <a name="BKMK_SMTP"></a>Configurazione del protocollo SMTP nel server per inviare le notifiche di avviso tramite posta elettronica in Windows Server Essentials  
 In questa sezione viene illustrato come configurare il server per inviare notifiche tramite posta elettronica per gli avvisi.  
  
> [!NOTE]
>  È possibile scaricare il componente aggiuntivo rapporto di stato per Windows Server Essentials dal [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Per impostare le notifiche di posta elettronica per gli avvisi  
  
1.  Dal **Dashboard**, aprire il **Visualizzatore avvisi**.  
  
2.  Nel **Visualizzatore avvisi**, fare clic su **impostare le notifiche di avviso tramite posta elettronica**.  
  
3.  Nel **configura notifiche di posta elettronica per avvisi** finestra, fare clic su **abilitare**.  
  
4.  Nel **impostazioni SMTP** finestra, eseguire le operazioni seguenti:  
  
    1.  Per **da indirizzo di posta elettronica**, digitare l'indirizzo di posta elettronica che si desidera utilizzare per inviare la posta elettronica avvisi da. Questo indirizzo di posta elettronica verrà visualizzato come indirizzo s mittente nella notifica di avviso.  
  
    2.  Per **nome server SMTP**, nel **da indirizzo di posta elettronica** testo, digitare il nome del server SMTP specificato nel passaggio 4a. (Vedere la tabella 1 per un elenco di nomi server SMTP).  
  
    3.  Per **porta SMTP**, digitare il numero di porta utilizzato dal server SMTP per inviare e ricevere posta elettronica. (Vedere la tabella 1 per i numeri di porta usati da alcuni server SMTP).  
  
    4.  Selezionare **questo server richiede una connessione protetta (SSL)** se il server SMTP Usa SSL (vedere la tabella 1).  
  
    5.  Selezionare **questo server richiede l'autenticazione** se il server SMTP necessita di informazioni su nome e una password utente (vedere la tabella 1). Se si seleziona questa casella di controllo, digitare il nome utente e la password dell'indirizzo di posta elettronica immesso nel **da indirizzo di posta elettronica** campo nel passaggio 4a, quindi fare clic su **OK**.  
  
    > [!NOTE]
    >  È possibile ottenere le informazioni su nome server SMTP, numero di porta e l'uso di SSL dal provider di servizi Internet.  
  
     **Tabella 1** i nomi dei server esempi del protocollo SMTP, autenticazione e i requisiti di crittografia SSL e numeri di porta  
  
    |Server SMTP|SSL necessario|Autenticazione necessaria|Numero di porta|Nome account/nome|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Sì|Sì|587|Fornire l'indirizzo di posta elettronica completo con nome di dominio e la password per l'autenticazione.|  
    |SMTP.Live.com|Sì|Sì|587|Fornire l'indirizzo di posta elettronica completo con nome di dominio e la password per l'autenticazione.|  
    |SMTP.Comcast.NET|Sì|No|587|Fornire l'indirizzo di posta elettronica completo con nome di dominio e la password per l'autenticazione.|  
    |SMTP.Mail.Yahoo.com|No|Sì|25|Fornire solo l'indirizzo di posta elettronica senza un nome di dominio per il nome utente.|  
  
5.  In **configura notifiche per gli avvisi**, per **destinatari di posta elettronica**, digitare gli indirizzi di posta elettronica delle persone che si desidera ricevere le notifiche di avviso tramite posta elettronica. Assicurarsi di separare ogni indirizzo di posta elettronica con un punto e virgola.  
  
6.  Per verificare che sia stato configurato il server SMTP impostazioni correttamente per inviare notifiche tramite posta elettronica per gli avvisi, fare clic su **applica e invia posta elettronica**.  
  
    > [!NOTE]
    >  Quando si fa clic **applica e invia posta elettronica**, in genere si riceverà una notifica di posta elettronica di esempio con senza che siano elencati avvisi. Tuttavia, se durante il processo di test viene identificato un avviso di stato configurato per inviare una notifica di posta elettronica, questo avviso è incluso nell'e-mail di test.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configurazione del protocollo SMTP nel server per inviare i rapporti di stato in Windows Server Essentials  
 In questa sezione viene descritto come configurare le impostazioni SMTP per il server in modo che sia possibile ricevere rapporti di stato tramite posta elettronica.  
  
> [!NOTE]
>  Per impostazione predefinita, il componente aggiuntivo rapporto di stato è integrato con Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, e i report sull'integrità vengono visualizzati nel **rapporti di stato** scheda del Dashboard s **Home** pagina.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Come impostare le notifiche di posta elettronica per i rapporti di integrità  
  
1.  Dal **Dashboard**, fare clic su di **report** scheda.  
  
2.  Nel **attività dei rapporti di integrità** riquadro attività fare clic su **personalizzare le impostazioni di report di stato**.  
  
3.  Nel **Personalizza le impostazioni del rapporto di integrità** finestra, fare clic su di **pianificazione e posta elettronica** scheda.  
  
4.  Nel **pianificazione e posta elettronica** nella scheda il **posta elettronica** sezione, eseguire le operazioni seguenti:  
  
    1.  Fare clic su **abilitare**e digitare l'indirizzo di posta elettronica che si desidera utilizzare per l'invio dell'integrità report dal. Questo indirizzo di posta elettronica verrà visualizzato come indirizzo s mittente nei rapporti di stato che vengono inviati tramite posta elettronica.  
  
        1.  Per **nome server SMTP**, digitare il nome del server SMTP. (Vedere la tabella 1 per un elenco di nomi server SMTP).  
  
        2.  Per **porta SMTP**, digitare il numero di porta utilizzato dal server SMTP per inviare e ricevere posta elettronica. (Vedere la tabella 1 per i numeri di porta usati da alcuni server SMTP).  
  
        3.  Selezionare **questo server richiede una connessione protetta (SSL)** se il server SMTP Usa SSL (vedere la tabella 1).  
  
        4.  Selezionare **questo server richiede l'autenticazione** se il server SMTP necessita di informazioni su nome e una password utente (vedere la tabella 1). Se si seleziona questa casella di controllo, digitare il nome utente e la password dell'indirizzo di posta elettronica immesso nel **da indirizzo di posta elettronica** campo nel passaggio 4a, quindi fare clic su **OK**.  
  
            > [!NOTE]
            >  È possibile ottenere le informazioni su nome server SMTP, numero di porta e l'uso di SSL dal provider di servizi Internet.  
  
            > [!NOTE]
            >  Microsoft consiglia di usare SSL, poiché il rapporto potrebbe contenere lo stato del server che potrebbe essere usato da parti non autorizzate per rilevare vulnerabilità (ad esempio: aggiornamenti di windows mancanti). Abilitazione di SSL verrà crittografare i dati in transito e ridurre il rischio di esposizione di vulnerabilità del server.  
  
5.  Dopo aver abilitato la posta elettronica, la **Modifica impostazioni SMTP** collegamento viene visualizzato. Anche la nuova attività, **il rapporto di stato di posta elettronica**, sarà visualizzata in **attività dei rapporti di integrità**.  
  
     **Tabella 1** i nomi dei server esempi del protocollo SMTP, autenticazione e i requisiti di crittografia SSL e numeri di porta  
  
    |Server SMTP|SSL necessario|Autenticazione necessaria|Numero di porta|Nome account/nome|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Sì|Sì|587|Fornire l'indirizzo di posta elettronica completo con nome di dominio e la password per l'autenticazione.|  
    |SMTP.Live.com|Sì|Sì|587|Fornire l'indirizzo di posta elettronica completo con nome di dominio e la password per l'autenticazione.|  
    |SMTP.Comcast.NET|Sì|No|587|Fornire l'indirizzo di posta elettronica completo con nome di dominio e la password per l'autenticazione.|  
    |SMTP.Mail.Yahoo.com|No|Sì|25|Fornire solo l'indirizzo di posta elettronica senza un nome di dominio per il nome utente.|  
  
6.  In **Personalizza le impostazioni del rapporto di integrità**, per **invia automaticamente rapporto di stato per i destinatari di posta elettronica:**, digitare gli indirizzi di posta elettronica delle persone che vuoi ricevere rapporti di stato tramite posta elettronica. Assicurarsi di separare ogni indirizzo di posta elettronica con un punto e virgola.  
  
7.  Per verificare che il server SMTP è stato configurato le impostazioni correttamente per inviare i rapporti di stato tramite posta elettronica, nella scheda rapporto nel Dashboard, seleziona un report e fare clic su **il rapporto di stato di posta elettronica** dal riquadro attività.  
  
##  <a name="BKMK_Potential"></a>Avvisi potenziali del computer  
 In questa sezione permette la comprensione e la gestione degli avvisi che sono specifici del computer in cui è connesso al server e che vengono visualizzati nella finestra di avvio del computer.  
  
 Nella tabella seguente sono elencati alcuni avvisi del computer che possono essere generati e visualizzati nel Visualizzatore avvisi, se sono applicabili al computer.  
  
|Titolo dell'avviso|Impatto e risoluzione dell'avviso|  
|-----------------|---------------------------------|  
|Lo stato corrente del Firewall di rete offre protezione ridotta per questo computer.|Gli utenti non autorizzati o software potrebbe essere in grado di accedere al computer se non è attivato Windows Firewall.|  
|Protezione da virus è disattivata, non installata o non aggiornata.|I dati nel computer sono a rischio se il **protezione da Virus** impostazione di protezione è disattivata o non aggiornato. [Per proteggere il computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), seguire i passaggi indicati.|  
|Spyware e software indesiderato è disattivato, non installata o non aggiornata.|I dati nel computer sono a rischio se il **Spyware e protezione da software indesiderato** è disattivata o non aggiornato. [Per proteggere il computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), seguire i passaggi indicati.|  
|Windows Update è disattivato.|Non sarà in grado di trarre vantaggio dalle funzionalità nuove e corrette degli aggiornamenti, a meno che non è attivato Windows Update. Per attivare Windows Update, nel Visualizzatore avvisi fare clic su **Apri Windows Update**.<br /><br /> Se il **Apri Windows Update** attività non viene visualizzata, non si è connessi al computer in cui è stato generato l'avviso. È necessario essere connessi al computer in cui è stato generato l'avviso per eseguire questa attività nel Visualizzatore avvisi.|  
|È necessario installare gli aggiornamenti importanti.|Non sarà in grado di trarre vantaggio dalle funzionalità nuove e corrette degli aggiornamenti, a meno che non è attivato Windows Update. Per attivare Windows Update, nel Visualizzatore avvisi fare clic su **Apri Windows Update**.<br /><br /> Se il **Apri Windows Update** attività non viene visualizzata, non si è connessi al computer in cui è stato generato l'avviso. È necessario essere connessi al computer in cui è stato generato l'avviso per eseguire questa attività nel Visualizzatore avvisi.|  
|Riavviare il computer per applicare gli aggiornamenti.|Non sarà possibile usufruire finché vengono applicate le funzionalità nuove e corrette degli aggiornamenti. Salvare tutti i dati e riavviare il computer per applicare gli aggiornamenti.|  
|Spazio disponibile è sufficiente nell'unità disco rigido.|Se non è disponibile spazio, non sarà in grado di salvare informazioni aggiuntive. Per aumentare lo spazio disponibile nel computer, considerare le opzioni seguenti:<br /><br /> -Aggiungere un nuovo disco rigido.<br /><br /> -Eseguire **pulizia disco** per rimuovere i file obsoleti e temporanei.<br /><br /> -Spostare i file in una cartella condivisa in un altro computer.<br /><br /> -File di archivio supporti rimovibili, ad esempio un CD, DVD o un'unità disco rigido esterna.|  
|Il **cronologia File** agente nel server non è configurato correttamente per l'esecuzione nel computer.|Impossibile creare il backup di cronologia file.|  
|Uno o più servizi non sono in esecuzione.||  
|Modificare la password di Windows.||  
|La password di Microsoft Office 365 non corrisponde alla password di Windows.||  
  
###  <a name="BKMK_Protect"></a>Per proteggere il computer  
  
1.  Aprire Centro sicurezza PC.  
  
2.  Determinare lo stato della protezione da virus installata.  
  
3.  Eseguire una delle operazioni seguenti a seconda dello stato di protezione:  
  
    -   Se non è abilitato, è necessario abilitarla.  
  
    -   Se non è aggiornato, completare il processo per aggiornare le firme.  
  
    -   Se la protezione da virus non è installata, considerarne l'installazione.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
